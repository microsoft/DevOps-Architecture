# The Professional-Grade DevOps Environment

## A Philosophical Review of DevOps
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