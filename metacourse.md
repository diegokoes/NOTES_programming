[[webhosting]]

tcp

udp

http


# OTHER INTERNET PROTOCOLS

Hypertext Transfer Protocols (HTTP) are used on top of Transmission Control Protocol (TCP) to transfer webpages and other content from websites. This reading explores other protocols commonly used on the Internet.

## DYNAMIC HOST CONFIGURATION PROTOCOL (DHCP)

You've learned that computers need IP addresses to communicate with each other. When your computer connects to a network, the Dynamic Host Configuration Protocol or DHCP as it is commonly known, is used to assign your computer an IP address. Your computer communicates over User Datagram Protocol (UDP) using the protocol with a type of server called a DHCP server. The server keeps track of computers on the network and their IP addresses. It will assign your computer an IP address and respond over the protocol to let it know which IP address to use. Once your computer has an IP address, it can communicate with other computers on the network.

## DOMAIN NAME SYSTEM PROTOCOL (DNS)

Your computer needs a way to know with which IP address to communicate when you visit a website in your web browser, for example, meta.com. The Domain Name System Protocol, commonly known as DNS, provides this function. Your computer then checks with the DNS server associated with the domain name and then returns the correct IP address.

## INTERNET MESSAGE ACCESS PROTOCOL (IMAP)

Do you check your emails on your mobile or tablet device? Or maybe you use an email application on your computer? Your device needs a way to download emails and manage your mailbox on the server storing your emails. This is the purpose of the Internet Message Access Protocol or IMAP.

## SIMPLE MAIL TRANSFER PROTOCOL (SMTP)

Now that your emails are on your device, you need a way to send emails. The Simple Mail Transfer Protocol, or SMTP, is used. It allows email clients to submit emails for sending via an SMTP server. You can also use it to receive emails from an email client, but IMAP is more commonly used.

## POST OFFICE PROTOCOL (POP)

The Post Office Protocol (POP) is an older protocol used to download emails to an email client. The main difference in using POP instead of IMAP is that POP will delete the emails on the server once they have been downloaded to your local device. Although it is no longer commonly used in email clients, developers often use it to implement email automation as it is a more straightforward protocol than IMAP.

## FILE TRANSFER PROTOCOL (FTP)

When running your websites and web applications on the Internet, you'll need a way to transfer the files from your local computer to the server they'll run on. The standard protocol used for this is the File Transfer Protocol or FTP. FTP allows you to list, send, receive and delete files on a server. Your server must run an FTP Server and you will need an FTP Client on your local machine. You'll learn more about these in a later course.

## SECURE SHELL PROTOCOL (SSH)

When you start working with servers, you'll also need a way to log in and interact with the computer remotely. The most common method of doing this is using the Secure Shell Protocol, commonly referred to as SSH. Using an SSH client allows you to connect to an SSH server running on a server to perform commands on the remote computer. All data sent over SSH is encrypted. This means that third parties cannot understand the data transmitted. Only the sending and receiving computers can understand the data.

## SECURE FILE TRANSFER PROTOCOL (SFTP)

The data is transmitted insecurely when using the File Transfer Protocol. This means that third parties may understand the data that you are sending. This is not right if you transmit company files such as software and databases. To solve this, the SSH File Transfer Protocol, alternatively called the Secure File Transfer Protocol, can be used to transfer files over the SSH protocol. This ensures that the data is transmitted securely. Most FTP clients also support the SFTP protocol.



Python is one of the best languages to use for various data science projects and applications. In this video, you'll learn about some of the commonly used Python libraries in data analysis and data science. The last decade has seen exponential growth in all data science areas. The demand for data analysis and scientists is continually increasing, as it's a requirement that developers to incorporate scientific and data analysis into their code. Python has emerged as one of the most popular languages with data scientists. One of the main reasons for its popularity is the large number of different open source packages. These have been developed by thousands of contributors collaborating to provide free, usable resources. 

