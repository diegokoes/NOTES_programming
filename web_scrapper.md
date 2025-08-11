# Web Scraper Project Analysis & Improvement Recommendations

## 📋 **Project Overview**

Your web scraping project is well-structured with clear separation of concerns. The architecture includes:
- **Multiple scrapers** (Literotica, DarkWanderer) with a base class
- **Dedicated processors** for markdown conversion
- **Storage management** with file operations and image downloading
- **Content indexing** for Obsidian-style note linking
- **Utility modules** for HTML parsing, rate limiting, and URL handling

## 🔧 **Critical Issues Requiring Immediate Attention**

### 1. **Code Duplication & Inconsistency**
**Problem:** Multiple `to_markdown()` implementations across scrapers with different logic.

```python
# LiteroticaScraper has to_markdown() at lines 1635, 1712, 2059, 2104
# DarkWandererScraper has to_markdown() at line 763
# MarkdownConverter has convert_story(), convert_series(), convert_author()
```

**Impact:** Maintenance nightmare, inconsistent output formats, bug multiplication.

**Solution:**
```python
# Remove all to_markdown() methods from scrapers
# Standardize on MarkdownConverter only

class BaseScraper:
    def __init__(self, config_path: str):
        self.markdown_converter = MarkdownConverter(config_path)
    
    def convert_to_markdown(self, data: Dict[str, Any], content_type: str) -> str:
        """Single entry point for all markdown conversion"""
        if content_type == "story":
            return self.markdown_converter.convert_story(data)
        elif content_type == "series":
            return self.markdown_converter.convert_series(data)
        elif content_type == "author":
            return self.markdown_converter.convert_author(data)
```

### 2. **Fragile YAML Handling**
**Problem:** Multiple YAML sanitization attempts across different files indicate ongoing parsing issues.

```python
# ContentIndexer._sanitize_yaml_content() - Complex regex-based fixes
# MarkdownConverter.yaml_safe_string() - Another sanitization layer
# FileManager reading YAML with manual parsing fallbacks
```

**Solution:** Implement robust YAML handling with proper data models:

```python
from pydantic import BaseModel, Field
from typing import List, Optional

class StoryMetadata(BaseModel):
    title: str = Field(..., description="Story title")
    author: str = Field(..., description="Author name")
    tags: List[str] = Field(default_factory=list)
    completed: bool = Field(default=False)
    rating: float = Field(default=0.0, ge=0, le=5)
    source: str = Field(..., description="Source URL")
    
    def to_yaml_dict(self) -> Dict[str, Any]:
        """Convert to YAML-safe dictionary"""
        return {
            "title": self._escape_yaml_string(self.title),
            "author": self._escape_yaml_string(self.author),
            "tags": self.tags,
            "completed": self.completed,
            "rating": self.rating,
            "source": self.source
        }
```

### 3. **Incomplete Error Handling**
**Problem:** Many methods have incomplete error handling blocks:

```python
# From literotica.py - lines with incomplete implementations
if not title_container:
    # Missing error handling/logging

processed_urls.add(link)  # No validation if link exists
```

**Solution:** Implement comprehensive error handling strategy:

```python
from contextlib import contextmanager
import traceback

@contextmanager
def safe_operation(operation_name: str, logger: logging.Logger):
    """Context manager for safe operations with consistent error handling"""
    try:
        yield
    except Exception as e:
        logger.error(f"Error in {operation_name}: {str(e)}")
        logger.debug(f"Traceback: {traceback.format_exc()}")
        raise  # Re-raise if needed for caller to handle
```

## 🏗️ **Architecture Improvements**

### 4. **Extract Content Processing Pipeline**
**Current Issue:** Content processing logic scattered across scrapers and converters.

**Proposed Architecture:**
```python
class ContentProcessor:
    """Central content processing pipeline"""
    
    def __init__(self, config_path: str):
        self.html_parser = HTMLParser()
        self.markdown_converter = MarkdownConverter(config_path)
        self.content_validator = ContentValidator()
    
    def process_content(self, raw_data: Dict, content_type: str) -> ProcessedContent:
        """Single pipeline for all content processing"""
        # 1. Validate raw data
        validated_data = self.content_validator.validate(raw_data, content_type)
        
        # 2. Clean and normalize
        cleaned_data = self._clean_content(validated_data)
        
        # 3. Convert to markdown
        markdown_content = self.markdown_converter.convert(cleaned_data, content_type)
        
        # 4. Post-process (image handling, link resolution, etc.)
        final_content = self._post_process(markdown_content)
        
        return ProcessedContent(content=final_content, metadata=cleaned_data)
```

