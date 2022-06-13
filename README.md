# Open Code Academy

<!-- TODO: Insert TOC -->

## Introduction

When I get on LinkedIn, I see countless heartbreaking posts about hundreds of applications, and rejections emails with no explanation.
I see people who could very well be fantastic engineers, publicly begging for help, because they don't know how to stand out from the wave of engineers entering the scene.
I know what it is like to feel that hopelessness.
I also know what it is like to persevere, and make it to the other side.

And, I know what I would like to see in an engineer coding across from me.

I'm Steven. You might know mee as @OleDakotaJoe
I started OpenCodeAcademy.org so that it would be a 100% free and open-source boot camp, with the mission of helping as many early
career engineers get their start in tech as possible. This organization exists to solve a very real catch 22 for early career engineers.
You can't get a job because you can't get experience, because you cant get a job, because you can't get experience.
Like this:

```
func lookForJob(Person p) {
    if (!p.hasExperience()) {
        lookForJob(p);
    }
    findJob(); // unreachable :D
}
```

Well. The answer? Open source.
I know, right... insert eye-roll.
How.
Where do you start?
What project? (lol this one :D)

OpenCodeAcademy.org will be a place to learn to code, then get gently led into the path of contributing to its own codebase.
This is how the organization will grow exponentially, and become **the community** to be a part of.
The goal?
Work with **industry standard** practices, building real software that you really interact with, and can really see the results in a real production environment.
Put that on your resume.

### PURPOSE STATEMENT

The purpose of this document is to provide an overview of the vision and roadmap of OpenCodeAcademy.org

This is so that others can refer back to this document and either amend it, or verify the original intent. Additionally, some information will be shared some of the initial thoughts around technologies that will be used, as well as a technical overview of the proposed MVP for OpenCodeAcademy.org. Some high-level architectural and functional details including the scope, testing plans, deployment expectations, performance expectations etc., will be discussed.

### OVERVIEW OF THE PROBLEM

Year after year, people of all ages turn to the tech industry for their own various reasons. Some do it to pursue passion, others to find a way up and out of their current situation, whatever that may be.

Inevitably, all are met with a simple conundrum: getting their first job.

For some, it may be easier. They may know someone. They may have competed in a hackathon and networked, or have shined in school or some other way, or even fell into a position. But for the majority? It is a plight of paradoxical proportion.
Once many people get their bearings in an IDE or editor, they are told "build projects, make sure it has a backend."
But what do you build?

The narrative could continue, but we can cut to the chase:

It's hard to discover what you don't know without being smacked in the face with it.

(figuratively, of course)

OpenCodeAcademy.org will usher early career engineers into real world scenarios, by having them slowly graduate to working on and participating in development of the very site that they are learning from.

Its like, kinda meta isn't it"?

### GOALS AND OBJECTIVES

The goals & objectives of this software solution are to provide a system which facilitates a community that allows engineers to share their knowledge and collaborate with others, and learn from a platform that they eventually help build.
Some key objectives are:

- Provide a platform for learning core programming and computer science fundamentals
- Provide functionalities for experienced or senior level engineers to contribute to the lessons and mentorship of early career engineers.
- Provide a pseudo-industry like feel and structure that allows early career engineers to work on realy problems in many areas of the tech stack
- Foster a community and public reputation of excellent software built by the students of this program so that large tech companies will eventually take note and build a talent pipeline (grabbing up our students)
- Create a community where others can look to for support of all kinds
- Build innovative software

### PREREQUISITES

A few pre-requisites must take place before the solution can move forward.

- Must open up AWS Accounts
- Must Set up an Access control method for codebase
- Must set up access to external accounts such as npm accounts, etc, in a secret store somewhere.
- Must set up secure strategy and mechanisms of providing minimum viable access to "relative strangers" to commit code and work within environments.

### SCOPE

The Minimum Viable Product (MVP) will be the first iteration of OpenCodeAcademy.org
The following features must be present:

- Ability for users to log in and create a profile
- Ability for users to track progress
- Users must be verified (how?) in order to get access to the application
- Simple curriculum for either computer science, or programming fundamentals in place.
- Should support videos
- Should support uploading code, and running it in a sandboxed environment against code tests
- Users should be able to be managed by RBAC and ABAC
- System should be secure, and data should be secure
- All Testing and Deployments should be 100% automated

### ENVIRONMENT