Many packages are top-rated because they are efficient and provide outstanding functionality. In no particular order of preference, these packages include NumPy, Scipy, Matplotlib, and Scikit-learn. As an example, Scikit-learn is used for predictive learning, and is built on top of other popular packages. It consists of various supervised and unsupervised machine learning algorithms for classification, regression, and SVMs. Modelling data is the primary focus of this library, and it provides popular models such as clustering, feature extraction and selection, validation and dimensionality reduction. Pandas is an acronym for Python data analysis, and this is a data analysis and manipulation tool. It's used primarily for working with datasets and provides functions for cleaning, analyzing, and manipulating data. 

Using it, I can compare different columns and find the arithmetic mean, max and min values. The primary data structures used in pandas or series and Dataframes. While series or single-dimensional and can be compared to a column in a table, dataframes are multi-dimensional and can potentially store tables efficiently. They are agnostic to the datatypes being stored. Pandas most common applications are reading CSV files and JSON objects and using them within Python code for faster retrieval. Pandas are known to bring speed and flexibility to data analysis. Pandas library is normally imported by the code import pandas as pd. 

NumPy stands for numerical Python and it's a powerful library forming the base for libraries such as Scikit-learn, Scipy, Plotly, and Matplotlib. Python scientists use the abilities of NumPy, especially when working in scientific domains such as signal, image processing, statistical computing, and quantum computing. NumPy carries out the calculations needed for algebraic areas such as Fourier transforms and matrices. The backbone data structure in NumPy is called ND array or N-dimensional array, which substitutes the conventional use of lists in Python, and is a much faster solution than lists. The dimensions in NumPy are called axes and the number of such axes is called a rank. Conventionally, NumPy is imported with import NumPy as np. Matplotlib is the visualization library used in Python. 

It can be used to create static, interactive and animated visualizations. Many third-party tools such as ggplot and seaborn extends the functions of matplotlib. These functions are located inside the pyplot stub package. Matplotlib is imported with import matplotlib.pyplot as plt. An example such as libraries to0 uses the matplotlib and NumPy libraries to for instance, display a graphical representation of students in a class or the distribution of their scores. To recap what you've learned in this video, you should now know about the most commonly used Python data analysis and data science packages.

- - - 
Artificial Intelligence or AI, is broadly about making machines think like humans. Data science primarily focuses on the management and exploration of data which may include media such as text, audio, images and video. Machine learning or Ml is a subsection of AI and deals with algorithms for training and generating insights from data. Many fields utilize machine learning. Some of the most widely used areas are natural language processing, deep learning, sentiment analysis, recommended engines, computer vision and speech recognition. With the amount of text, image and video data available today, data science and AI in particular are in greater demand than ever. Python is one of the most popular languages used in these domains. 

The reasons are, seeing tactical efficiency and readability, flexibility with different languages, frameworks and operating systems welcoming and large community of developers. Ability to build ML models without having to understand the intricacies. Use a friendly debug and testing tools and modular structure. These will promote the development of many primarily open source libraries and frameworks. It's important not to get confused with the terms, packages, library and frameworks. A package is a collection of modules and both library and framework are often used interchangeably with packages. Libraries can also be a collection of packages with specific purpose, whereas the term framework is usually used where certain flow and architecture is involved. 

It's important to remember that all of these pieces of python code are used with the help of import statements. Some of the most popular Ml libraries in use today are in the areas of deep learning and neural networks. Computer vision and image recognition. Natural language processing, data visualization and web scraping. It's important to understand that these are broad categorizations. Most of the libraries associated with them are not restricted to a particular field. Every project is unique and should be treated as such. 

The right selection of the library can save precious time when coding. In this video, you learned about machine learning libraries. Many fields use machine learning and those fields like deep learning and neural networks rely on open source machine learning libraries that make developers work easier. These libraries are collections of packages and the selection of the right library can save you time when coding. So in the future, think carefully about which library you should pick for a project to make sure that it suits your needs.
- - - 
**Understanding Big Data**

- Big Data refers to the management of large sets of structured and unstructured data.
- Key characteristics include Volume (size of data), Variability (inconsistency in data), and Velocity (speed of data processing).

**Data Management Process**

- Data is stored in data warehouses and lakes, both on servers and in the cloud.
- The process involves data analysis and publishing results through reports and visualizations.