### 5. **Implement Repository Pattern for Data Access**
**Problem:** File operations mixed with business logic throughout codebase.

**Solution:**
```python
from abc import ABC, abstractmethod

class ContentRepository(ABC):
    @abstractmethod
    def save_story(self, story: ProcessedContent) -> bool: pass
    
    @abstractmethod
    def find_duplicate(self, title: str, author: str) -> Optional[str]: pass
    
    @abstractmethod
    def get_author_stories(self, author: str) -> List[ProcessedContent]: pass

class FileSystemRepository(ContentRepository):
    """File-based implementation"""
    def __init__(self, config: dict):
        self.file_manager = FileManager(config)
    
    def save_story(self, story: ProcessedContent) -> bool:
        return self.file_manager.save_content(story.content, story.metadata)

class DatabaseRepository(ContentRepository):
    """Future database implementation"""
    pass
```

### 6. **Service Layer Implementation**
**Problem:** Scrapers handle too many responsibilities.

**Solution:**
```python
class ScrapingService:
    """High-level scraping orchestration"""
    
    def __init__(self, scraper: BaseScraper, processor: ContentProcessor, 
                 repository: ContentRepository):
        self.scraper = scraper
        self.processor = processor
        self.repository = repository
    
    async def scrape_and_save(self, url: str) -> ScrapingResult:
        """Complete scraping workflow"""
        # 1. Determine content type
        content_type = self.scraper.identify_content_type(url)
        
        # 2. Scrape raw data
        raw_data = await self.scraper.scrape(url)
        
        # 3. Process content
        processed_content = self.processor.process_content(raw_data, content_type)
        
        # 4. Check for duplicates
        if self.repository.find_duplicate(processed_content.title, processed_content.author):
            return ScrapingResult.DUPLICATE
        
        # 5. Save content
        success = self.repository.save_story(processed_content)
        
        # 6. Update indexes
        self.indexer.update_indexes(processed_content)
        
        return ScrapingResult.SUCCESS if success else ScrapingResult.FAILED
```

## 🚀 **Performance Optimizations**

### 7. **Implement Proper Async Architecture**
**Problem:** Mixing sync/async operations inefficiently.

**Current Issue:**
```python
# Anti-pattern in literotica.py
playwright_html = self._run_playwright_sync(url)  # Sync wrapper around async
```

**Solution:**
```python
class AsyncScrapingService:
    async def scrape_multiple_urls(self, urls: List[str]) -> List[ScrapingResult]:
        """Concurrent scraping with proper async/await"""
        semaphore = asyncio.Semaphore(5)  # Limit concurrent requests
        
        async def scrape_with_semaphore(url: str) -> ScrapingResult:
            async with semaphore:
                await self.rate_limiter.wait_async()
                return await self.scrape_and_save(url)
        
        tasks = [scrape_with_semaphore(url) for url in urls]
        return await asyncio.gather(*tasks, return_exceptions=True)
```

### 8. **Add Caching Layer**
**Problem:** No caching of HTTP responses or parsed content.

**Solution:**
```python
import aioredis
from functools import wraps

class CacheService:
    def __init__(self, redis_url: str = "redis://localhost"):
        self.redis = aioredis.from_url(redis_url)
    
    async def get_cached_content(self, url: str) -> Optional[str]:
        return await self.redis.get(f"content:{url}")
    
    async def cache_content(self, url: str, content: str, ttl: int = 3600):
        await self.redis.setex(f"content:{url}", ttl, content)

def cached_fetch(cache_service: CacheService):
    def decorator(func):
        @wraps(func)
        async def wrapper(self, url: str):
            # Try cache first
            cached = await cache_service.get_cached_content(url)
            if cached:
                return cached
            
            # Fetch and cache
            content = await func(self, url)
            await cache_service.cache_content(url, content)
            return content
        return wrapper
    return decorator
```

### 9. **Optimize File I/O Operations**
**Problem:** Synchronous file operations blocking execution.

