---
title: "QA and Security Pt. 2: Software Testers"
date: "2014-09-10T13:57:09"
slug: "qa-and-security-pt-2-software-testers"
categories: ["Uncategorized"]
tags: ["burp", "processes", "QA", "Quality Assurance", "sdlc", "security training", "Software", "software security", "training", "zap"]
---

Lets start by discussing the type of QA that tests software to ensure it is working as designed. These are the bug finders of the QA world. The process starts by a developer creating some functional code and then deploying it to an environment specific to the QA team. The QA team then uses its knowledge of how the application is supposed to function and determines if it is performing properly. Any bugs identified get filed and sent back for remediation (hopefully).

There are some pretty major differences between different companies, and maybe within some companies, in how technical a QA team actually is. Many companies I have worked at, the QA team was not as technically savvy as I would have expected. They would use Microsoft Excel spreadsheets to define their test scripts. In this case the word script is used as a set of processes to follow, not automation. Here is how the process would go.

- Open Internet Explorer
- Type http://www.oursite.com into the address bar and hit go
- Enter Valid Username (james)
- Enter Valid Password (password)
- Submit Login Form
- Verify the user is logged in and redirected to Default.html
- Pass/Fail

The above example may be a little over the top, but this is how a lot of scripts are designed. There is not a lot of thinking outside of the box when it comes to testing the functionality. While that may sound bad, there are some pros to the approach. For example, it ensures that the app is tested the same way every time which can help reduce the chance of a previous test being missed later on. These scripts are actually very easy to automate as well. On the other hand, it opens up the opportunity for a lot of good testing to not be performed.

Many companies are even using automation tools to help out with scripts like the one above. LoadRunner and Selenium come to mind as frameworks to help with this. These tools can record your testing and then play them back repeatedly. This helps when it comes time to re-test an application you can re-run your previous scripts automatically, allowing the manual tester to free up some time to look at other stuff. Of course, as you test more things you would hope that those tests also become automated.

In other companies, the QA testers are very technical. They work much more with creating custom tools to help with testing the applications. They are more savvy regarding how the application works and different ways that users will use it. With the addition of the automated tools, it makes sense to have testers with these capabilities to really dig into testing the application.

In many cases, we are not seeing a lot of QA teams embracing security testing. There are many people that want to believe that many security flaws cannot be tested by your normal QA department. I am not one of those people. I think to help increase security, we need to increase our internal QA capabilities. We hear that there is a shortage of security professionals, but really, we just need to enlist the current professionals in the development process to help out and we will be better off.

You might wonder why I think QA can find many of these security flaws. To be blunt, most of these security issues are just bugs. Bugs are what QA people (in the current context) do. There is no need for so much segregation between QA and security testing. I understand that we don’t want to share our bowl of rice or think that someone else could actually do that, but they can. Lets look at a few examples that QA should be able to test for.

Injection Bugs are the first on the list. While I group these together, this refers to things like OS Injection, SQL Injection, LDAP Injection, Cross-Site Scripting, XML Injection, etc. These types of bugs are typically not that difficult to identify. Before you jump all over my back, I will agree that some of them can be very difficult to identify. I don’t see that as the norm though. The method: Input known control characters for the current context and look at the output.

For example, enter a single quote (‘) into a field and examine the response. Did you get a database error message? If yes, good chance you have SQL injection – investigate further. No error message? Were there other indicators of a flaw? How about the response time? Did it change drastically from a request without that malicious value? Obviously there are other tests involved in this, but this shows how simple some of the tests can be to help identify the easy vulnerabilities. Will it find all of them, probably not, but it is better than finding none. We should not be relying on outside parties to identify a SQL Injection flaw by entering a (‘) into a field. This should be found internally.

Cross-Site Scripting is another good example. Just like above, enter in known control characters for this bug and analyze the output. If you enter an HTML tag does it get sent back without being encoded? How does the application react? Does it validate input? Does it encode output?

Another set of bugs are related to authentication and authorization. Again, this is something simple that QA can test. Verifying if restricted resources are only available to those given permission is a simple process. Same goes for verifying that a user must be authenticated properly before accessing the system. Many times during a penetration test we are given different sets of credentials to help facilitate the testing of auth bugs. Create a map of what pages/functions each role should have access too. Then check to ensure the other roles cannot access those pages/functions. You might actually be surprised at how much more you know about your application after mapping it like this.

Much of this testing can be accomplished using simple tools such as your browser, plugins, and proxies like Burp or ZAP. There is no good reason for why software testers don’t have and understand how to use these tools during their testing. Lets stop the “We don’t allow hacking tools” non-sense. Many of these tools were started as just testing tools, not hacking tools. If you are not providing QA the tools they need, you will never get better at assuring the quality of your software.

If you fall into the category of the less technically savvy personnel work to get them some security training. Not only will it benefit you by better security testing, but it will also make that person much more knowledgable in general and most likely less prone to falling for phishing or social engineering attacks.