**Python's Role in Big Data**

- Python is favored for its ease of use, open-source libraries, and strong community support.
- It offers high compatibility with tools like Hadoop and Spark, enhancing its utility in Big Data applications.

**Key Python Libraries for Big Data**

- Libraries such as NumPy, Pandas, and PySpark facilitate data processing and analysis.
- Additional tools include Amazon's RedShift and S3, Google’s BigQuery, and Kafka for messaging.

- - - 
Web frameworks are software applications designed to provide us with a standard way to build, deploy, and support web applications that we can use on the web. They help developers to focus on application logic and routines by automating redundant tasks, which helps cut development time. They also provide easy structuring and default model so they are reliable, stable, and easily maintainable, saving time and effort. Web frameworks are primarily written in high-level code, which removes the overhead required for understanding concepts such as sockets, threading, and protocols. As a result, time is better spent working on application logic instead of routines. Python is a popular framework in web development, thanks to several features such as good documentation, abundant libraries and packages, ease of implementation, code reusability, a secure framework, and easy integrations. The different frameworks in Python are efficient and make it easy to handle tasks such as form processing, routing requests, connection with databases, and user authentication. 

They also provide debugging and testing tools to handle profiling, test coverage, and test automation, etc. There are mainly three types of web frameworks in Python. These are fullstack, microframeworks and asynchronous. Let's explore each briefly now. Fullstack frameworks are considered a one-stop solution and usually include all the required functionalities. This can include form generations and validators, template layouts, HTTP request handling, WSGI interfaces for connection with web servers and database connection handling. Some of the most popular Python frameworks are Django, Web2py, and Pyramid. 

Microframeworks are a lighter version of fullstacks that do not offer as many patterns and functionalities. They are usually used in smaller web projects and building APIs. Flask, Bottle, Dash, and CherryPy are some of the popular microframeworks. As the name suggests, asynchronous framework types are used to handle a large sets of concurrent connections. They are mainly built using Async IO networking libraries. Growler, AIOHTTP, and Sanic, are some of the names you'll encounter. Choosing a framework can depend on many factors. 

This can include things like available documentation, scalability, flexibility, and integration. While this categorization is pretty broad, it's important to remember that each framework in Python has its own unique set of features and functionalities. This can make certain frameworks more suitable than others for a specific project. Two of the most widely used are flask and Django. Let's explore each briefly now. Django is a high level framework that encourages clean design and rapid development. It's a full-stack framework that's rich in features and libraries. 

It's secure and has templating systems and third party supports. It primarily getting popularity due to its rapid deployment speed. You can quickly build scalable apps without extensive knowledge of low-level programming. Flask is a microframework and better used for smaller projects. It's easy to learn, simple to use, and as a large library of add-ons. In this lesson, you learned about web frameworks and the different types. You also learned about the different web frameworks in Python such as Flask and Django.

- - - 
Testing is an essential component in quality assurance and ensures our software, applications, and websites work as expected. For example, suppose you've built your own website and it has a few 100 businesses every day. One day an article you've published goes viral, suddenly a million people visiting your site and the website crashes. Another scenario is online forms. We've all faced situations where we fill out a form and a prompt appears telling us that we have made a mistake. For example, accidentally entering letters in space provided for credit card numbers or missing special characters and passwords. This type of data validation is even more critical, especially in the domains of banking and finance. 

In this video, you'll learn about testing and it's important in the software development life cycle. But what exactly is testing? Well, software testing is a process of evaluating and verifying the various software applications and products in terms of performance, correctness, and completeness. It helps identify bugs, gaps in the product, defects, and missing requirements with set expectations. In the early days of computers, software developers relied heavily on debugging, a process for removing and detecting potential errors. After the 1980s software grew in size, several different testing types of products also grew in parallel depending on the requirements. Testing was primarily done in the later stages of the software lifecycle. 

Now it's evolved to be integrated at the early stages as well. The efficiency of any testing type is dependent on how well-written it is. The ideal testing scenario is to have the least tests written to find the largest number of defects. While software testing is important in any scenario, the real test of the products comes when it's launched the market, there, it's judged by stakeholders and users. We live in the Internet age, products with bugs, especially in the early stages make consumers lose interests very quickly as many alternatives are available. This is where testing plays an important role. Here are a few reasons why it can help. 

