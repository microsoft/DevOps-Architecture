# The Professional-Grade DevOps Environment

## A Philosophical Review of DevOps
DevOps started as a recognition of some dysfunction that had become ingrained into the software development lifecycle (SDLC), even within teams attempting to use Agile methods. Ever since the dawn of the first multi-user mainframe with networked terminals, organizations have struggled to balance the need to keeping systems running in a stable fashion even while continually changing them to meet additional business scenarios. Over the following decades, the industry formalized the divide in job description for people who held these responsibilities. The original Computer Programmer became divided into the Software developer and the Systems Administrator. Microsoft has long organized it's flagship technology conferences into sessions geared for Developers and those geared toward IT Professionals. Today, entire job descriptions and departments exists for each. Many large companies have consolidated their IT Professionals in order to maintain standards and consistency as well as cost efficiency as they strive to operate stable, available computing systems. They have learned along the way that this desire is inherently in conflict with the goals and objectives of the developers, which is to move fast, change the systems, and provide new capabilities to those seeking to use the software. Companies today are using custom software applications to connect with their customers. This makes software a part of strategic revenue generation, and speed is more important now than ever.

In 2001, a group of industry leaders met at a ski resort to mull over the problems in the software industry that had been building throughout the 1990s and the infamous .Com bubble of the late '90s. This group produced the Manifesto for Agile Software Development (www.agilemanifesto.org). This meeting the resulting principles have fundamentally changed the way the industry organizes and executes software development work. A fundamental premise is to organize and perform work in much smaller batches than had previously been used in the late '90s. Now, units of software delivery called "sprints" or "iterations" are commonly discussed. Many organizations run in cadences of iterations that are 1-3 weeks in length. The unit of software changes has also shrunk to the size of a change that can be accomplished in this iteration, and many teams experiment with just how small software changes can become while keeping the software stable and releasable at all times. 

In 2004, Michael Feathers wrote a book called "Working Effectively with Legacy Code". This book came out in the years following the Manifesto for Agile Software Development, and it addressed a real situation that many organizations were faced with: how can I cahnge my software when every change generates at least two new defects? As teams were attempting to make changes and re-stabilize their software, they realized that previous engineering methods were insufficient when attempting to drive cycle times down to less than a month. The book goes into painstaking detail describing methods for breaking apart code bases that were never intendend to execute outside of a completely integrated production environment. The author summarizes that without running the software within automated test harnesses, any piece of software, however new, is destined to become labelled as "legacy code", unchangeable, brittle, and expensive to maintain. The techniques included in the book illustrated how to insert seams into existing code for the purpose of retrofitting tests. Then, armed with tests that protected the functionality, the software could be changed with less fear. Michael Feathers was also the author of some early unit testing framework for C++,  namely CppUnit.

Shortly thereafter, in 2006, Paul M. Duvall, Steve Matyas, and Andrew Glover wrote a very influential book, "Continuous Integration: Improving Software Quality and Reducing Risk". This book illustrated a method that some in the industry had been perfecting. This method was called Continuous Integration. This method sought to run an automated build of the software application every time any change was committed to the version control system. In order to accomplish this, some specific engineering methods have to be adopted, and the book illustrated them. A ground-shaking inclusion was the premise that the continuous integration build must include the building and testing of dependencies that an application owns, including relational databases and other storage mechanisms. The authors dedicated an entire chapter (chapter 5) to "Continuous Database Integration".  Additionally in 2006, Martin Fowler penned an article entitled "Continuous Integration" (https://www.martinfowler.com/articles/continuousIntegration.html). In this article, he proposes some standards for a continuous integration build:
* Maintain a Single Source Repository.
* Automate the Build
* Make Your Build Self-Testing
* Everyone Commits To the Mainline Every Day
* Every Commit Should Build the Mainline on an Integration Machine
* Fix Broken Builds Immediately
* Keep the Build Fast
* Test in a Clone of the Production Environment
* Make it Easy for Anyone to Get the Latest Executable
* Everyone can see what's happening
* Automate Deployment

While many in the industry were innovating new and better methods for shortening cycle time, 2006 was the year when successful and proven methods were being shared opening and published widely. Continuous Integration did include the automated deployment of software releases. In the years that followed, continuous integration become known as Continuous Delivery in order to expressly imply the inclusion of software deployments all the way to end-users in production environments. Some people refer to the method as CI/CD given that confusion exists to this day about where CI stopped and CD started. In short, both refer to the full process of integrating source code from multiple developers and providing new working software to end-users. However, in the common lexicon, most refer to the "CI build" and then refer to a CD pipeline that deploys across successive environments. Historically, however, continuous integration, as a method, in 2006 included automated deployments to production.

2009 saw another seminal work published. Jez Humble and David Farley authored "Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation". This book cited the 2006 book on continuous integration and pulled the story forward by proposing methods by which to not only build continuous but also deploy to downstream environments in a continuous fashion. More proven methods for handling deployment scenarios was included. The stages proposed in this book are:
* Version control
* Commit stage
* Automated acceptance tests
* Manual validations
* Release (production)
This process is much more high level, but the commit stage includes the private build and the integration build, and the automated acceptance tests are prescribed to run against a fully-deployed pre-production environment.

In 2010, at a conference in ______, the term DevOps was coined accidentally by ____

    * Methodologies that have contributed to the current state of the art
        * Agile 2001
        * CI 2006 (Addision Wesley book)
            * CruiseControl.net
        * CD 2009 (Addision Wesley book)
        * Working Effectively with Legacy Code (Michael Feathers)
        * Phoenix Project, DevOps Handbook (Jez Humble, etc)
        * DevOps (since 2010, but still misunderstood)
            * The ways of devops.

## Our Sample App
    * Sample app intro (ASP.NET Core & SQL database)
        * .Net Core 3. quick walkthrough of our application shell - setting ourselves up for the ability to write features.
        * Azure subscription - PaaS
          - rule of thumb is new Azure subscription for a new software system. May combine pre-production if multiple applications owned by same team. Could other mechanism work? Yes. Once you know why you are doing it.

## Professional Tools
    * Professional Tools
       * Visual Studio 2017 Ent. subscription.
         * Options and why to choose Enterprise subscription
         * What types of add-ins to consider and encouraging to use the extension marketplace
       * SQL Server Express 2017 min.  SQL Developer edition
         * What benefits come from SQL Developer edition vs. express
         * How to set up SQL Express for development including TCP/IP network enablement
       * Azure DevOps Service organization (Introduction to Azure DevOps Services & the different areas)
         * Setting up the organization and a project for working thro8ugh this book
         * Navigating the process templates - overview of process templates - doesn't matter which one you choose because you will be redoing it anyway.
         * Naming projects, git repositories - no spaces in the project name.
         * Encouragement about the marketplace and some interesting extensions
            * Octopus Deploy
            * Redgate (database)
            * DevLab

## The Fundamental Requirements of a Modern Development Environment
    * Principles: short lead time, short cycle time, defects prevented at every stage
    * Tracking work
    * Tracking code
    * Building the code
    * Validating the code
    * Creating a release candidate
    * Provisioning/configuring server environment
    * Deploying the release
    * Operating/supporting the software release