The software solution will be a distributed system and will thus live in multiple environments, in Amazon Web Services (AWS). AWS is a cloud services provider with an extensive community and evolving software ecosystem.

All of the standards set forth here are subject to change, pending a discussion.

#### ACCESS MANAGEMENT

AWS provides a robust Identity and Access Management (IAM) solution for access to the infrastructure, and this will be used to ensure the integrity of the “supply chain” and systems.

We will use Red Hat SSO (Keycloak), and open-source Identity software that has a licensing model for support in case we ever need it, but is 100% open source as is. This can be integrated with an external Database for user federation (user and user metadata retrieval) if necessary, but may work fine out of the box. This will provide a JSON Web Token as well as validation keys, which will used for securely accessing the various components of the microservices architecture.

#### DEVELOPMENT/PRODUCTION ENVIRONMENTS

The system will be comprised of 3 environments. There will be a development (dev), testing (qa), production (prod) environment within 4 separate AWS accounts. All environments will be kept in parity, and will be controlled by a master security and internal-tools account for a total 6 AWS accounts. Students will be doing active development in the development environment. The qa environment exists to facilitate testing in an environment that has as much parity with the prod environment as possible, but that has no impact on end users. Any new features which are introduced will go through any end to end testing in this environment. Prod will be our live production environment. All access to infrastructure will be limited to read only, and must be manipulated through Infrastructure as Code (IAC). We will use Terraform for our infrastructure as code tool.

#### SECURITY

An external Security AWS account will exist to handle all access control for automated systems which are responsible for maintaining the cloud infrastructure. The addition of the security account will be a critical step in site reliability and cybersecurity. This will allow a single point to shutdown all access to internal tooling systems in the event of a “supply chain attack”.

The security account will be managed by only trusted, long standing members of the community. Any infrastructure around accessign the various environments, and around internal infrastructure IAM roles and permissions will be handled by this account.

#### INTERNAL TOOLING

An AWS account will also be stood up which will be leveraged for internal tooling systems, such as artifact repositories, source code repositories, and agile ticketing systems like Jira, and the automation server (Jenkins) which handles continuous integration and continuous deployment. Jenkins will run in an ECS instance, so that it is always available for deployments, and will not hinder development work.

#### ARCHITECTURE

Each component of the system will be built using a microservice architecture, so that redundancy and scaling can be easily achieved. AWS Lambda will be used for any single use or seldom used functionalities, such as batch processing (seldom used) as well as authorization (single use), and other serverless functionalities.

Front end technologies will be built using a micro-frontend strategy, using the model – view – view model (MVVM) architecture. This architecture will allow front end components to be decoupled from the data layer and will allow components to be re-used and faster development, as a result.

#### PROGRAMMING LANGUAGES

##### Backend

Amazon Corretto 11 will be the flavor of Java used for backend microservices. All client-facing front-end work will be served using Amazon Cloudfront, a distributed content delivery network. This will ensure blazing fast speeds, caching, and reliable content delivery. Each “microservice” which comprised the distributed system will be containerized in what is known as Docker Containers. These are Linux based virtual machines, which can be configured in any way.

##### DevOps

Groovy (based on Java) will be used when developing custom tooling for Jenkins automation server.

Docker will be for build and deployment environments for applications.

Terraform will be used as an infrastructure as code (IAC) tool and will ensure parity across multiple environments, enhancing code quality and testability.

##### Front-End

Typescript, in conjunction with React.js will be used on the front end. Typescript is a superset of JavaScript, which is statically typed and compiled. This can be used in conjunction with bundling tools to automatically compile to a version of JavaScript which is compatible with a vast array of browsers. React will be used because it is a fast and versatile JavaScript library which allows for rapid development of new feature using re-usable components. Storybook will be used for presenting the UI Library and Jest will be used for testing, and cypress for the hopefully seldom end to end test

#### DATA

For databases, AWS RDS in combination with Aurora Postgres will be used because of the advantages it gives when working in the cloud, as well as for cost reasons, since Postgres is free and open-source software. An agnostic database management tool called liquibase will be used for managing changes to Relational Databases.

If a NoSQL Solution is to be used, MongoDB will be used.

If/when caching comes up, probably Redis but lets keep it open for discussion for now.

#### MONITORING

A system of this complexity will undoubtedly require monitoring and reporting to make it maintainable. For this reason, a technology will need to be selected and documented. Cloudwatch supports logging, but a consolidated view of logging with tracing would be ideal.
Logging should be built into every microservice, and ideally tracing.

