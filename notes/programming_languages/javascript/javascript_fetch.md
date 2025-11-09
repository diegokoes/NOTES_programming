# javascript > fetch

## SUMMARY
>
> [!summary]
>The Fetch API is provided from the web platform (browser), for making HTTP requests and processing responses, being a modern replacement for [[xmlhttprequest]].
>
## THEORY

### fetch()

Accepts:

 1. Resource to fetch (url/object,request instance)
 2. Object to configure the request (GET by default), Headers, Timeout...

Returns a [[javascript_promises|promise]].

Request Response Headers

Two ways:

>1. `async / await`

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}
```

>2. `.then / .catch`

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```