**Solution:**
```python
import aiofiles
from pathlib import Path

class AsyncFileManager:
    async def save_content_async(self, content: str, filepath: Path) -> bool:
        try:
            async with aiofiles.open(filepath, 'w', encoding='utf-8') as f:
                await f.write(content)
            return True
        except Exception as e:
            self.logger.error(f"Failed to save {filepath}: {e}")
            return False
    
    async def batch_save(self, content_list: List[Tuple[str, Path]]) -> List[bool]:
        """Save multiple files concurrently"""
        tasks = [self.save_content_async(content, path) for content, path in content_list]
        return await asyncio.gather(*tasks, return_exceptions=True)
```

## 🛡️ **Reliability Improvements**

### 10. **Implement Circuit Breaker Pattern**
**Problem:** No protection against cascading failures.

**Solution:**
```python
import time
from enum import Enum

class CircuitBreakerState(Enum):
    CLOSED = "closed"
    OPEN = "open"
    HALF_OPEN = "half_open"

class CircuitBreaker:
    def __init__(self, failure_threshold: int = 5, timeout: int = 60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = CircuitBreakerState.CLOSED
    
    async def call(self, func, *args, **kwargs):
        if self.state == CircuitBreakerState.OPEN:
            if time.time() - self.last_failure_time > self.timeout:
                self.state = CircuitBreakerState.HALF_OPEN
            else:
                raise Exception("Circuit breaker is OPEN")
        
        try:
            result = await func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise e
```

### 11. **Add Comprehensive Retry Logic**
**Problem:** No retry mechanism for failed operations.

**Solution:**
```python
from tenacity import retry, stop_after_attempt, wait_exponential, retry_if_exception_type

class NetworkError(Exception):
    pass

class ScrapingService:
    @retry(
        stop=stop_after_attempt(3),
        wait=wait_exponential(multiplier=1, min=4, max=10),
        retry=retry_if_exception_type(NetworkError)
    )
    async def fetch_with_retry(self, url: str) -> str:
        try:
            response = await self.session.get(url)
            response.raise_for_status()
            return await response.text()
        except aiohttp.ClientError as e:
            raise NetworkError(f"Failed to fetch {url}: {e}")
```

### 12. **Implement Health Checks**
**Problem:** No visibility into system health.

**Solution:**
```python
from dataclasses import dataclass
from typing import Dict

@dataclass
class HealthStatus:
    service: str
    status: str
    details: Dict[str, any]
    timestamp: float

class HealthChecker:
    async def check_all(self) -> Dict[str, HealthStatus]:
        """Check health of all components"""
        checks = {
            "rate_limiter": self._check_rate_limiter(),
            "file_system": self._check_file_system(),
            "network": self._check_network(),
            "cache": self._check_cache()
        }
        
        results = {}
        for name, check in checks.items():
            results[name] = await check
        
        return results
```

## 📊 **Data Management Enhancements**

### 13. **Implement Data Models with Validation**
**Problem:** Raw dictionaries used throughout without validation.

**Solution:**
```python
from pydantic import BaseModel, validator, HttpUrl
from typing import List, Optional
from datetime import datetime

class StoryContent(BaseModel):
    title: str
    author: str
    content: List[str]
    url: HttpUrl
    tags: List[str] = []
    completed: bool = False
    rating: float = 0.0
    scraped_at: datetime = datetime.now()
    
    @validator('title', 'author')
    def must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('must not be empty')
        return v.strip()
    
    @validator('rating')
    def rating_must_be_valid(cls, v):
        if not 0 <= v <= 5:
            raise ValueError('rating must be between 0 and 5')
        return v

class SeriesContent(BaseModel):
    title: str
    author: str
    stories: List[StoryContent]
    url: HttpUrl
    completed: bool = False
    total_stories: int = 0
    
    @validator('total_stories')
    def validate_story_count(cls, v, values):
        if 'stories' in values and len(values['stories']) != v:
            raise ValueError('total_stories must match stories list length')
        return v
```

### 14. **Add Database Support**
**Problem:** File-only storage limits querying and performance.