Testing helps detect poor designs, change inefficient flow or functionality, address scalability concerns, and find security vulnerabilities. Testing helps provide AB testing to find the best suitable options, address compatibility with platforms and devices, provide assurance to stake holders, and a better experience for end users. There are a few good practices that must be followed in testing to achieve optimal results. Test code allowing reusability of tests. Tests must be traceable to the requirements set. Tests written must be purpose-driven, efficient, and allow for repeatability. These testing techniques can then follow a procedural approach according to the type of testing used. 

The testing lifecycle in general can be broadly described as planning, preparation, execution, and reporting. The steps involved in achieving this can include writing scripts and test cases, compiling test results, correcting defects based on them, and generating reports from our test results. You've already learned about test cases. They are a general sets of actions containing steps, data, pre and post-conditions written for a specific purpose. This purpose can improve functionality, flow, and finding defects. A well-written test case eventually provides good coverage, reusability, better user experience, reduces costs, and increases overall satisfaction. As the tech industry is ever-growing, several test and categories, types, tools, and products have evolved, which are tailored to best meet the requirements of the software in question. 

For example, a webpage will have different testing needs than an Android-based game. Even among web pages, a social media page will differ from one, say, from financial management. Testing can be categorized by several different factors. For example, depending on the amount we know about the internal implementation, we can call it black box or white box testing. There are also many testing types using practice. These include compatibility, ad hoc, usability, and regression testing. Don't worry too much about these terms for the moment. 

You'll learn more about them later. For now, I just want you to know that with testing, there is no one-size-fits-all solution. When testing products, it's also important to understand when to stop. There is no application will ever be 100 percent perfect. Otherwise, a developer may feel the product is well-tested, but realize it's full of bugs and flaws as soon as it's released to the end-users. A few metrics can be established for this purpose, given that there are well-written test cases in place. These include a certain number of tests cycles, passing percentage of test cases, time deadlines, and time intervals between subsequent test failures. 

Testing in software development can be seen as the anchor of a ship or insurance for your vehicle. You can hope that everything operates smoothly, but often it does not and while you can aim for perfection, there is always potential for human error.

- - - 
Quality assurance today has become an important component in the software development life cycles. Much of the credit goes to development of testing tools and techniques. The question is, what type of testing should you use? In this video, you'll learn about the types of testing, including the four main levels or categories of testing which is units, integration, system and acceptance testing. There are different ways in which you can categorize the different test types. There are white box and black box tests. White box testing is whether tester has knowledge of the code design and functionalities. 

Black box tests function with no such information and the tester has no idea about the internal implementation. There are also other ways to categorize different tests as functional, non functional, and maintenance tests. Let's explore these. Functional tests are based on the business requirements stated. They determine if the features and functionalities are in line with the expectations. Non functional tests are more complex to define and involve metrics such as overall performance and quality of the product. Maintenance tests occur when the system and its operational environment is corrected, changed or extended. 

But there are also manual and automated testing methods that are dependent on the scale of the software. The most broadly accepted categorization is in terms of the levels of testing as you move ahead in the software lifecycle, let's delve deeper into these levels of testing. The four main levels of testing are, unit or component testing, integration testing, system testing and acceptance testing. The four types of testing levels build on each other and have a sequential flow. Let's explore these now. In unit or component testing, the program tests specific individual components by isolating them. The components are low level which means that they are closer to the actual written code. 

They often involve use of automation for continuous integration given their small sizes. So you usually write these tests while writing the code. For example, if the code is in python, unit tests can be written with packages such as pi test, integration testing, combines the unit tests and test the flow of data from one component to another. The key word here is an interface. This means that you test if the data is correctly fetched from a database within the python code, and if you have sent it to the web page. There are different approaches to it such as top down, bottom up and sandwich approaches. Your approach depends on what code level interfaces you attempt first. 

