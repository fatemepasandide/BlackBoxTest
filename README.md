# Black Box Testing
Black Box Testing is a software testing method in which the functionalities of software applications are tested without having knowledge of internal code structure, implementation details and internal paths. Black Box Testing mainly focuses on input and output of software applications and it is entirely based on software requirements and specifications. It is also known as Behavioral Testing.
#### Types of Black Box Testing
1. Functional testing – This black box testing type is related to the functional requirements of a system; it is done by software testers.
Few major types of Functional Testing are:
    - Smoke Testing
    - Sanity Testing
    - Integration Testing
    - System Testing
    - Regression Testing
    - User Acceptance Testing

2. Non-functional testing – This type of black box testing is not related to testing of specific functionality, but non-functional requirements such as performance, scalability, usability.
Few major types of Non-Functional Testing include:
    - Usability Testing
    - Load Testing
    - Performance Testing
    - Compatibility Testing
    - Stress Testing
    - Scalability Testing

### Performance Testing
Performance testing checks how well software components work. These tests find issues in software design and architecture performance.

This is typically done by:
- Measuring response times
- Identifying bottlenecks
- Locating failure points

Performance tests ensure software quality. They validate that it’s fast, scalable, stable, and reliable.
### Load Tests
Load testing checks how the software behaves under normal and peak conditions. This is done to determine how much work the software can handle before performance is affected.

You can do load tests by running multiple applications simultaneously, subjecting a server to a lot of traffic, or downloading a large quantity of files.

Load tests are used to ensure fast and scalable software.
### Stress Tests
Stress testing checks how the software behaves under abnormal conditions. This determines the limit at which the software will break.

It’s important to find out what happens when the system is under stress. Does the right error message display? Does the system fail? How will it recover?

Stress tests are used to analyze what happens when a system fails. This ensures that software is recoverable, stable, and reliable.

### Best Open Source Load Testing Tools
- [The Grinder](https://grinder.sourceforge.net/)
- [Tsung](http://tsung.erlang-projects.org/)
- [JMeter](https://jmeter.apache.org/)
- [Locust](https://locust.io/)

## Locust
Locust is a Python-based testing tool used for load testing and user behavior simulation.

The main benefits of Locust can be defined as: 

- A user-friendly web-based UI.
- Brings a set of dashboards, visualizations, and test reports that summarize the load testing process. 
- Provides the testing team with a complete picture of the test's current performance.
- Enables the testing team to run multiple test scripts to find out the main performance and load handling problems..
- Enables the testing team to quickly increase the number of test cases and compare the test results for every task, user, and request.

###### Example locustfile.py
```python
import random
from locust import HttpUser, task, between

class QuickstartUser(HttpUser):
    wait_time = between(5, 9)

    @task
    def hello_world(self):
        self.client.get("/hello")
        self.client.get("/world")

    @task(3)
    def view_item(self):
        item_id = random.randint(1, 10000)
        self.client.get(f"/item?id={item_id}", name="/item")

    def on_start(self):
        """
        on_start is called when a Locust start before any task is scheduled
        """
        self.client.post("/login", json={"username":"foo", "password":"bar"})
```
Here we define a class for the users that we will be simulating. It inherits from HttpUser which gives each user a client attribute, which is an instance of HttpSession, that can be used to make HTTP requests to the target system that we want to load test. 
Methods decorated with @task are the core of your locust file. For every running user, Locust creates a greenlet (micro-thread), that will call those methods.

We’ve declared two tasks by decorating two methods with @task, one of which has been given a higher weight (3). When our QuickstartUser runs it’ll pick one of the declared tasks - in this case either hello_world or view_items - and execute it. Tasks are picked at random, but you can give them different weighting. The above configuration will make Locust three times more likely to pick view_items than hello_world. When a task has finished executing, the User will then sleep during its wait time (in this case between 1 and 5 seconds). After its wait time it’ll pick a new task and keep repeating that.

###### Test
Write the above code in locustfile.py and you can run the test by running locust command on your terminal.
##### Example :
from locust import HttpUser, task
```python
class HelloWorldUser(HttpUser):
    @task
    def hello_world(self):
        self.client.get("/hello")
        self.client.get("/world")
```

<img title="a title" alt="Alt text" src="https://docs.locust.io/en/stable/_images/webui-splash-screenshot.png" width="70%">
<img title="a title" alt="Alt text" src="https://docs.locust.io/en/stable/_images/webui-running-statistics.png" width="70%">
<img title="a title" alt="Alt text" src="https://docs.locust.io/en/stable/_images/number_of_users.png" width="70%">
<img title="a title" alt="Alt text" src="https://docs.locust.io/en/stable/_images/response_times.png" width="70%">



## JMeter
Apache JMeter is pure Java-based open-source software designed to load test functional behavior and measure performance. You can use JMeter to analyze and measure the performance of web applications or a variety of services.

After [install](https://jmeter.apache.org/usermanual/get-started.html) JMeter create a plan test.
A test plan describes a series of steps JMeter will execute when run. A complete test plan will consist of one or more Thread Groups, logic controllers, sample generating controllers, listeners, timers, assertions, and configuration elements.

<img title="a title" alt="Alt text" src="https://raw.githubusercontent.com/apache/jmeter/master/xdocs/images/screenshots/jmeter_screen.png" width="70%">


## JMeter VS Locust
|  | JMeter | Locust |
|  :---:  |  :---:  |  :---: |
| Open Source | Yes | Yes |
|Execution Monitoring| Console, File, Graphs, Desktop client, Custom plugins|Console, Web|
|Support of “Test as Code”|Weak (Java)|Strong (Python)|
|In-built Protocols Support|HTTP, FTP, JDBC, SOAP, LDAP, TCP, JMS, SMTP, POP3, IMAP|HTTP|
|Scripting (recorded)|Recorded|none|
|Usable as a library|No|Yes|


#### References:
https://www.softwaretestinghelp.com/black-box-testing/
https://www.guru99.com/black-box-testing.html
https://www.perforce.com/blog/alm/what-non-functional-testing
https://www.blazemeter.com/blog/open-source-load-testing-tools
https://docs.locust.io/
https://github.com/apache/jmeter
https://betterprogramming.pub/introduction-to-locust-an-open-source-load-testing-tool-in-python-2b2e89ea1ff