**Solution:**
```python
from sqlalchemy import create_engine, Column, Integer, String, Text, Boolean, Float, DateTime
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()

class Story(Base):
    __tablename__ = 'stories'
    
    id = Column(Integer, primary_key=True)
    title = Column(String(500), nullable=False)
    author = Column(String(200), nullable=False)
    url = Column(String(1000), unique=True, nullable=False)
    content_hash = Column(String(64), nullable=False)  # For duplicate detection
    file_path = Column(String(1000), nullable=False)
    completed = Column(Boolean, default=False)
    rating = Column(Float, default=0.0)
    scraped_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)

class DatabaseRepository(ContentRepository):
    def __init__(self, database_url: str):
        self.engine = create_engine(database_url)
        self.Session = sessionmaker(bind=self.engine)
        Base.metadata.create_all(self.engine)
    
    def find_duplicate(self, title: str, author: str) -> Optional[str]:
        with self.Session() as session:
            story = session.query(Story).filter(
                Story.title == title,
                Story.author == author
            ).first()
            return story.file_path if story else None
```

## 🧪 **Testing Strategy**

### 15. **Implement Comprehensive Test Suite**
**Problem:** No tests visible in the project.

**Solution:**
```python
# tests/test_scrapers.py
import pytest
from unittest.mock import Mock, patch, AsyncMock
from src.scrapers.literotica import LiteroticaScraper

class TestLiteroticaScraper:
    @pytest.fixture
    def scraper(self):
        return LiteroticaScraper("tests/fixtures/test_config.json")
    
    @pytest.fixture
    def mock_html(self):
        with open("tests/fixtures/literotica_story.html", 'r') as f:
            return f.read()
    
    @patch('src.scrapers.literotica.requests.get')
    def test_scrape_story_success(self, mock_get, scraper, mock_html):
        # Setup
        mock_response = Mock()
        mock_response.text = mock_html
        mock_response.status_code = 200
        mock_get.return_value = mock_response
        
        # Execute
        result = scraper.scrape_story("https://example.com/story")
        
        # Assert
        assert result['title'] == "Expected Title"
        assert result['author'] == "Expected Author"
        assert len(result['content']) > 0
    
    @patch('src.scrapers.base_scraper.requests.get')
    def test_network_error_handling(self, mock_get, scraper):
        # Setup
        mock_get.side_effect = requests.exceptions.RequestException("Network error")
        
        # Execute & Assert
        with pytest.raises(NetworkError):
            scraper.scrape_story("https://example.com/story")

# tests/test_content_processor.py
class TestContentProcessor:
    def test_markdown_conversion(self):
        # Test data
        story_data = {
            'title': 'Test Story',
            'author': 'Test Author',
            'content': ['Paragraph 1', 'Paragraph 2'],
            'tags': ['romance', 'drama']
        }
        
        processor = ContentProcessor("tests/fixtures/test_config.json")
        result = processor.process_content(story_data, "story")
        
        assert "# Test Story" in result.content
        assert "author: Test Author" in result.content
        assert "Paragraph 1" in result.content
```

## 🔒 **Security & Ethics**

### 16. **Implement Ethical Scraping Controls**
**Problem:** No robots.txt checking or ethical guidelines enforcement.

**Solution:**
```python
import urllib.robotparser
from datetime import datetime, timedelta

class EthicalScrapingManager:
    def __init__(self, user_agent: str = "ResponsibleBot/1.0"):
        self.user_agent = user_agent
        self.robots_cache = {}
    
    async def can_fetch(self, url: str) -> bool:
        """Check if we can fetch this URL according to robots.txt"""
        domain = urlparse(url).netloc
        
        if domain not in self.robots_cache:
            await self._load_robots_txt(domain)
        
        robots = self.robots_cache.get(domain)
        if robots:
            return robots.can_fetch(self.user_agent, url)
        
        return True  # If no robots.txt, assume allowed
    
    async def _load_robots_txt(self, domain: str):
        """Load and cache robots.txt for domain"""
        try:
            robots = urllib.robotparser.RobotFileParser()
            robots.set_url(f"https://{domain}/robots.txt")
            robots.read()
            self.robots_cache[domain] = robots
        except Exception as e:
            self.logger.warning(f"Could not load robots.txt for {domain}: {e}")
            self.robots_cache[domain] = None
```

## 📁 **Recommended Project Structure**