## REQUIREMENTS

The software solution must provide the following general features:

- Lesson upload, update, delete capabilities
- Identity and access management for users
- publicly availbe user signup
- Dashboard for user to track their progress
- Lessons support steps, or slides with different media inputs
- Lessons support coding challenges witht code testing (leetcode style)
- Different roles are identified so that members of the organization can manage lesson content
- Lessons support comment streams

### "BUSINESS" REQUIREMENTS

This is not meant to be a business, but it has to deliver value somehow!

- Must be free and open source wherever possible
- Must set up a way to accept donations
- Must have a great reputation to attract donors
- Must provide business value by creating a talent pipeline
- Must provide reliable means for identifying rising talent
- Must funnel students into contributing to the codebase

### USER REQUIREMENTS

The users of the system are perhaps the most important stakeholders in the success of the solution. For that reason, the following requirements have been identified:

- Must be easy to use
- Must be user-friendly
- Must be intuitive (natural user flows/journey)
- Must be easy to get support/help
- Must be logical cohesive lessons
- Lessons must be complete

### FUNCTIONAL REQUIREMENTS

The system must be capable of meeting the following functional requirements:

- Must have robust security, since anyone will be able to join
- Must be able to be deployed without downtime
- Must be able to recover if a disaster arises (backups, redundancy, etc)
- Must Support a CRUD operations on Lessons
- Must Scale to meet user demand
- Must allow access to users through SSO, so that their user profile on OpenCodeAcademy.org will be
  attached to the roles which control their access to repositories and code
- Must Integrate with Github SSO
- Must have robust identity and access management solution
- Must have the ability to restrict curriculum and lesson changes to certain roles
- Must have logging
- Must support 0 downtime deployments

### NONFUNCTIONAL REQUIREMENTS

For this system to be valuable, it must meet the following non-functional requirements:

- Must be reliable
- Must be performant
- Must contain quality code
- Must be eye appealing
- Code contribution and open source community must be structured in a way that delivers value to the students
  and recruiters who eventually pop in looking for talent
- Must use industry standard tooling and practices
- Application must protect serious students' time from fake accoutns/trolls/not serious students.

## Software Development Methodology

The best methodology for developing this learning solution, is the agile method. The agile methodology allows for a rapidly deployed minimum viable product, and then rapid improvement in iterations on top of that project.