It builds on the unit testing and a tester deals with it. Next is system testing, which tests all the software you tested against the set requirements and expectations to ensure completeness. This includes measurements of the location of deployed components such as reliability, performance, security and load balancing. It also measures operability in the working environment such as the platform and the operating system. This is the most important stage handled by team of testers. It's also the most critical stage as the shipping of software to the stakeholders and end user happened after this phase. The final type of testing is acceptance testing. 

When the product arrives at this stage, it's generally considered to be ready for deployment. It's expected to be bug free and meet the set standards. The stakeholders and the select few end users are involved in acceptance testing. It normally involves alpha, beta and regression testing. One way of approaching this is to give pre-written scenarios to the users. You use the results for improvements and try to find bugs that were missed earlier. All the different testing levels are designed to optimize software at different stages. 

The key to testing is testing early and testing frequently. While each of the testing phases is important, early detection saves time, effort and money. As the code gets increasingly complex mistakes become harder to fix. It doesn't necessarily mean that unit testing will happen only at the beginning and acceptance at a later stage. There are many testing cycles where these levels are approached iteratively. A typical example is the agile model, here you release different versions of the product iteratively and you perform acceptance testing every few weeks. In this video, you learned about some of the types of testing such as a unit testing, integration testing, system testing and acceptance testing. 

It's important to remember that the purpose of these testing methods is to build a systematic approach for testing and identify faults and improvements as early as possible. This results an improved overall performance and experience. Well done.

- - - 

With advancements in technology and increasing drivers towards code automation. In this video, you'll learn about test automation packages and the importance of automated testing. In the past, machines have substituted human efforts in making goods, which helps us save both time and effort. In programming, the tests chosen for automation are the ones that have high repeatability and volume, predictable environments, and data, and determinants outcomes. There are a number of testing types that can be automated. These include units, regression, and integration. An ideal test code must form a bridge between the programming codes and the test cases. 

Python does a fine job in achieving this in addition to its clean, concise way of coding. There are some well-written frameworks in Python and some are more well-accepted than others. The ideal steps involved in test automation are usually preparing the test environment, running the test scripts, and analyzing the results. Let's now examine some important Python testing frameworks that have grown in popularity over the years. First, let's explore the built-in testing package per unit or unittest. The unittest framework supports test automation, independent testing modules, and aggregation of tests into collections. The first is Pytest and native Python library that is simple, easy to use, and reasonably scalable. 

Pytest will be demonstrated later in this course. It can handle several functional test types such as unit, integration, and end-to-end. There is support for parameterized testing which enables us to execute unit tests multiple times with different parameters passed. It can run parallel tests and generates HTML, XML, or plain texts reports. You can also integrate it with other frameworks like Pyunits and nose too, and web frameworks like Flask and Django. While primarily used with testing APIs, it's also well used with UI, database connections, and other web applications. Easy creation and quick bug fixes a why Pytest is the most popular testing framework for automation. 

Next is Robot, which is popular primarily for its keyword-driven development capabilities. These keywords are used in test cases and can be predefined or user-defined. Robot is very versatile and use for acceptance testing, robotic process automation, or RPA, and test-driven development. It can be used for many domains, including Android, APIs, and mainframes. Selenium is another open-source testing framework that has gained popularity over time, and it's primarily driven towards a web applications. It has support for the majority of web browsers and OS. There a browser-specific web drivers that enable testing functionalities like login, button clicks, and filling out forms. 

It allows the test set to select the speed and execution of tests and it has an option to run specific tests or tests suites. Apart from the popular frameworks Pytest, robot, and selenium, there are many more. It's important to know that a number of these testing frameworks are often used with other tools such as plug-ins, widgets, extensions, test runners, and drivers. These tools help integrate the software pieces being tested and add functionality. Sometimes more than one framework has employed over the code being tested. In this video, you learned about test automation packages. Let's recap quickly. 

Automation testing is an important reason why the software industry is able to move ahead swiftly and more smoothly. Manual testing provides focused attention and the ability to handle nuances and complex problems with more sophistication. This kind of testing can't be replaced by automated tests yet. It still sometime before test scenarios can be fully automated, but the development of all these frameworks are in line with that endeavor.

