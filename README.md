<p align="center">
<img src="https://github.com/cjolson1/Lagunitas-Quality-Assurance/blob/master/Screen%20Shot%202016-06-03%20at%202.31.16%20PM.png">

<h1>Lagunitas Quality Assurance</h1>

</p>

###### Christopher Olson | Maria Casciani &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; June 3, 2016 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Last Revision: June 7, 2016 11:33
<br>
## Table of Contents

1. [Getting Started](#getting-started)
  * [Quality Overview](#quality-overview)
  * [Requirements and Design Development](#requirements-and-design-development)
  * [Test Driven Development (TDD)](#TDD)
  * [Skepticism Toward Test Driven Development](#skepticism-toward-test-driven-development)
2. [Test Planning and Designing](#test-planning-and-designing)
  * [Test Cases](#test-cases)
  * [Test Automation](#test-automation)
  * [Bugs](#bugs)
  * [Metrics](#metrics)
3. [Development and Repeated Testing](#development-and-repeated-testing)
  * [Unit Tests](#unit-tests)
  * [Mocking](#mocking)
  * [Integration Tests](#integration-tests)
  * [Code Reviews](#code-reviews)
    - [Architecture and Design](#architecture-and-design)
    - [Style](#style)
    - [Testing](#testing)
  * [Security](#security)
4. [Final Steps](#final-steps)
  * [Regression Cycle](#regression-cycle)
  * [System Tests](#system-tests)
  	- [Performance Tests](#performance-tests)
  	- [Session Based Testing](#session-based-testing)
	- [Acceptance Tests](#acceptance-tests)
5. [Testing Frameworks](#testing-frameworks)
  * [Django](#django)
  * [JavaScript](#javascript)
  * [Selenium](#selenium)

## Getting Started

When beginning any project, review code standards and metrics, as well as establish the requirements for the new software. Reviewing these standards and metrics is important to maintain a high-quality codebase that is consistent. 

This is the basic plan for the Quality Assurance process:

<p align="center">
 <img src="https://github.com/cjolson1/Lagunitas-Quality-Assurance/blob/master/Screen%20Shot%202016-05-31%20at%204.36.12%20PM.png">
</p>

### Quality Overview

Each of the following should be reviewed and understood by all developers on the team, in addition to the <a href="/">Lagunitas Code Quality Standards</a> and <a href="https://www.python.org/dev/peps/pep-0008/">PEP 8</a>:

- **Error handling:** Errors should be handled gracefully & explicity. Custom errors can be created to more effectively address issues in the code.
- **Avoid repeated code**
- **Comment code clearly & concisely:** It's a good idea to remove any commented out lines.
- **File length:** Excessive file lengths are a good indicator that the file should be split into smaller, more focused files. As the file size increases, discoverability decreases. 
- **Review your own code first:** Examine the changes made, and look for discrepancies or issues you might want to address before others address you about it.
- **Method names:** Naming things is one of the hard problems in computer science. If a method is named `get_message_queue_name` and it is actually doing something completely different like sanitizing HTML from the input, then that’s an inaccurate method name. And probably a misleading function.
- **Variable names:** `foo` or `bar` are probably not useful names for data structures. `e` is similarly not useful when compared to `exception`. Be as verbose as you need (depending on the language). Expressive variable names make it easier to understand code when you have to revisit it later.
- **Docstrings:** For complex methods or those with longer lists of arguments, is there a docstring explaining what each of the arguments does, if it’s not obvious?
- **Readability:** Is the code easy to understand? Is it necessary to pause frequently during the review to decipher it?

### Requirements and Design Development

Develop clear, and mutually agreeable goals for the final product. These should be well communicated and delegated. Developers should commit to deadlines for each stage. In addition, standards should be established for security of the software metrics.

### Test Driven Development (TDD)

TDD is when the tests are developed BEFORE the code. That way sofware engineers develop functionality until the code passes the test. As each new addition to the code is made, its test can be added to the suite of tests that are run when you build the integrated work. This front-loading of test creation allows the CI process to accelerate smoothly. Test Driven Development also can be thought of as "Test Driven Design", allowing the structure of the tests to allow your development to proceed with smaller less complex units of code more clearly coming together. 

### Skepticism Toward Test Driven Development

#### Pros of TDD

- Makes code easier to maintain and refactor.
- Makes collaboration easier and more efficient, team members can edit each others code with confidence because the tests will inform them if the changes are making the code behave in unexpected ways.
- Good unit testing forces good architecture.  In order to make your code unit-testable, it must be properly modularized, by writing the unit testing first various architectural problems tend to surface earlier.
- Over the long term it's faster. Refactoring code written two years in the past is hard. If that code is backed up by a good suite of unit tests, the process is made so much easier.
- The Django web framework directly supports TDD for development. They have unit tests for the web data model, control layers, and HTML presentation and behavior. 

#### Cons of TDD

- Like any programming, there is a big difference between doing it and doing it well.  Writing good unit tests is an art form. (We will get to that in this document.)
- Initially slower while unit tests are developed.
- Unit testing is something the whole team has to buy into. Team Leaders have to enforce policies and actually get in and check the tests.
- Complex cases are much harder to design test cases for. This is why the cases must be broken down AHEAD of development.

While test driven development (TDD) projects have higher quality, there is a cost of increased deployment time for less bugs. This time expense is a valid concern; however, for Lagunitas, a company in the process of scaling, TDD can be advantageous for developing quality applications with minimized time wasted fixing bugs long term. 

TDD is a critical part of Agile development, according to the Agile <a href="http://programmers.stackexchange.com/questions/21870/can-you-be-agile-without-doing-tdd-test-driven-development">community</a>.

<p align="center">
<b>According to "Exploding Software-Engineering Myths" by Microsoft Research, which used Microsoft to test the efficiency of TDD, test-driven development reduces defects in software by 60-90% while increasing time investment by 15-35%.</b>
</p>

According to the author of the paper, "Over a development cycle of 12 months, 35 percent is another four months, which is huge; however, the tradeoff is that you reduce post-release maintenance costs significantly, since code quality is so much better. Again, these are decisions that managers have to make—where should they take the hit? But now, they actually have quantified data for making those decisions.”

TDD is helpful to learn, understand, and internalise key principles of modular design to deliver quality results. On the flipside, TDD forces a shift toward a more concentrated effort at the inception of the project's development, forcing a slow start that eventually picks up speed.

<p align="center">
<b>Furthermore, the software Lagunitas currently employs for testing (Selenium, Jenkins, Django) are made to be used with TDD.</b>
</p>

With that being said, we strongly recommend TDD be used by Lagunitas. (except for cases in which a project is too small or trivial to consider testing extensively)

## Test Planning and Designing

Test driven development fosters clear goals and allows for those key requirements to be checked at each step in the development process. 

### Test Cases
These depend on the product, but they should address the desired requirements and functionality. Test cases form the backbone of any software platform and should be create painstakingly. It is important to NOT write trivial tests for your project; they contribute to code rot and generally make you and your team test-resistant, which is not a good thing for the quality of your software. 

A good test should be able to answer the following questions clearly:

- What are you testing?
- What is the functionality?
- What is the actual output?
- What is the expected output?
- How can the test be reproduced?

### Test Automation
Designing tests to be automated will allow for more tests and a better use of developers' time. In the case of Lagunitas, Jenkins and Selenium would be used to test code as it is committed; however, it is important to do more exhaustive, time-consuming testing when the project is not being worked on (e.g. when the office is empty). During this phase of the software development cycle, we recommend that tests be automated in a structured, two-tier process.

#### Tier 1
Also known as "microtesting" or sanity checks, this stage requires testing after each commit (i.e. required cases). This testing should be rudimentary and quick in order to ensure productivity.

#### Tier 2
Also known as "macrotesting" or regression testing, exhaustive testing at night when project isn't being worked on. These tests should take a lot of time and should work to test every aspect of the system extensively.

### Bugs
When faced with a system with multiple bugs, it is important to address those that affect the most people/users first. Integrating a tool like <a href="https://www.google.com/analytics/#?modal_active=none">Google Analytics</a> or a custom software tool to evaluate the most pertinent bugs is recommended.

### Metrics
Metrics are critical to understanding the level and quality of the software’s progress. A 100% success rate is not always advised, as sufficient rates will allow for quality while not restraining progress. 

Some metrics include:
- **User sentiment**
- **Requirement Turnover:** Requirements delivered/requirements desired at the outset
- **Defect removal efficiency:** No. of Defects found during QA testing / (No. of Defects found during QA testing +No. of Defects found by End user)) * 100
- **Defect density:** defects in a module/ total defects
- **Pass rate:** While need not be 100% in all scenarios, test case pass rates should be 100% for continuous integration testing.
- **Code coverage:** This metric does not necessarily reflect the quality of the test cases written, code coverage is always a good indicator of how much of the project has actually been tested. 100% code coverage means that when the entire sum of all your tests are run, every line is executed at least once. This metric is an indicator of working code, not good code.
- **Defect metrics:** Used in conjunction with the team's goals, it establishes thresholds for how many bugs are allowed in a development over a certain time period.

&#9;While these metrics are helpful, they are certainly not complete; metrics may be project-specific and establishing these before a project starts accelerates the software development process while maintaining quality.

## Development and Repeated Testing
This step of the Quality Assurance process will be the most time-consuming and important. Testing during development keeps track of the project's progress. 

In general, the testing of a project will follow the following ascending order:

<p align="center">
 <img src="https://github.com/cjolson1/Lagunitas-Quality-Assurance/blob/master/Screen%20Shot%202016-05-31%20at%204.37.26%20PM.png">
</p>

During the development process, unit tests are the most critical to ensure quality in the software. We recommend in [Tier 1](#tier-1) testing that tests be 70% unit tests & 20% integration tests. [Tier 2](#tier-2) tests that are end-to-end should account for 10% of total tests according to <a href="http://googletesting.blogspot.com/2015/04/just-say-no-to-more-end-to-end-tests.html"> Google Testing </a>. 

### Unit Tests
Unit tests take the smallest piece of testable software in your project, isolate it, and determine whether its behavior matches what you expect from it. This type of testing is a critical initial part of the quality assurance process. Unit tests provide fast, reliable, isolation of failures. This fuels efficient and effective error removal. 

Studies from Microsoft Research, IBM, and Springer tested the efficacy of test-first vs test-after methodologies and consistently found that a test-first process produces better results than adding tests later. It is resoundingly clear:

<p align="center">
 <b>Before you implement, write the test.</b>
</p>

Some general topics to cover when unit testing are the following:

- **Module Interface test:** This test checks if information is properly flowing in and out of the program or module.
- **Local data structures:** Testing if the data that is supposed to be saved is saved is extremely important.
- **Boundary conditions**
- **Error handling paths:** The system's behavior surrounding errors should be controlled and useful [(see more)](#quality-overview)

It is important to document each test extensively in order to promote reusability and clarity. Additionally, a failing test should read like a high-quality bug report. Failed test reports should include what was being tested, the expected output, and the actual output. 

How do you unit test? <br>

A common approach to unit testing involves writing drivers and stubs. The driver simulates a calling unit and a stub simulates a called unit. While creating these tests can be bothersome, they pay back when errors must be found in the complex application later, and support [test automation](#test-automation). 

Furthermore, while it may be tempting/cost-effective to "glue" together two units and test them as an integrated unit, an error can occur in a variety of places, creating confusion and uncertainty. With an integrated unit composed of unit 1 and unit 2, an error can occur in either of the units, both of the units, in the interface between the units, and in the test itself. These possibilities demonstrate why unit tests must come first, then integration, then system tests. 

### Mocking

There are lots of definitions things which are under the umbrella of test doubles. Here I will focus on Mocks and stubs. Mocks simulate the behavior of actual objects to allow testers to isolate a smaller component of the program during unit testing, such as a class. Mocking is more elaborate than stubbing. Stubs perform state verification, or simply provide input (the minimal amount of object behavior to enable the tested module to run). When unit testing, mocks perform behavior verification. 

For example, you want to test your software’s ability to send an email if an order wasn’t received. A stub in this case would count how many emails were sent. A mock would track if the program sent the email with the right contents and to the correct person. We have a detailed example of mocking in Django [here](#testing-django-with-mocking).

When to use test doubles?

When your method being unit tested has dependencies. Use test doubles for every dependency your unit test touches. The choice to implement a mock vs a stub is the testers decision based on what they need. When creating a mock, set the exact return values the dependency should return when tested, and exceptions to throw to test the method’s error handling. 

Mock Shortcomings:

Excessive mocking calls in a unit test can lead to unit tests that are too tightly coupled to the internal implementation of the very dependency you’re trying to mock.  This can make your test ineffective because the mock is too similar to the actual dependedency so if your dependency implementation. 

Usage of mocks usually follows this pattern:

1. Setup of the mock(s) and expectations.
2. Execution of the code to test.
3. Verification that the expectations were met.

### Integration Tests
Integration testing begins testing multiple units of code together. It is the logical extension of unit testing. Integration testing allows for an intermediary step before system testing so that problems can be more quickly localized and addressed. This form of testing identifies problems that occur when units are combined. Most of the time when an errors occurs, it is due to the interfacing between the units.

As a rule of thumb:

<p align="center">
 <b>
 Unit testing makes sure you are using quality ingredients. Functional testing makes sure your application doesn't taste like crap.
 </b>
</p>

Of particular interest for Lagunitas is the interaction of the software being written and the third-party libraries and external resources such as file systems, databases, and network services.  This is due to the fact that the behavior of such dependencies may not be fully known or controlled, or may change in unexpected ways when new versions are introduced. In context of external webservices that may be used in conjunction with a project, this means that integration testing can prove vital to the success of the project.

You and your team can do integration testing in a variety of ways, but it is important to automate them. It is important that you and your team come to a consensus on what the strategy will be in order to improve workflow and the organization/quality of your work. 

The following are three common approaches to integration testing:

- **Top-down approach:** This approach requires testing the highest-level modules first. This allows high-level logic and data flow to be tested early on and minimizes the need for drivers; however, the need for stubs complicates test management and low-level applications are tested late in the development cycle. Furthermore, this form of testing does not promote early release of a project with limited functionality because it requires everything be thoroughly tested before finishing.
- **Bottom-up approach:** The logical counterpart to the top-down approach. The need for stubs is minimized, but there is a need for drivers which complicates the test management. High-level logic and data flow are tested late. Like the top-down approach, the bottom-up approach also provides poor support for early release of limited functionality.
- **Umbrella approach:** This approach is more advanced and free-spirited. It requires testing along functional data and control-flow paths. First, the inputs for functions are integrated in the bottom-up pattern discussed above. The outputs for each function are then integrated in the top-down manner. The primary advantage of this approach is the degree of support for early release of limited functionality. It also helps minimize the need for stubs and drivers. The potential weaknesses of this approach are significant, however, in that it can be less systematic than the other two approaches, leading to the need for more regression testing.

We suggested Lagunitas use either the top-down or bottom-up methods first to adapt to the idea of test-driven development. As comfort with the process grows, we advise a transition to the Umbrella approach for projects that would like to have Alpha and Beta stages before deployment.

### Code Reviews
Briefly, a code review is a discussion between two or more developers about changes to code to address an issue. Its importance is critical; it promotes collaboration, identifies bugs, and keeps code more maintainable. We suggest that code reviews be done when a component of a project is finished. Code reviews should be done with two or more developers, including the person that wrote the code under review.

Here are some useful practices to employ during code reviews:

- **Review fewer than 200-400 lines of code at a time:** The Cisco code review study showed that for optimal effectiveness, developers should review fewer than 200-400 lines of code (LOC) at a time. Beyond that, the ability to find defects diminishes. At this rate, with the review spread over no more than 60–90 minutes, you should get a 70–90% yield. In other words, if 10 defects existed, you'd find 7 to 9 of them.
- **Aim for an inspection rate of fewer than 300-500 lines of code (LOC) per hour:** Take your time with code review. Faster is not better. IBM research shows that you'll achieve optimal results at an inspection rate of less than 300–500 LOC per hour.
- **Take enough time for a proper, slow review, but not more than 60–90 minutes:** IBM research shows that 60-90 minutes are the upper bound for effective code reviewing. On the flip side, you should always spend at least five minutes reviewing code, even if it's one line.
- **Be sure that authors annotate source code before the review begins**
- **Create a personal checklist:** Each person typically makes the same 15–20 mistakes. If you notice what your typical errors are, you can develop your own personal checklist (Personal Software Process, the Software Engineering Institute, and the Capability Maturity Model Integrated recommend this practice, too). Reviewers will do the work of determining your common mistakes. All you have to do is keep a short checklist of the common flaws in your work, particularly the things that you most often forget to do.
- **Foster a good code review culture in which finding defects is viewed positively**
- **Metrics should never be used to single out developers, particularly in front of their peers:** This practice can seriously damage morale.

Code reviews should span the components Architecture and Design, Style, and Testing of the code being looked at. In conjunction with the practices mentioned above, here are some guidelines to consider when looking at each of these elements specifically.

#### Architecture and Design
- **Single Responsibility Principle:** The idea that a class should have one-and-only-one responsibility. Harder than one might expect. If you have to use “and” to finish describing what a method is capable of doing, it might be at the wrong level of abstraction. This greatly improves testability.
- **Code duplication:** Go by the “three strikes” rule. If code is copied once, it’s usually okay If it’s copied again, it should be refactored so that the common functionality is split out.
- **Code left in a better state than found:** If changing an area of the code that’s messy, it’s tempting to add in a few lines and leave. We recommend going one step further and leaving the code nicer than it was found.
- **Efficiency:** If there’s an algorithm in the code, is it using an efficient implementation? For example, iterating over a list of keys in a dictionary is an inefficient way to locate a desired value.

#### Style
See [Quality Overview](#quality-overview)

#### Testing
- **Test coverage and quality:** It's great to see tests for new features. Are the tests thoughtful? Do they cover the failure conditions? Are they easy to read? How fragile are they? How big are the tests? Are they slow?
- **Testing at the right level:** When reviewing tests make sure that they are testing the program at the right level. In other words, are we as low a level as we need to be to check the expected functionality? Gary Bernhardt, a prominent TDD software blogger, recommends a ratio of 95% unit tests, 5% integration tests. With Django projects, it’s easy to test at a high level by accident and create a slow test suite so it’s important to be vigilant.
- **Number of Mocks:** Mocking is great. Mocking everything is not great. Use a rule of thumb where if there’s more than 3 mocks in a test, it should be revisited. Either the test is testing too broadly or the function is too large. Maybe it doesn’t need to be tested at a unit test level and would suffice as an integration test. Either way, it’s something to discuss.
- **Meets requirements:** Usually as part of the end of a review, look at the requirements of the story, task, or bug which the work was filed against. If it doesn’t meet one of the criteria, it’s better to bounce it back before problems arise.

### Security
The dependence on Django, a ubiquitous technology, means that its primary security vulnerabilities are well known and documented. Developers should consider taking steps to, at the very least, ensure that the most critical software (such as connections to the financial software) is strong against basic threats.

 - The simplest first step is to update the current working version of your Django framework in all your environments. And while Django is backwards compatible, it is nonetheless crucial that you identify any components in your web app that might be impacted by patching/updating.
 - Django's built-in CSRF protection is good. Enable it and use it everywhere.  (Cross-Site Forgery Attack: when a hacker tricks an authenticated user into loading a malicious url to transmit commands)

 - Avoid manually forming SQL queries using string concatenation. For instance, do not use raw SQL queries (e.g. `raw()`). Similarly, do not use the `extra()` method/modifier to inject raw SQL. Do not execute custom SQL directly; if you bypass Django's Object Relational Mapping layer, you bypass its protections against SQL command injection. (SQL injection attacks are used to read sensitive data, modify data, or perform administrative actions) 
 
Of course, these are only the basics; however, any platform that is being rolled out should have comprehensive security.

#### Fuzz Testing
Fuzz testing purposely attempts to cause the system to crash, by inputting large quantities of random data. Examples of these kinds of issues would be buffer overflow, cross-site scripting, denial of service attacks, or SQL injection. Fuzz testing is typically employed by hackers to bring a system down with minimal effort. Fuzz testing is not effective for discovering threat potential from things such as: spyware, viruses, and Trojans. We do not recommend Lagunitas use this form of testing because it is not an efficient, or methodical testing technique.
<br>

## Final Steps

The following tests are used once the all the software modules have been developed, unit, and integration tested. These tests determine systemic issues, and assess what needs to be fixed before the software goes into use. 

### Regression Cycle

A regression cycle is run in the final phase of product stabilization, and it is that proceeds that triggers the green light to go to production. Since very little is changing in development at this point in the project, there is an opportunity to validate the entire product. We suggest modelling the entire piece of software as a directed graph with edges pointing from components to the components that are dependent upon it. When any branch is modified, the hierarchy shows what branches below it will be affected and will need additional QA testing.

We suggest using a "traffic light" method for the regression cycle. If every edge recieves a green light (passes all tests), the product is considered ready for delivery. If a branch receives a yellow light (all tests passed but with one or more reported warnings), it is important to talk about whether the piece should be worked on more or deployed and fixed after. Finally, if a branch receives a red light (one or more tests failed), stop and address the issue. We suggest that you automate the regression cycle, so it only takes a few days to run.

#### System Tests

System tests verify that the whole system meets the application architecture and business requirements by assessing all of the feature, as a whole. 

Entry Criteria to begin System Tests:
- Unit Testing should be finished.
- Integration of modules should be fully integrated.
- As per the specification, document software development is completed.
- Testing environment is available for testing.

System testing is important because of the following reasons:
- System testing is the first step in testing the application as a whole.
- The application is tested thoroughly to verify that it meets the functional (business requirements) and technical specifications.
- The application is tested in an environment that is very close to the production environment where the application will be deployed.

To illustrate how comprehensive we suggest System Tests should be, here is an example:

For example you are doing testing on a web application of a school. This web application there are many modules like Teacher Module, Parent Module, Student Module. Now you have to do System Testing on a web application of a school, so your criteria for doing System Testing will be like that which is given below:

1. First you test the GUI related issues on all these above mentioned modules by checking that font size, alignment, display images work properly on all the modules.

2. Now after checking the GUI issues you move towards functionality related issues by checking that the requirements of client have been met or not.

3. After checking functionality, you can check whether the application is user friendly or not by checking that proper error message should be displayed on screen or not. 

#### Performance Tests
Performance tests assess speed, scalability, reliability, and stability. 

Lagunitas' software, especially programs that will interact with a growing number of users should be assessed with performance-based testing. Scaling issues are critical to platforms such as the distributor ordering system, or ones which will have to withstand larger amounts of data flow. Load testing and endurance testing seem most useful to the expected usage of Lagunitas software which will likely endure sustained usage, with minimal high loads or spikes.

Performance will be determined by running software through a variety of difficult scenarios.
 
- **Load Testing:** This measures important business critical transactions and expected load on the database, application server, etc. There are many open source options to simulate many users such as <a href="https://jmeter.apache.org/">JMeter</a> or <a href="http://gatling.io/#/">Gattling</a>. Selenium is capable of performance testing, but isn’t an optimal choice. This is because when doing load tests it is significantly slower than other options like JMeter. The Selenium WebDriver is also capable of working with <a href="http://junit.org/junit4/">JMeter JUnit 4</a>. To use <a href="http://www.seleniumhq.org/projects/webdriver/">Selenium WebDriver</a>, simply install “WebDriver Set” plugins. The WebDriver sampler is very useful if you want to test AJAX based web applications and simulated user actions. 
    1. **Endurance Testing:** Tests system performance under sustained load.
Can use <a href="http://loadrunnerjmeter.com/">LoadRunner</a> in JMeter, or  Selenium WebDriver. 
Issues commonly found include: 
       * Serious memory leaks that would eventually result in application or Operating System crash.
       * Failure to close connections between the layers of the system could stall some or all modules of the system.
       * Failure to close database connections under some conditions might result in the complete system crash.
       * Gradual degradation of response time of the system as the application becomes less efficient as a result of prolonged test.
    2. **Stress Testing:** Finds the upper capacity of the system. 
- **Spike Testing:** Determines the ability of the system to handle large increases in usage in a small amount of time. Can be done with a variety of performance testing tools, including JMeter.

#### Session Based Testing
Session based testing is when a developer takes 90 ± 45 minutes to try to find errors and break a program. It is very exploratory and spontaneous, as opposed to scripted testing with software. As a result, new errors can be found while also verifying software requirements. Session based testing in conjunction with more traditional, regimented testing is recommended by experts at Microsoft. 

Session testing basics: 
- **Goal-based:** Set a focus for the tester. Without a clear testing procedure, the session tester will approach the problem in their own way.
- **Documentation:** The session tester must clearly document what was found and how errors occurred. 
   - **Scenario #1:** I tried launching the Open File dialog using keyboard shortcut, and I found out that the hotkey to Open is “n” instead of the more traditional “o”.  We should keep the hotkey consistent with other applications.  Bug #123 filed.
   - **Scenario #2:** Dragging the Open File dialog window around, and this seems to work fine as expected.  Also drag the window outside of the screen and the window redraws itself back correctly.
   - **Scenario #3:** Tried opening file without extension, and Notepad crashes!  Bug #456 filed.

Pros: 
- **Can find previously unknown errors**
- **Low Cost:** No need to create unit tests, so developers get instantaneous feedback on software performance.

Cons: 
- **Limited by the domain of knowledge of the tester**
-  **Cannot be done for long period of time**

### Acceptance Tests

Acceptance tests verify if the full system meets all requirements and if the program is prepped for delivery. At this stage, the focus is on the user-experience. Acceptance tests are not only intended to point out simple spelling mistakes, cosmetic errors, or interface gaps, but also determine:

- **Usability:** Is the UI configured to their liking? Did we put the customer branding in all the right places? Do   we have all the fields/screens they asked for?
- **Maintainability:** How will we deliver software updates/patches?
- **Configurability:** Can user modify the system to suit their needs?

Here is a <a href="https://blog.8thlight.com/doug-bradbury/2011/04/26/ten-ways-to-do-acceptance-testing-wrong.html">link</a> to some common mistakes that teams make when doing Acceptance Tests.

There is a variety of software out there that can perform automated front-end tests, and we suggest using [CasperJS](#casperjs). These should always be supplemented by manual human testers verifying if everything does in fact look correct. 

Here is a breakdown of the good and the bad with Selenium and Casper:

Selenium WebDriver

- **The good:** You can automate most browsers and mobile devices in many different programming languages. It’s pretty widely adopted across other frameworks as well, and can integrate with BDD (Behavior Driven Development) tools such as Cucumber. There are meetups and conferences and Google Hangouts where you can find support. Selenium is pretty easy to pick up with some programming knowledge. You can also use Selenium Grid/Server or <a href = "https://saucelabs.com/">SauceLabs</a> to run your tests in parallel across multiple machines.
- **The bad:** Just because you can do almost anything doesn’t mean you should. It’s very tempting to try to make your test suite run on every browser and support mobile devices, but the maintenance cost is very high and it’ll take you a while to figure out how to make each browser’s driver work with your code. Utilizing Selenium’s capabilities to run across browsers and devices and in parallel on Sauce was a lot of effort for very little reward. All of that effort you spend maintaining can’t be spent making new tests, and you still have to manually check those browsers for visual bugs.

Selenium is excellent for large teams who intend to utilize the cross-platform and cross-browser testing features or use Sauce Labs. The maintenance cost will be high, but if you have the patience and bandwidth, go for it. Selenium is a widely-adopted and powerful tool for automation and can help you achieve your wildest automated testing dreams if you’re dedicated enough.

CasperJS

- **The good:** CasperJS’s <a href="http://docs.casperjs.org/en/latest/modules/">API documentation</a> is incredibly helpful compared to the digging with Selenium. The tester module makes your passes and fails very clear. You can write your tests in CoffeeScript or JavaScript, or CasperJS. CasperJS, as a headless browser is quick, light-weight, and easy to set up. There are very helpful guides for beginners. This will leave you time for the inevitable manual testing, or for writing even more CasperJS scripts. While CasperJS doesn’t have quite as wide an adoption as Selenium, there are already a lot of great open-source tools to help you do a lot of cool things with it.
- **The bad:** Headless browsing is sometimes intimidating; debugging is not incredibly fun to do when you can’t see what your code is doing in a browser. CasperJS’s debug mode combined with its methods to capture screenshots are a good substitute in my opinion, but be patient when you start learning this framework.

CasperJS is a great tool for small teams like Lagunitas, who want to test quickly. 

A detailed slideshow on how to perform using Selenium WebDriver can be found <a href="http://www.slideshare.net/NickBelhomme/mastering-selenium-for-automated-acceptance-tests/34-Setting_up_the_grid">here</a>.

A rundown of acceptance testing in CasperJS can be found <a href="https://github.com/ivanoats/Full-Stack-JavaScript-Engineering/blob/master/casper/acceptance_testing_with_casperjs.md">here</a>.

### Testing Frameworks

There are many frameworks that can be used to test software built here at Lagunitas. Many of the platforms that are used here are made for TDD and easily support unit testing, easily the most critical step in the testing process. 

With that said, here are how the testing frameworks for Django, JavaScript, and Selenium work:

#### Django

The testing framework for Django is split between the front-end and back-end. 

##### Testing Django on the back end

On the back-end, Django's testing platform is an extension of Python's `unittest` module, which makes it easy to interact with. Test cases are objects from the subclass `django.test.TestCase`, which is a subclass of `unittest.TestCase` that runs each test inside a transaction to provide isolation. We highly recommend taking advantage of this testing library.

Here is an example of what an example test case might look like:

```python
from django.test import TestCase
from myapp.models import Animal

class AnimalTestCase(TestCase):
    def setUp(self):
        Animal.objects.create(name="lion", sound="roar")
        Animal.objects.create(name="cat", sound="meow")

    def test_animals_can_speak(self):
        """Animals that can speak are correctly identified"""
        lion = Animal.objects.get(name="lion")
        cat = Animal.objects.get(name="cat")
        self.assertEqual(lion.speak(), 'The lion says "roar"')
        self.assertEqual(cat.speak(), 'The cat says "meow"')
```

When you run your tests, the default behavior of the test utility is to find all the test cases (that is, subclasses of `unittest.TestCase`) in any file whose name begins with **test**, automatically build a test suite out of those test cases, and run that suite. The default **startapp** template creates a **tests.py** file in the new application. This might be fine if you only have a few tests, but as your test suite grows you’ll likely want to restructure it into a tests package so you can split your tests into different submodules such as **test_models.py, test_views.py, test_forms.py,** etc. Feel free to pick whatever organizational scheme you like.

Once you've written tests, run them using the **test** command of your project's **manage.py** utility:

```python
$ ./manage.py test
```

Django's test library also has the ability to do built-in test discovery. You can specify particular tests to run by supplying any number of “test labels” to `./manage.py test`. Each test label can be a full Python dotted path to a package, module, `TestCase` subclass, or test method.

Some examples of this include:

```python
# Run all the tests in the animals.tests module
$ ./manage.py test animals.tests

# Run all the tests found within the 'animals' package
$ ./manage.py test animals

# Run just one test case
$ ./manage.py test animals.tests.AnimalTestCase

# Run just one test method
$ ./manage.py test animals.tests.AnimalTestCase.test_animals_can_speak
```

You can also provide a path to a directory to discover tests below that directory:

```python
$ ./manage.py test animals/
```

There is also a pattern option:

```python
$ ./manage.py test --pattern="tests_*.py"
```

We also suggest using the `--failfast` option which allows for a notice on failures without having to wait for the entire testing sequence to complete.

Further information regarding unittest and its customizations can be found <a href="https://docs.python.org/3/library/unittest.html#module-unittest">here</a>.

##### Testing Django on the front-end

On the front-end, Django has a test client that acts as a dummy Web browser and allows you to test your views and interact with your Django app programmatically.

Some of the things you can do with the test client are:

- Simulate GET and POST requests on a URL and observe the response – everything from low-level HTTP (result headers and status codes) to page content.
- See the chain of redirects (if any) and check the URL and status code at each step.
- Test that a given request is rendered by a given Django template, with a template context that contains certain values.

This framework by no means is meant to replace [Selenium](#selenium). It has a different focus. In short:

- Use Django’s test client to establish that the correct template is being rendered and that the template is passed the correct context data.
- Use in-browser frameworks like Selenium to test *rendered* HTML and the *behavior* of Web pages, namely JavaScript functionality. 

In the best case scenario, teams would use both the Django test client and Selenium.

To use the test client, instantiate `django.test.Client` within a session of the Python interactive interpreter and retrieve Web pages:

```python
>>> from django.test import Client
>>> c = Client()
>>> response = c.post('/login/', {'username': 'john', 'password': 'smith'})
>>> response.status_code
200
>>> response = c.get('/customer/details/')
>>> response.content
b'<!DOCTYPE html...'
```

Note a few important things about how the client works:

- The test client does not require the Web server to be running. In fact, it will run just fine with no Web server running at all! That’s because it avoids the overhead of HTTP and deals directly with the Django framework. This helps make the unit tests run quickly.
- When retrieving pages, remember to specify the path of the URL, not the whole domain. The client is not capable of retrieving Web pages that are not powered by your Django project. If you need to retrieve other Web pages, use a Python standard library module such as <a href="https://docs.python.org/2/library/urllib2.html">urllib2</a> or <a href="http://docs.python-requests.org/en/master/">requests</a>.
- To resolve URLs, the test client uses whatever URLconf is pointed-to by your <a href="https://docs.djangoproject.com/en/1.9/ref/settings/#std:setting-ROOT_URLCONF">ROOT_URLCONF</a> setting.
- By default, the test client will disable any CSRF checks performed by your site. If you want the test client to perform CSRF checks, you can create an instance of the test client that enforces CSRF checks. To do this, pass in the enforce_csrf_checks argument when you construct your client: 
`csft_client = Client(enfore_csrf_checks=True)`.

The Django Client can perform `GET` and `POST` requests and should be looked at closely before beginning testing. The full documentation can be found <a href="https://docs.djangoproject.com/en/1.9/topics/testing/tools/#django.test.Client">here</a>.

##### Testing Django with Mocking

It is also possible to run [mocks](#mocking) using Django. Mocking can be done using the <a href="https://pypi.python.org/pypi/mock/">mock</a> library in conjunction with Django and <a href="https://docs.python.org/2.7/library/unittest.html">unittest</a>. Mocking generally uses unittest's `TestCase` object as opposed to the django `TestCase` object because it saves the database cleanup between tests.

```python
from django.db import models
 
class SampleManager(models.Manager):
    def get_by_user(self, user):
        self.filter(user=user)
 
class Sample(models.Model):
    pass
```

```python
import unittest
import mock
from django_testing import models
 
class SampleTests(unittest.TestCase):
    def test_filters_by_user(self):
        user = mock.Mock()
        manager = mock.Mock(spec=models.SampleManager)
        models.SampleManager.get_by_user(manager, user)
        manager.filter.assert_called_with(user=user)
```

Let's walk though the test line by line. <br>
1. Create a mock user. `user = mock.Mock()` <br>
2. Create a mock manager. `manager = mock.Mock(spec=models.SampleManager)` <br>
3. Call the method that you want to have do something. You'll notice something a bit tricky here by using the actual `SampleManager` class and passing the manager mock object in as the self argument. This allows you to capture what the implementation code does with the manager inside the `get_by_user` method. `models.SampleManager.get_by_user(manager, user)`<br>
4. Assert that your desired result occured. Here, you are asserting that the filter method of the manager mock object was called with the keyword user argument with the value of your user mock. `manager.filter.assert_called_with(user=user)`<br><br>

Let's take a look at a different way to write that same test.

```python
@mock.patch('django_testing.models.SampleManager.filter', mock.Mock())
    def test_filters_by_user_with_patch(self):
        user = mock.Mock()
        models.Sample.objects.get_by_user(user)
        models.Sample.objects.filter.assert_called_with(user=user)
```

Here, the `mock` library's patch decorator to mock the `filter` method on the `SampleManager` class instead of using the 'mock as self' trickery. Let's look at one more way to write this test.


```python
@mock.patch('django_testing.models.SampleManager.filter')
    def test_filters_by_user_with_patch_and_filter_passed_in(self, filter_method):
        user = mock.Mock()
        models.Sample.objects.get_by_user(user)
        filter_method.assert_called_with(user=user)
```

In this example, the patch decorator is used a little bit differently. By omitting the second argument, the patch decorator will pass the mock into your test method which will then allow you to do assertions directly against it. Now, say you want to check for specific return values, consider this test.

```python
@mock.patch('django_testing.models.SampleManager.get_last')
    @mock.patch('django_testing.models.SampleManager.get_first')
    def test_result_of_one_query_in_args_of_another(self, get_first, get_last):
        result = models.Sample.objects.get_first_and_last()
        self.assertEqual((get_first.return_value, get_last.return_value), result)
```

You want to make sure that the result of `get_first_and_last` returns a tuple of the result of `get_first` and `get_last`. Our implementation code would look like this.

```python
from django.db import models
 
class SampleManager(models.Manager):
    def get_first(self):
        pass
    def get_last(self):
        pass
    def get_first_and_last(self):
        return self.get_first(), self.get_last()
 
class Sample(models.Model):
    objects = SampleManager()
```

That is really all there is to getting started with mocking Django. There are a few more advanced things that can be found <a href="http://chimera.labs.oreilly.com/books/1234000000754/ch16.html#_checking_the_view_actually_logs_the_user_in">here</a>.


#### JavaScript
Since Lagunitas uses a Django back-end, we do not forsee any use of a JavaScript testing framework other than [Selenium](#selenium) because of its ability to perform test automation especially on browser compatibility; however, we have compiled a list of JavaScript unit testing tools that are TDD compliant that we found <a href="http://stackoverflow.com/questions/300855/javascript-unit-test-tools-for-tdd/680713#680713">here</a>.

A relatively simple front-end testing tool is <a href="http://docs.casperjs.org/en/latest/quickstart.html">CapserJS</a>. It runs on <a href="http://phantomjs.org/">PhantomJS</a> and relies on [integration tests](#integration-tests) in order to thoroughly test the front-end of your platform.

Compared to Selenium GUI interface, CasperJS is fast, simple, and can be run from the command line.

##### CasperJS

To use Casper, you simply write some JavaScript, save it to a file, then run it from the command line like so: `casperjs my-source.js`. If you will be running unit tests, you must include the `test` command, like so: `casperjs test my-test.js`. All of the examples below should be run with the `test` command. 

Casper has a fantastic API full of convenience methods to help you interact with your phantasmic browser. There are two main modules that you can use, the <a href="http://docs.casperjs.org/en/latest/modules/casper.html">casper module</a> and the <a href="http://docs.casperjs.org/en/latest/modules/tester.html">tester module</a>. Methods in the tester module are only available when you run Casper with casperjs test `my-test.js`.

As an example, let's look at what the `casper` module can test on <a href="http://www.reddit.com/">http://www.reddit.com</a>.

```javascript
casper.options.viewportSize = {width: 1024, height: 768};
casper.start("http://www.reddit.com/", function() {
	console.log('Opened page with title \"' + this.getTitle() + '"');
    casper.capture("../images/reddit-home.png");
}).run();
```

The `start()` call opens the page and executes the callback when it's loaded. The `capture()` functionality allows you to take a screenshot and save it in the form of a PNG. Saving screenshots in this manner can be a great part of your functional tests, and can be hugely helpful as a debugging tool when writing your tests, helping you "see" what's going on in the invisible browser.

Now let's take a look at Casper's test API. Let's open up the /r/programming subreddit, click the "New" link and confirm that we're on the right page and have the correct content.

```javascript
casper.options.viewportSize = {width: 1024, height: 768};
var testCount = 2;
casper.test.begin("Testing Reddit", testCount, function redditTest(test) {
    casper.start("http://www.reddit.com/r/programming", function() {
    	test.assertTitleMatch(/programming/, "Title is what we'd expect");
    	//Click "new link"
    	casper.click("a[href*='/programming/new/']");
    	casper.waitForUrl(/\/programming\/new\/$/, function() {
    		test.assertElementCount("p.title", 25, "25 links on first page");
    		casper.capture("../images/reddit-programming-new.png");
    	});
    }).run(function() {
        test.done();
    });
});
```

Here, after we click the "New" link, we wait for the url to change and a new page to load. Then we confirm that there are 25 links on the page, the Reddit default.

Casper's API is chock full of handy helper methods like `click()` and `assertElementCount()`. Let's look at a more complicated method, `fill()`. `fill()` is a convenient way to fill out and (optionally) submit forms on the page. In this example, let's fill out the search box form and search for "javascript" within the /r/programming subreddit.

```javascript
casper.options.viewportSize = {width: 1024, height: 768};
var testCount = 1;
casper.test.begin("Searching Reddit", testCount, function redditSearch(test) {
    casper.start("http://www.reddit.com/r/programming", function() {
    	//Search for "javascript"
        casper.fill("form#search", {
            "q": "javascript",
           "restrict_sr": true
        }, true);
        casper.then(function(){
            test.assertElementCount("p.title", 25, "Found 25 or more results");
            this.capture("../images/Reddit search.png");
        });
    }).run(function() {
        test.done();
    });
});
```

Sometimes, to really test something complicated, you need to jump into the Document Object Model (DOM) of the browser itself. Casper provides the ability to do just that with the <a href="http://docs.casperjs.org/en/latest/modules/casper.html#evaluate">`evaluate()`</a> method. If you find this a bit confusing, they have a nice <a href="http://docs.casperjs.org/en/latest/_images/evaluate-diagram.png">diagram</a> to help you picture this.

Here's an example where we will jump into the context of the r/programming page, click on upvote, confirm that the login modal appears, then click on the `.close` backdrop and confirm that the modal disappears. The results are then returned to the Casper environment.

```javascript
casper.options.viewportSize = {width: 1024, height: 768};
var testCount = 1;
casper.test.begin("Testing upvote login", testCount, function upvoteLogin(test) {
    casper.start("http://www.reddit.com/r/programming", function() {
    	var modalOpensAndCloses = casper.evaluate(function(){
            console.log("Now I'm in the DOM!");
            $("div.thing:first .arrow.up").click()
            var modalVisibleAfterClick = $(".popup").is(":visible");
            $(".cover").click()
            var modalClosedAfterClickOff = $(".popup").is(":visible");
            return (modalVisibleAfterClick && !modalClosedAfterClickOff);
        });
        test.assert(modalOpensAndCloses, "Login Modal is displayed when clicking upvote before signing in");
    }).run(function() {
        test.done();
    });
});
```

An important thing to note when using `casper.evaluate()` is that unlike almost any other interaction that occurs with the browser (`click()`, `fill()`, etc.), evaluate is synchronous. Whereas after calling fill or `sendKeys` you will need to wrap your next interaction in a `casper.then()` callback, evaluate happens instantly. This actually makes it easier to use than some of its asynchronous counterparts, but after a while you get used to asynchronous and have to remind yourself that evaluate is synchronous.

#### Selenium

Django supports Seleium test case integration, which can help facilitate the testing process, with the class `LiveServerTestCase` within `django.test`. This class will automatically run a test server in the background and your Selenium tests will be run against that server. It not only simplifies the process, but it also means your tests can be setup the same way your Django tests are, and so both can be run together without any additional setup using the standard `manage.py test` command.

Selenium tests can be setup inside each app’s test directory just like a regular test would be, but since they’re very tied to the specific project, we suggest creating an app where all the tests are stored.

Here is an example of what to include in `test.py`:

```python
class SeleniumTestCase(LiveServerTestCase):
    """
    A base test case for Selenium, providing helper methods for generating
    clients and logging in profiles.
    """

    def open(self, url):
        self.wd.get("%s%s" % (self.live_server_url, url))
```

Here we defined our base test class and helper methods. We have an “open” helper because Selenium needs an absolute URL (including server and port), but you can also define methods for common operations like “sign in”, “sign out”, etc.. that you may need throughout all the tests.

Because the default webdriver query methods are very verbose and missing some important features, it can be useful to create your own web driver in `webdriver.py`.

```python
class CustomWebDriver(SELENIUM_WEBDRIVER):
    """Our own WebDriver with some helpers added"""

    def find_css(self, css_selector):
        """Shortcut to find elements by CSS. Returns either a list or singleton"""
        elems = self.find_elements_by_css_selector(css_selector)
        found = len(elems)
        if found == 1:
            return elems[0]
        elif not elems:
            raise NoSuchElementException(css_selector)
        return elems

    def wait_for_css(self, css_selector, timeout=7):
        """ Shortcut for WebDriverWait"""
        return WebDriverWait(self, timeout).until(lambda driver : driver.find_css(css_selector))
```

In this example, we have only one method, which is very handy:

- `find_css` is basically a shortcut for both “find_elements_by_css_selector” and “find_element_by_css_selector”, and as the method names imply, it enables you to find DOM elements using regular CSS selectors.
- `wait_for_css` is a helpful method to block the test from progressing until a element (css selector) is found on the page. This method is commonly used in non-blocking instructions (such as AJAX requests) or some JavaScript interactions that do DOM manipulation. Whenever you get an “element not found” error due to JavaScript being slower than your test instructions, this is the method to use.

As you start writing more and more tests, you’ll find yourself writing similar code to deal with selects or custom widgets for example. webdriver.py is where you can add these so your tests can be less repetitive. As a rule of thumb, We tend to use `webdriver.py` to add “generic” shortcuts and `test.py` for “app specific” shortcuts (like sign in).

Here is an example webdriver test that tests admin sign in:

```python
# Make sure your class inherits from your base class
class Auth(SeleniumTestCase):

    def setUp(self):
        # setUp is where you setup call fixture creation scripts
        # and instantiate the WebDriver, which in turns loads up the browser.
        User.objects.create_superuser(username='admin',
                                      password='pw',
                                      email='info@lincolnloop.com')

        # Instantiating the WebDriver will load your browser
        self.wd = CustomWebDriver()

    def tearDown(self):
        # Don't forget to call quit on your webdriver, so that
        # the browser is closed after the tests are ran
        self.wd.quit()

    # Just like Django tests, any method that is a Selenium test should
    # start with the "test_" prefix.
    def test_login(self):
        """
        Django Admin login test
        """
        # Open the admin index page
        self.open(reverse('admin:index'))

        # Selenium knows it has to wait for page loads (except for AJAX requests)
        # so we don't need to do anything about that, and can just
        # call find_css. Since we can chain methods, we can
        # call the built-in send_keys method right away to change the
        # value of the field
        self.wd.find_css('#id_username').send_keys("admin")
        # for the password, we can now just call find_css since we know the page
        # has been rendered
        self.wd.find_css("#id_password").send_keys('pw')
        # You're not limited to CSS selectors only, check
        # http://seleniumhq.org/docs/03_webdriver.html for 
        # a more compreehensive documentation.
        self.wd.find_element_by_xpath('//input[@value="Log in"]').click()
        # Again, after submiting the form, we'll use the find_css helper
        # method and pass as a CSS selector, an id that will only exist
        # on the index page and not the login page
        self.wd.find_css("#content-main")
```

This is an introduction to how Django/Selenium testing can work. Of course, one can test in the Selenium GUI. More info on that can be found <a href="http://www.seleniumhq.org/docs/02_selenium_ide.jsp">here</a>. 

Advanced testing topics in Django/Selenium-specific testing can be found <a href="http://agiliq.com/blog/2014/09/advanced-functional-testing-with-selenium/">here</a>.