However, because of the nature of this project, we will work in 3 week "sprints" instead of the industry standard 2 week sprint. After each sprint will be a one week "grace-period" where code can be completed, and we do the agile ceremonies: Review/Demo, Closing, Retrospective, Planning (we'll also lock in teams during this phase). Note that this structure will _not_ follow the calendar month cycle.

In order to protect students from investing their time in this project and it falling on its face, we must take this very seriously.
As such, we must implement standards to prevent fake accounts and trolls from gaining access to internal systems.
Before being allowed to contribute, all users will be required to pass an entrance assessment. This will assess VERY basic coding skills, nothing like leetcode. Just "have you even tried" kinds of tests.

After a student is able to pass a basic acceptance assessment, they can "mock interview" for their positions on a team for a sprint.
To be clear, a student will be allowed to join whatever team they want, but this will be a chance for students to get experience in an "interview-like setting". Hard part here will be finding people who want to lead teams. (i.e: mid-senior engineers who have the time, a.k.a. unicorns)

A note:
The "interview" is not going to be a "interview" where they are accepted or fail, it will exist more like a job fair near the beginning of each month, where students can learn about the project a "mentor" is leading, and decide to work on it or not. Students will be committing to putting in the effort, when the sign up or they can be put into a "probationary" status. This is TBD, and will be discussed accordingly.

Each student will have to COMMIT to a months' time, and there must be some probationary system in place to prevent trolling, abuse and people not taking it seriously. If this is going to work properly, when someone enters a team they must commit to delivering that feature by the end of the sprint. Afterward, they must participate in the ceremonies.

There must be some sort of accountability in the level of committment, but only for those people who have committed to a sprint. Open contribution could also be worked out, but the real value will come with the team and collaborative activities.

Each 1 month long sprint will end with the typical Agile Review/Demo, retrospective, and next sprint planning sessions.
These ceremonies can take place over the last week of the sprint, basically making the sprint a 3 weeks of development, and one week of planning/demoing/etc.

Strech goal: stream the demos live on youtube/twitch, etc.

### Why Agile?

The agile method is the best suited method for this software project for three key reasons:

1. The continuous integration of new features in the agile methodology works perfectly complements the advantages of the microservices architecture pattern, which allows the view layer of the application to be completely decoupled from the data. This will allow the OpenCodeAcademy.org to have very easy extensibility and integration with third party vendors and clients. Because Agile focuses on continuous integration and deployment and microservices allow developers to work on and improve isolated components of the system, a team of developers can rapidly improve and extend existing systems to accommodate new functionalities.
2. Agile will provide the fastest path to production available to OpenCodeAcademy.org’s users. This will allow OpenCodeAcademy.org to identify gaps during the qa testing stage, and iterate faster, while delivering value sooner as well.
3. Software is an iterative process, even in the waterfall methodology. After you deliver the product, you evaluate how to make it better, and set a new target. The Agile methodology takes advantage of the iterative nature of software and excels at delivering value and quality quickly.

## Roadmap

Here is a high level view of the project roadmap, which will be broken up into milestones

<!-- TODO: build out roadmap with links to below subsections -->

### AWS Foundations

#### Security AWS Account Created

#### Internal Tools AWS Account Created

#### Tooling created for applying infrastructure to each account.

#### Application Environments Infrastructure

#### Blue/Green System and Tooling Create

### Coming Soon Page

Needs Email Collection

### Identity Solution

Keycloak comes with RBAC and ABAC, as well as SSO, and many other features out of the box. For this reason, it has been chosen as the Identity solution.

![](./Identity.png)

1. UI -- Makes a call to Keycloak with login credentials which returns a JWT Returns JWT if credentials are valid
2. API Gateway
   Authorizer-- Calls made to resources on private network through
   API Gateway are authenticated with a Lambda Authorizer. This will effectively filter out unauthenticated traffic Validates traffic is from credible source.
3. Application -- Perform role and permission-based access controls based on the needs of the application Allow, deny, or modify response to conform to requirements

Must support internal team structure (requirements TBD)

### Lesson Service

API for managing storage of lessons and their metadata

### UserService

This service will manage a User's information, configurations, metadata, progress, etc. Can be relational or NoSQL

### Community-UI

Front End UI Library where all components will be built for the UI

### OpenCodeAcadmey UI

This is where the UI for OpenCodeAcademy will live.
Architecture is TBD

### CI/CD

We will use CI/CD principles throughout our development.
For that reason we will strive for high/effective test coverage, automated testing, automated deployment processes, and well documented code.

## Testing

Because this solution is heavily leveraging microservices, it is imperative that each individual component of the system continue working as expected, otherwise other systems which are dependent may break. To ensure code quality, integration tests will be written to ensure that APIs maintain a consistent “contract”, and developers are alerted when ci/cd pipelines have run that those tests have not passed. Additionally, unit testing will be used to ensure that the code’s business logic remains stable when iterating.

### Automated Testing

In an application running directly on a server, when you need to make improvements, you typically must take that application down for a short period of time in order deploy out the improved version. You might experience this when you go to your school’s website and see a notice that the site will be down for maintenance. Blue/Green testing and deployments defeat this inconvenience to end users by allowing 0 downtime deployments. We achieve this by running two versions of an application side by side and diverting only select traffic (with a marker) to an offline (green) environment for testing. If all of those tests pass, we will send a signal to our load balancer to flip the traffic over full time, thus changing that environment from green, to blue or live.

API testing during blue/green deployments will be completed with a technology called karate or cypress.

Karate is an API testing library and can be used for ensuring the correct response from backend APIs. Karate will eliminate the need for manual testing, by codifying API calls, and asserting that the response must match a certain criterion, or the test fails. These tests will be completed before any deployments in the blue/green deployments. The concept is that a pipeline running in our automation server, Jenkins, runs a series of steps that we define to deploy our applications. In blue/green, we deploy to an offline blue environment, and run automated tests (Karate).

Cypress is a front-end user interface (UI) testing library, and should be used only for “user-journey” type testing. Because this type of testing can be easily broken, it should be pragmatic and carefully used only for core user experience requirements which are considered to be critical. Similar to the scenario with karate, cypress tests can be ran automatically and their results reported within a Jenkins pipeline to deploy or rollback deployment of UI code to AWS CloudFront.