```
src/
├── config/
│   ├── __init__.py
│   └── settings.py              # Centralized configuration
├── core/
│   ├── __init__.py
│   ├── models.py               # Pydantic data models
│   ├── exceptions.py           # Custom exceptions
│   └── interfaces.py           # Abstract base classes
├── scrapers/
│   ├── __init__.py
│   ├── base.py                 # Base scraper with common functionality
│   ├── literotica.py           # Site-specific scraper
│   └── darkwanderer.py         # Site-specific scraper
├── services/
│   ├── __init__.py
│   ├── scraping_service.py     # High-level scraping orchestration
│   ├── content_processor.py    # Content processing pipeline
│   └── health_checker.py       # System health monitoring
├── repositories/
│   ├── __init__.py
│   ├── base.py                 # Abstract repository
│   ├── filesystem.py           # File-based storage
│   └── database.py             # Database storage (future)
├── utils/
│   ├── __init__.py
│   ├── rate_limiter.py         # (Keep existing)
│   ├── circuit_breaker.py      # Reliability patterns
│   ├── cache_service.py        # Caching abstraction
│   └── ethical_manager.py      # Ethical scraping controls
└── tests/
    ├── unit/
    ├── integration/
    └── fixtures/
```


# Site-Specific Configuration & Scraper Architecture

Looking at your requirements and the different site behaviors you mentioned, here's how I'd architect a flexible, site-specific scraping system:

## 🏗️ **1. Site Configuration Strategy**

### **Hierarchical Configuration System**
```python
# config/sites/literotica.json
{
    "site_name": "literotica",
    "base_url": "https://www.literotica.com",
    "features": {
        "has_series": true,
        "has_authors": true,
        "has_categories": true,
        "pagination_type": "load_more_button",
        "requires_javascript": true
    },
    "selectors": {
        "story": {
            "title": "h1.headline",
            "author": ".by-line a",
            "content": ".story-text p",
            "rating": ".rating-value",
            "tags": ".story-tags a"
        },
        "series": {
            "title": ".series-title",
            "stories": ".series-story-item",
            "story_link": ".story-link",
            "completed_indicator": ".completed-badge"
        },
        "author": {
            "name": ".author-name",
            "works_container": "[class*='_works_wrapper_']",
            "story_items": "[class*='_works_item_']",
            "load_more": "[class*='_load_more_']"
        }
    },
    "url_patterns": {
        "story": r"^https?://(?:www\.)?literotica\.com/s/[\w-]+$",
        "series": r"^https?://(?:www\.)?literotica\.com/series/[\w-]+$",
        "author": r"^https?://(?:www\.)?literotica\.com/authors/[^/]+/?(?:works/?(?:stories)?)?$"
    },
    "scraping_behavior": {
        "rate_limit_ms": 2000,
        "use_playwright": true,
        "wait_for_selector": ".story-text",
        "scroll_to_load": true
    }
}
```

```python
# config/sites/darkwanderer.json
{
    "site_name": "darkwanderer",
    "base_url": "https://www.darkwanderer.net",
    "features": {
        "has_series": false,           # Key difference
        "has_authors": true,
        "has_categories": false,
        "pagination_type": "standard",
        "requires_javascript": false
    },
    "selectors": {
        "story": {
            "title": "h1.story-title",
            "author": ".author-link",
            "content": ".story-content p",
            "rating": null,
            "tags": null
        },
        "author": {
            "name": ".author-header h1",
            "story_list": ".story-list",
            "story_items": ".story-item"
        }
    }
}
```

```python
# config/sites/ao3.json
{
    "site_name": "ao3",
    "base_url": "https://archiveofourown.org",
    "features": {
        "has_series": true,
        "series_type": "collection_based",  # Different from literotica
        "has_authors": true,
        "has_categories": true,
        "pagination_type": "numbered",
        "requires_authentication": false
    },
    "selectors": {
        "series": {
            "title": ".series-header h2",
            "works": ".series .work",
            "work_title": ".work h4 a",
            "work_authors": ".work .byline a",
            "series_notes": ".series .notes"
        }
    },
    "scraping_behavior": {
        "rate_limit_ms": 5000,  # Be extra respectful to AO3
        "respect_user_preferences": true
    }
}
```

## 🔧 **2. Flexible Scraper Architecture**