- - - in this video, I will demonstrate how to use pi test to create simple tests for unit testing. Py test is one of the most popular modules for unit testing in python. This is because it allows you to do simple tests with minimal effort and it also has simple clean code with good documentation. First I create a file called addition.py. Next I add a function and past two variables A and B inside it. I'm just going to do a simple calculation that will return the sum of these two variables. Similarly, I create another function called sub which will perform the subtraction between the two variables. 

Second, I create another file called test edition dot py in which I'm going to write my test cases. Now I import the file that consists of the functions that need to be tested. Next, I'll also import the py test module. After that I define a couple of test cases with the addition and subtraction functions. Each test case should be named test underscore then the name of the function to be tested. In our case we'll have test underscore ad and test underscore sub. I'll use the assert keyword inside these functions because tests primarily rely on this keyword. 

It checks for conditions in your code and expects a boolean value of true or false. When the return value is true, the test passes when it is false the test fails. Let's add a certain statements to our tests. In our first test will assert that the addition of 4 and 5 is nine. And in the second test will assert that the subtraction of four and 5 is negative one. Next I make a split screen so that I could see both files. Now I run py test and I specify the file over which I'm going to do the testing. 

To do this, open a new terminal and enter python dash m py test and the name of the test file test edition dot py. I ran the code and both tests passed. This means that both the assert statements have been confirmed to be true. 4+5 is 9 and 4 -5 is -1. These two dots after test edition dot py in the terminal also indicate that both tests passed. Now I will intentionally make one of these tests fail. I do this by changing the -1 answer to -2. I make sure that I have saved the file, clear my terminal and I'll run the test again. 

Note that the first test passed but the 2nd 1 didn't also note that where there were previously two dots. There is now only one and an f to indicate that the second test failed. The ease at the start of the lines show where the test failed and it supplies the possible reason as to why it failed. I can also write these tests without the assert statement and just add pass this passes the test regardless of any errors. When I run the code again it indicates that both tests passed. You should note that I used an equality operator here but I could have used less than greater than or keywords such as is, in or not in all that matters. Is that the assert statement gets a boolean value? 

I can also add multiple assert statements in a single function. So if I write assert true, that's your return the result. And when I run the code again it passes both tests. But if I make it false, it will show that one test has failed. This indicates that all the assert statements within a given function should return a true value for the test to pass. Note that using the test underscore prefix for both the file name as well as the function name is good practice. Now I'll restore my code and save the file. 

If I want to run my test over a specific function, I just add a double colon at the end of the file name and then I write the function name. I'll clear my terminal 1st. Then run my code. Note that only the function I specified has run. Congratulations. You now know about simple functions. In this video you learned that you could use py test to implement unit testing and how to create and use simple tests for unit testing.
- - - 
**Installation of pytest**

- Use `pip3 install pytest` for Mac or `pip install pytest` for Windows.

**Naming Conventions**

- Prefix test files and functions with 'test_' to ensure pytest recognizes them.

**Running Tests**

- Execute tests using `python3 -m pytest test_file.py` or `py.test test_file.py`.
- To run a specific function, use `::` after the file name.

**Using Flags**

- Common flags include:
    - `-v` for verbose output
    - `-q` for quiet mode
    - `-s` to allow print statements
    - `-x` to stop after the first failure
    - `--maxfail n` to limit test failures

**Fixtures and Markers**

- Fixtures provide data before tests run, defined with `@pytest.fixture`.
- Markers allow tagging functions, using `@pytest.mark.<markername>`, and can be executed with `pytest -m <markername> -v`.
- - - 
tdd
Testing has been a relatively recent entry in the software development life cycle but it's importance has been growing as time passes. Software development is time sensitive and in the process, developers often find testing gets squeezed into the time remaining after the code is written. This doesn't leave enough time to test and can lead to the software containing bugs that need to be dealt with over time. Test-driven development or TDD is an alternative programming practice in which the tests are written first and the code is written so that the tests will pass. This difference from the convention of first writing the code and testing the application progressively, TDD follows an iterative approach beginning with writing the test cases. The initial work requires feature and test planning by the team. With slight variations, let's explore the standard steps. 

