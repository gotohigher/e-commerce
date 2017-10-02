<!--- # Node.JS Best Practices -->
<!--- ![Node.js Best Practices](assets/images/banner-2.jpg) -->
<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Node.js Best Practices" />
</h1>

<img src="https://img.shields.io/badge/⚙%20Item%20count%20-%2053%20Best%20practices-blue.svg" alt="53 items"> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%206%20days%20ago-green.svg" alt="Last update: 7 days ago"> <img src="https://img.shields.io/badge/%E2%9C%94%20Updated%20For%20Version%20-%20Node%208.4-brightgreen.svg" alt="Updated for Node v.8.4">

<!--- ![Node.js Best Practices](assets/images/banner-1.png) -->

# Welcome to Node.js Best Practices

Welcome to the biggest compilation of Node.JS best practices. The content below was gathered from all top ranked books and posts and is updated constantly - when you read here rest assure that no significant tip slipped away. Feel at home - we love to discuss via PRs, issues or Gitter.

## Table of Contents
* [Project Setup Practices (18)](#project-setup-practices)
* [Code Style Practices (11) ](#code-style-practices)
* [Error Handling Practices (14) ](#error-handling-practices)
* [Going To Production Practices (21) ](#going-to-production-practices)
* [Testing Practices (9) ](#deployment-practices)
* [Security Practices (8) ](#security-practices)


<br/><br/>
# `Project Setup Practices`

## ✔ 1. Structure your solution by feature ('microservices')

**TL&DR:** The worst large applications pitfal is a huge code base where hundreds of dependencies slow down developers as try to incorporate new features. Partioning into small units ensures that each unit is kept simple and very easy to maintain. This strategy pushes the complexity to the higher level - designing the cross-component interactions. 

**Otherwise:** Developing a new feature with a change to few objects demands to evaluate how this changes might affect dozends of dependants and ach deployment becomes a fear.

🔗 [**Read More: Structure by feature*](/sections/errorhandling/asyncawait.md)

<br/><br/>

## ✔ 2. Layer your app, keep Express within its boundaries

**TL&DR:** It's very common to see Express API passes the express objects (req, res) to business logic and data layers, sometimes even to every function - this makes your application depedant on and accessible by Express only. What if your code should be reached by testing console or CRON job? instead create your own context object with cross-cutting-concern properties like the user roles and inject into other layers, or use 'thread-level variables' libraries like continuation local storage

**Otherwise:** Application can be accessed by Express only and require to create complex testing mocks

🔗 [**Read More: Structure by feature*](/sections/errorhandling/asyncawait.md)

<br/><br/>

## ✔ 3. Configure ESLint with node-specific plugins

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my sug

**Otherwise:** You end-up with a blackbox that is hard to reason about, then you start re-writing all logging statements to add additional information

🔗 [**Read More: Structure by feature*](/sections/errorhandling/asyncawait.md)


<br/><br/>
# `Code Style Practices`


<br/><br/><br/>
# `Error Handling Practices`
<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

## ✔ 1.  Use Async-Await or promises for async error handling

**TL&DR:** Handling async errors in callback style is probably the fastest way to hell (a.k.a the pyramid of doom). The best gift you can give to your code is using instead a reputable promise library or async-await which provides much compact and familiar code syntax like try-catch

**Otherwise:** Node.JS callback style, function(err, response), is a promising way to un-maintainable code due to the mix of error handling with casual code, excessive nesting and awkward coding patterns

🔗 [**Read More: avoiding callbacks*](/sections/errorhandling/asyncerrorhandling.md)

<br/><br/>

## ✔ 2. Use only the built-in Error object

**TL&DR:** Many throws errors as a string or as some custom type – this complicates the error handling logic and the interoperability between modules. Whether you reject a promise, throw exception or emit error – using only the built-in Error object will increases uniformity and prevents loss of information


**Otherwise:** When invoking some component, being uncertain which type of errors come in return – makes it much harder to handle errors properly. Even worth, using custom types to describe errors might lead to loss of critical error information like the stack trace!

🔗 [**Read More: using the built-in error object*](/sections/errorhandling/useonlythebuiltinerror.md)

<br/><br/>

## ✔ 3. Distinguish operational vs programmer errors

**TL&DR:** Operational errors (e.g. API received an invalid input) refer to known cases where the error impact is fully understood and can be handled thoughtfully. On the other hand, programmer error (e.g. trying to read undefined variable) refers to unknown code failures that dictate to gracefully restart the application

**Otherwise:** You may always restart the application when an error appear, but why letting ~5000 online users down because of a minor, predicted, operational error? the opposite is also not ideal – keeping the application up when unknown issue (programmer error) occurred might lead to an unpredicted behavior. Differentiating the two allows acting tactfully and applying a balanced approach based on the given context

  🔗 [**Read More: operational vs programmer error*](/sections/errorhandling/operationalvsprogrammererror.md)

<br/><br/>

## ✔ 4. Handle errors centrally, not within an Express middleware

**TL&DR:** Error handling logic such as mail to admin and logging should be encapsulated in a dedicated and centralized object that all end-points (e.g. Express middleware, cron jobs, unit-testing) call when an error comes in.

**Otherwise:** Not handling errors within a single place will lead to code duplication and probably to errors that are handled improperly

🔗 [**Read More: handling errors in a centralized place*](/sections/errorhandling/centralizedhandling.md)

<br/><br/>

## ✔ 5. Document API errors using Swagger

**TL&DR:** Let your API callers know which errors might come in return so they can handle these thoughtfully without crashing. This is usually done with REST API documentation frameworks like Swagger

**Otherwise:** An API client might decide to crash and restart only because he received back an error he couldn’t understand. Note: the caller of your API might be you (very typical in a microservices environment)


🔗 [**Read More: documenting errors in Swagger*](/sections/errorhandling/documentingusingswagger.md)

<br/><br/>

## ✔ 6. Shut the process gracefully when a stranger comes to town

**TL&DR:** When an unknown error occurs (a developer error, see best practice number #3)- there is uncertainty about the application healthiness. A common practice suggests restarting the process carefully using a ‘restarter’ tool like Forever and PM2

**Otherwise:** When an unfamiliar exception is caught, some object might be in a faulty state (e.g an event emitter which is used globally and not firing events anymore due to some internal failure) and all future requests might fail or behave crazily

🔗 [**Read More: shutting the process*](/sections/errorhandling/shuttingtheprocess.md)

<br/><br/>



## ✔ 7. Use a mature logger to increase errors visibility

**TL&DR:** A set of mature logging tools like Winston, Bunyan or Log4J, will speed-up error discovery and understanding. So forget about console.log.

**Otherwise:** Skimming through console.logs or manually through messy text file without querying tools or a decent log viewer might keep you busy at work until late

🔗 [**Read More: using a mature logger*](/sections/errorhandling/usematurelogger.md)


<br/><br/>


## ✔ 8. Test error flows using your favorite test framework

**TL&DR:** Whether professional automated QA or plain manual developer testing – Ensure that your code not only satisfies positive scenario but also handle and return the right errors. Testing framework like Mocha & Chai can handle this easily (see code examples within the "Gist popup")

**Otherwise:** Without testing, whether automatically or manually, you can’t rely on our code to return the right errors. Without meaningful errors – there’s no error handling


🔗 [**Read More: testing error flows*](/sections/errorhandling/testingerrorflows.md)

<br/><br/>

## ✔ 9. Discover errors and downtime using APM products

**TL&DR:** Monitoring and performance products (a.k.a APM) proactively gauge your codebase or API so they can auto-magically highlight errors, crashes and slow parts that you were missing

**Otherwise:** You might spend great effort on measuring API performance and downtimes, probably you’ll never be aware which are your slowest code parts under real world scenario and how these affects the UX


🔗 [**Read More: using APM products*](/sections/errorhandling/apmproducts.md)

<br/><br/>


## ✔ 10. Catch unhandled promise rejections

**TL&DR:** Any exception thrown within a promise will get swallowed and discarded unless a developer didn’t forget to explictly handle. Even if you’re code is subscribed to process.uncaughtException! Overcome this by registering to the event process.unhandledRejection

**Otherwise:** Your errors will get swallowed and leave no trace. Nothing to worry about


🔗 [**Read More: catching unhandled promise rejection *](/sections/errorhandling/catchunhandledpromiserejection.md)

<br/><br/>

## ✔ 11. Fail fast, validate arguments using a dedicated library

**TL&DR:** This should be part of your Express best practices – Assert API input to avoid nasty bugs that are much harder to track later. Validation code is usually tedious unless using a very cool helper libraries like Joi

**Otherwise:** Consider this – your function expects a numeric argument “Discount” which the caller forgets to pass, later on your code checks if Discount!=0 (amount of allowed discount is greater than zero), then it will allow the user to enjoy a discount. OMG, what a nasty bug. Can you see it?

🔗 [**Read More: failing fast*](/sections/errorhandling/failfast.md)


<br/><br/><br/>
# `Going To Production Practices`
## ✔ 1. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>

## ✔ 2. Increase transparency using smart logging

**TL&DR:** Logs can be a dumb warehouse of debug statements or the enabler of a beautiful dashboard that tells the story of your app. Plan your logging platform from day  1: how logs are collected, stored and analyzed to ensure that the desired information (e.g. error rate, following an entire transaction through services and servers, etc) can really be extracted

**Otherwise:** You end-up with a blackbox that is hard to reason about, then you start re-writing all logging statements to add additional information


🔗 [**Read More: monitoring*](/sections/errorhandling/smartlogging.md)
	
<br/><br/>

## ✔ 3. Delegate anything possible (e.g. gzip, SSL) to a reverse proxy

**TL&DR:** Node is awfully bad at doing CPU intensive tasks like gzipping, SSL termination, etc. Instead, use a ‘real’ middleware services like nginx, HAproxy or cloud vendor services

**Otherwise:** Your poor single thread will keep busy doing networking tasks instead of dealing with your application core and performance will degrade accordingly


🔗 [**Read More: monitoring*](/sections/errorhandling/delegatetoproxy.md)

<br/><br/>

## ✔ 4. Lock dependencies

**TL&DR:** Your code must be identical across all environments but amazingly NPM lets dependencies drift across environments be default – when you install packages at various environments it tries to fetch packages’ latest patch version. Overcome this by using NPM config files , .npmrc, that tell each environment to save the exact (not the latest) version of each package. Alternatively, for finer grain control use NPM” shrinkwrap”. *Update: as of NPM5 , dependencies are locked by default. The new package manager in town, Yarn, also got us covered by default

**Otherwise:** QA will thoroughly test the code and approve a version that will behave differently at production. Even worse, different servers at the same production cluster might run different code


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>

## ✔ 5. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>

## ✔ 6. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>

## ✔ 7. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>

## ✔ 8. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 9. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 10. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 11. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 12. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 13. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 14. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 15. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 16. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 17. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 18. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 19. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 20. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


## ✔ 21. Monitoring!

**TL&DR:** Monitoring is a game of finding out issues before our customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that tick all boxes. Click ‘The Gist’ below for overview of solutions

**Otherwise:** Failure === disappointed customers. Simple.


🔗 [**Read More: monitoring*](/sections/errorhandling/monitoring.md)

<br/><br/>


<br/><br/><br/>
# `Deployment Practices`


<br/><br/><br/>
# `Security Practices`