### **Site-Aware Base Scraper**
```python
from abc import ABC, abstractmethod
from typing import Dict, Any, List, Optional, Type
import importlib
import json

class SiteConfig:
    """Encapsulates site-specific configuration"""
    
    def __init__(self, config_path: str):
        with open(config_path, 'r') as f:
            self.config = json.load(f)
        
        self.site_name = self.config['site_name']
        self.features = self.config['features']
        self.selectors = self.config['selectors']
        self.url_patterns = self.config['url_patterns']
        self.behavior = self.config.get('scraping_behavior', {})
    
    def has_feature(self, feature: str) -> bool:
        return self.features.get(feature, False)
    
    def get_selector(self, content_type: str, element: str) -> Optional[str]:
        return self.selectors.get(content_type, {}).get(element)
    
    def supports_content_type(self, content_type: str) -> bool:
        return content_type in self.selectors

class BaseScraper(ABC):
    """Base scraper with site-specific configuration support"""
    
    def __init__(self, site_config_path: str):
        self.site_config = SiteConfig(site_config_path)
        self.rate_limiter = RateLimiter(
            pause_ms=self.site_config.behavior.get('rate_limit_ms', 1000)
        )
        
        # Initialize components based on site requirements
        self._init_components()
    
    def _init_components(self):
        """Initialize components based on site configuration"""
        if self.site_config.behavior.get('use_playwright', False):
            self.browser_manager = PlaywrightManager()
        else:
            self.session = requests.Session()
    
    @abstractmethod
    def scrape_story(self, url: str) -> Dict[str, Any]:
        pass
    
    def scrape_series(self, url: str) -> Dict[str, Any]:
        """Only implemented if site supports series"""
        if not self.site_config.has_feature('has_series'):
            raise NotImplementedError(f"{self.site_config.site_name} doesn't support series")
        return self._scrape_series_impl(url)
    
    @abstractmethod
    def _scrape_series_impl(self, url: str) -> Dict[str, Any]:
        pass
    
    def identify_content_type(self, url: str) -> str:
        """Identify content type based on site-specific URL patterns"""
        for content_type, pattern in self.site_config.url_patterns.items():
            if re.match(pattern, url):
                return content_type
        raise ValueError(f"Unknown URL pattern for {url}")
```

### **Site-Specific Scraper Implementations**
```python
class LiteroticaScraper(BaseScraper):
    """Literotica-specific scraping logic"""
    
    def scrape_story(self, url: str) -> Dict[str, Any]:
        # Use site config for selectors
        title_selector = self.site_config.get_selector('story', 'title')
        author_selector = self.site_config.get_selector('story', 'author')
        content_selector = self.site_config.get_selector('story', 'content')
        
        if self.site_config.behavior.get('use_playwright'):
            html = self._fetch_with_playwright(url)
        else:
            html = self._fetch_with_requests(url)
        
        soup = BeautifulSoup(html, 'html.parser')
        
        return {
            'title': self._extract_text(soup, title_selector),
            'author': self._extract_text(soup, author_selector),
            'content': self._extract_content(soup, content_selector),
            'site': self.site_config.site_name
        }
    
    def _scrape_series_impl(self, url: str) -> Dict[str, Any]:
        """Literotica-specific series scraping"""
        series_title_selector = self.site_config.get_selector('series', 'title')
        stories_selector = self.site_config.get_selector('series', 'stories')
        
        # Literotica-specific logic for series
        # Handle dynamic loading, pagination, etc.
        return self._handle_literotica_series(url)

class DarkWandererScraper(BaseScraper):
    """DarkWanderer-specific scraping logic"""
    
    def scrape_story(self, url: str) -> Dict[str, Any]:
        # DarkWanderer doesn't need JavaScript
        html = self._fetch_with_requests(url)
        soup = BeautifulSoup(html, 'html.parser')
        
        title_selector = self.site_config.get_selector('story', 'title')
        author_selector = self.site_config.get_selector('story', 'author')
        
        return {
            'title': self._extract_text(soup, title_selector),
            'author': self._extract_text(soup, author_selector),
            'content': self._extract_content(soup, content_selector),
            'site': self.site_config.site_name,
            'rating': None,  # DarkWanderer doesn't have ratings
            'tags': []       # DarkWanderer doesn't have tags
        }
    
    def _scrape_series_impl(self, url: str) -> Dict[str, Any]:
        # This will never be called due to site config
        raise NotImplementedError("DarkWanderer doesn't support series")

class AO3Scraper(BaseScraper):
    """Archive of Our Own specific scraping logic"""
    
    def _scrape_series_impl(self, url: str) -> Dict[str, Any]:
        """AO3-specific series handling - very different from Literotica"""
        
        # AO3 series are collection-based, not sequential
        if self.site_config.features.get('series_type') == 'collection_based':
            return self._handle_ao3_collection_series(url)
        else:
            return self._handle_standard_series(url)
    
    def _handle_ao3_collection_series(self, url: str) -> Dict[str, Any]:
        """Handle AO3's unique series structure"""
        # AO3 series can have:
        # - Multiple authors per work
        # - Works in different fandoms
        # - Complex tagging systems
        # - User-defined reading order
        pass
```