Step 1, you write a test for a feature that fails. In Step 2, you write code in accordance with the tests. Step 3 requires that you run the test expecting them to fail. In Step 4, you evaluate the error and refactor the code as needed. Finally, in Step 5, you rerun the process. This process is also called the red-green-refactor cycle. Red implies the failed tests and green shows the passing tests after refactoring. 

The whole point of following this cycle is to fail the tests and rewrite until you don't have to. A feature is complete when everything is green and you no longer need to rerun. You can use a package libraries such as Pytest when automation becomes a priority. Pytest only requires writing functions while a unit test requires classes. This means that pytest has the advantage of being easier because it requires less effort. Now let's explore some of the advantages you gain from TDD. Writing tests first and refactoring code based on it ensures the test cover the code. 

You can now write tests with a specific feature and outcome in mind. The need for such forecasting provides clarity from the beginning. The forecasting also plays a role in integrating different components where you add new features and interfaces in accordance with the components that are already there. Working in cycles over the code gives us develop a competence that easily refactor in terms of additional changes. Overall, smaller code with early bug fixes, code extensibility, and eventual ease of debugging are the primary reasons TDD is growing in acceptance. Finally, let's briefly explore some of the differences between TDD and traditional testing. The main difference is that with TDD, the requirements and standards are highlighted from the beginning, making it purposeful. 

Modern-day development often employs a combination of both these forms of testing, depending on the different parts and stages of the software development cycle. There are several subtypes and variations of test-driven development. These include; behavior-driven acceptance, test-driven, scaling, and developer test-driven development. These are all options that can be used in the software development process. Congratulations. In this video, you learned about the process of test-driven development.

- - - 
applying tdd

n this video, you'll learn about how to apply the test-driven development methodology. In conventional testing, you follow the process of writing code and then writing test cases to ensure the integrity of that code. In test-driven development or TDD, the approach is the other way round, and test case is where you must begin your thinking. The steps involved are as follows. Write test cases with some functionality in mind. Write code in accordance with the test cases ensuring that they pass, and refactor code in case the tests fail. Let me demonstrate an example to design a test case that check students enrollment with data stored in a database. 

The test needs to check the integrity of the names entered. First, I'll demonstrate how to design the test case, and then write some code. Let's say I'm checking students enrollments for a class exam against the list of names that I already have. To keep things straight forward, I'm going to use a Python list with names instead of a database. I want to make sure that the names I enter are on the list. I also want to ensure data integrity, which means that I must be sure that the names are entered in the correct format. I've created two files. 

The first is test_findstring, which is my testing file, and the second findstring is my main file. I already have the pytest package installed, and because this is test-driven development, I'll write my test function first. I begin by importing a curses module that will help me check the A-S-C-I-I characters that are present. Then I import the pytest module, as well as the findstring module, which is my main file. I define a function named test_ is present, and I add an assert statement to check if the ispresent function works, because I'm going to use it to validate my data entry. Contrary to the conventional approach of writing code, I first write test_findstring.py, and then I add the test function named test_ispresent. In accordance with the test, I create another file named findstring.py, in which I'll write the ispresent function. 

I define the function named ispresent, and I pass an argument called person in it. Then I make a list of names written as values. After that, I create a simple if else condition to check if the passed arguments is present inside the list. The function called ispresent will check if the name passed is present in the list. Let me test my code. Note that the test has passed because the name Al is in the list. But this doesn't ensure the integrity of entries I may add. 

For example, I might not want numeric characters in the names. To address this issue, I write another function named test_nodigit. I'm going to update some of the code in my main program, findstring.py, in accordance with the newly added test. To do this, I create a function called no digit that matches my tests. Again, I create a simple if else condition. I run the code, and you'll note that one of the tests have passed, and the other one has failed. The name Al passed because it's on the list of names. 

But the value of N7 didn't pass because it contains a numeric character. I could also add more test cases and modify my code so that it's suitable for the test cases. I can repeat the cycle until I have no more failed tests. Congratulations. You've now explored the test-driven development methodology.

- - - 