## 🎯 **3. Scraper Factory & Registry**

### **Dynamic Scraper Selection**
```python
class ScraperRegistry:
    """Registry for site-specific scrapers"""
    
    def __init__(self):
        self._scrapers: Dict[str, Type[BaseScraper]] = {}
        self._site_configs: Dict[str, str] = {}  # domain -> config_path
    
    def register_scraper(self, domain: str, scraper_class: Type[BaseScraper], 
                        config_path: str):
        """Register a scraper for a specific domain"""
        self._scrapers[domain] = scraper_class
        self._site_configs[domain] = config_path
    
    def get_scraper(self, url: str) -> BaseScraper:
        """Get appropriate scraper for URL"""
        domain = urlparse(url).netloc.replace('www.', '')
        
        if domain not in self._scrapers:
            raise ValueError(f"No scraper registered for domain: {domain}")
        
        scraper_class = self._scrapers[domain]
        config_path = self._site_configs[domain]
        
        return scraper_class(config_path)

# Initialize registry
registry = ScraperRegistry()
registry.register_scraper('literotica.com', LiteroticaScraper, 'config/sites/literotica.json')
registry.register_scraper('darkwanderer.net', DarkWandererScraper, 'config/sites/darkwanderer.json')
registry.register_scraper('archiveofourown.org', AO3Scraper, 'config/sites/ao3.json')

class ScrapingService:
    """High-level scraping service"""
    
    def __init__(self, registry: ScraperRegistry):
        self.registry = registry
    
    async def scrape_url(self, url: str) -> Dict[str, Any]:
        """Automatically select and use appropriate scraper"""
        scraper = self.registry.get_scraper(url)
        content_type = scraper.identify_content_type(url)
        
        if content_type == 'story':
            return scraper.scrape_story(url)
        elif content_type == 'series':
            return scraper.scrape_series(url)
        elif content_type == 'author':
            return scraper.scrape_author(url)
        
        raise ValueError(f"Unknown content type: {content_type}")
```

## 🔄 **4. Content Processing Pipeline**

### **Site-Aware Content Processing**
```python
class ContentProcessor:
    """Process content based on site-specific rules"""
    
    def __init__(self):
        self.markdown_converter = MarkdownConverter()
    
    def process_content(self, raw_data: Dict[str, Any], site_config: SiteConfig) -> ProcessedContent:
        """Process content with site-specific rules"""
        
        # Apply site-specific normalization
        normalized_data = self._normalize_data(raw_data, site_config)
        
        # Handle site-specific content types
        if site_config.has_feature('has_series') and 'series_data' in normalized_data:
            processed_series = self._process_series(normalized_data['series_data'], site_config)
            normalized_data.update(processed_series)
        
        # Convert to markdown with site-specific templates
        markdown_content = self._convert_to_markdown(normalized_data, site_config)
        
        return ProcessedContent(
            content=markdown_content,
            metadata=normalized_data,
            site=site_config.site_name
        )
    
    def _normalize_data(self, data: Dict[str, Any], site_config: SiteConfig) -> Dict[str, Any]:
        """Normalize data based on site capabilities"""
        normalized = data.copy()
        
        # Handle missing features
        if not site_config.has_feature('has_series'):
            normalized.pop('series_data', None)
        
        if not site_config.get_selector('story', 'rating'):
            normalized.setdefault('rating', None)
        
        if not site_config.get_selector('story', 'tags'):
            normalized.setdefault('tags', [])
        
        return normalized
    
    def _process_series(self, series_data: Dict[str, Any], site_config: SiteConfig) -> Dict[str, Any]:
        """Process series data based on site type"""
        
        if site_config.features.get('series_type') == 'collection_based':
            # AO3-style series processing
            return self._process_collection_series(series_data)
        else:
            # Literotica-style sequential series
            return self._process_sequential_series(series_data)
```

## 📝 **5. Site-Specific Templates**

### **Flexible Markdown Templates**
```python
# templates/literotica_story.md
---
title: "{{ title }}"
author: "{{ author }}"
site: "{{ site }}"
url: "{{ url }}"
completed: {{ completed }}
rating: {{ rating }}
tags: {{ tags | join(', ') }}
---

# {{ title }}

**Author:** {{ author }}
{% if rating %}**Rating:** {{ rating }}/5⭐{% endif %}
{% if tags %}**Tags:** {{ tags | join(', ') }}{% endif %}

{{ content | join('\n\n') }}

---
*Source: [{{ title }}]({{ url }})*
```

```python
# templates/ao3_series.md
---
title: "{{ title }}"
series_type: "collection"
works_count: {{ works | length }}
authors: {{ authors | unique | join(', ') }}
fandoms: {{ fandoms | unique | join(', ') }}
---

# {{ title }} (Series)

**Type:** Collection Series
**Works:** {{ works | length }}
**Authors:** {{ authors | unique | join(', ') }}
**Fandoms:** {{ fandoms | unique | join(', ') }}

## Works in Series

{% for work in works %}
### {{ work.title }}
- **Author(s):** {{ work.authors | join(', ') }}
- **Fandom:** {{ work.fandom }}
- **Tags:** {{ work.tags | join(', ') }}
- **Link:** [Read here]({{ work.url }})

{{ work.summary }}

---
{% endfor %}
```

## 🔧 **6. Configuration Management**

### **Updated requirements.txt**
```pip-requirements
# Web scraping
beautifulsoup4>=4.9.3
requests>=2.25.1
playwright>=1.40.0
aiohttp>=3.8.0

# Content processing
markdown>=3.3.4
pyyaml>=6.0
python-slugify>=5.0.2
jinja2>=3.0.0

# Data validation & models  
pydantic>=2.0.0

# Async & utilities
asyncio-throttle>=1.0.0
tenacity>=8.0.0

# Caching (optional)
redis>=4.0.0

# Database (optional)
sqlalchemy>=2.0.0
alembic>=1.8.0
```

## 🚀 **7. Usage Examples**

```python
# Simple usage
service = ScrapingService(registry)

# Automatically handles different sites
literotica_story = await service.scrape_url("https://literotica.com/s/story-name")
darkwanderer_story = await service.scrape_url("https://darkwanderer.net/story/123")
ao3_series = await service.scrape_url("https://archiveofourown.org/series/12345")

# Batch processing with mixed sites
urls = [
    "https://literotica.com/s/story1",
    "https://darkwanderer.net/story/456",
    "https://archiveofourown.org/works/789",
    "https://literotica.com/series/my-series"
]

results = await service.scrape_multiple_urls(urls)
```

This architecture provides:

- ✅ **Site-specific configuration** without code changes
- ✅ **Feature detection** (series support, JavaScript requirements, etc.)
- ✅ **Flexible selector management** with fallbacks
- ✅ **Easy addition of new sites** via configuration
- ✅ **Different content types** handled appropriately per site
- ✅ **Site-specific rate limiting** and behavior
- ✅ **Template-based output** for consistent formatting

The key insight is **configuration over code** - new sites require minimal code changes, mostly just configuration files and potentially some site-specific processing logic.
## 🎯 **Implementation Priority**

### **Phase 1: Foundation (Weeks 1-2)**
1. ✅ **Extract MarkdownConverter logic** - Remove duplicate `to_markdown()` methods
2. ✅ **Implement data models** - Add Pydantic models for type safety
3. ✅ **Complete error handling** - Fix all incomplete code blocks
4. ✅ **Add basic tests** - Cover core scraping logic

### **Phase 2: Architecture (Weeks 3-4)**
1. ✅ **Implement service layer** - Extract business logic from scrapers
2. ✅ **Add repository pattern** - Abstract data access
3. ✅ **Implement async architecture** - Remove sync/async mixing
4. ✅ **Add circuit breaker** - Improve reliability

### **Phase 3: Performance (Weeks 5-6)**
1. ✅ **Add caching layer** - Redis or in-memory caching
2. ✅ **Optimize file I/O** - Async file operations
3. ✅ **Implement batching** - Process multiple items concurrently
4. ✅ **Add monitoring** - Health checks and metrics

### **Phase 4: Polish (Weeks 7-8)**
1. ✅ **Database support** - Optional database backend
2. ✅ **Comprehensive testing** - Full test coverage
3. ✅ **Documentation** - API docs and usage guides
4. ✅ **Ethical controls** - robots.txt respect, rate limiting
