## ARTS

##### Review

## Continuous delivery

**Continuous delivery** (**CD** or **CDE**) is a [software engineering](https://en.wikipedia.org/wiki/Software_engineering) approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time and, when releasing the software, doing so manually.[[1]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-CD_LC-1)[[2]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-shahin-et-al-2017-2) It aims at building, testing, and releasing software with greater speed and frequency. The approach helps reduce the cost, time, and risk of delivering changes by allowing for more incremental updates to applications in production. A straightforward and repeatable deployment process is important for continuous delivery.
CD contrasts with [continuous deployment](https://en.wikipedia.org/wiki/Continuous_deployment), a similar approach in which software is also produced in short cycles but through automated [deployments](https://en.wikipedia.org/wiki/Software_deployment) rather than manual ones.

## Relationship to DevOps

Continuous delivery and [DevOps](https://en.wikipedia.org/wiki/DevOps) are similar in their meanings and are often conflated, but they are two different concepts.[[3]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-3) DevOps has a broader scope,[[4]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-CD_HJ-4) and centers around the cultural change, specifically the collaboration of the various teams involved in software delivery (developers, operations, quality assurance, management, etc.), as well as automating the processes in software delivery.[[4]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-CD_HJ-4) Continuous delivery, on the other hand, is an approach to automate the delivery aspect, and focuses on bringing together different processes and executing them more quickly and more frequently.[[5]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-5) Thus, DevOps can be a product of continuous delivery, and CD flows directly into DevOps.

## Relationship to continuous deployment

*Main article: *[continuous deployment](https://en.wikipedia.org/wiki/Continuous_deployment)
Continuous delivery is often confused with [continuous deployment](https://en.wikipedia.org/wiki/Continuous_deployment) with some people using the two terms interchangeably. Most practitioners, however, distinguish between the two by labeling continuous delivery as the ability to deliver software that can be deployed at any time through manual releases while continuous deployment does the same but through automated deployments.[[6]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-Overcome-challenges-6) According to [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler), in order to do continuous deployment one must be doing continuous delivery.[[7]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-:0-7) Academic literature, however, explicitly differentiates the two approaches as one uses manual deployments while the other uses automated ones.[[2]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-shahin-et-al-2017-2)[[8]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-8)
## Principles

Continuous delivery treats the commonplace notion of a *deployment pipeline*[[9]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-9) as a [lean](https://en.wikipedia.org/wiki/Lean_software_development) [Poka-Yoke](https://en.wikipedia.org/wiki/Poka-Yoke):[[10]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-10) a set of validations through which a piece of software must pass on its way to [release](https://en.wikipedia.org/wiki/Software_release). Code is compiled if necessary and then packaged by a build server every time a change is committed to a [source control repository](https://en.wikipedia.org/wiki/Source_control), then tested by a number of different techniques (possibly including manual testing) before it can be marked as releasable.

Developers used to a long cycle time may need to change their mindset when working in a CD environment. It is important to understand that any code commit may be released to customers at any point. Patterns such as [feature toggles](https://en.wikipedia.org/wiki/Feature_toggle) can be very useful for committing code early which is not yet ready for use by end users. Using [NoSQL](https://en.wikipedia.org/wiki/NoSQL) can eliminate the step of data migrations and schema changes, often manual steps or exceptions to a continuous delivery workflow.[[11]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-11) Other useful techniques for developing code in isolation such as [code branching](https://en.wikipedia.org/wiki/Branching_(software)) are not obsolete in a CD world, but must be adapted to fit the principles of CD - for example, running multiple long-lived code branches can prove impractical, as a releasable artifact must be built early in the CD process from a single code branch if it is to pass through all phases of the pipeline.[[clarification needed](https://en.wikipedia.org/wiki/Wikipedia:Please_clarify)]

## Deployment pipeline

Continuous delivery is enabled through the deployment pipeline. The purpose of the deployment pipeline has three components: visibility, feedback, and continually deploy.[[12]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-12)

* **Visibility** – All aspects of the delivery system including building, deploying, testing, and releasing are visible to every member of the team to promote collaboration.

* **Feedback** – Team members learn of problems as soon as possible when they occur so that they are able to fix them as quickly as possible.

* **Continually deploy** – Through a fully automated process, you can deploy and release any version of the software to any environment.

## Tools/tool types

Continuous delivery takes automation from source control all the way through production. There are various tools that help accomplish all or part of this process.[[13]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-13) These tools are part of the deployment pipeline which includes continuous delivery. The types of tools that execute various parts of the process include: [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration), [application release automation](https://en.wikipedia.org/wiki/Application_release_automation), [build automation](https://en.wikipedia.org/wiki/Build_automation), [application lifecycle management](https://en.wikipedia.org/wiki/Application_lifecycle_management).[[14]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-14)

## Architecting for continuous delivery

To practice continuous delivery effectively, software applications have to meet a set of [architecturally significant requirements](https://en.wikipedia.org/wiki/Architecturally_significant_requirements) (ASRs) such as deployability, modifiability, and testability.[[15]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-ArchCD_LC-15) These ASRs require a high priority and cannot be traded off lightly anymore.

Microservices is often used for architecting for continuous delivery.[[16]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-Micro_Chen-16) The use of Microservices can increase a software system’s deployability and modifiability. The observed deployability improvements include: deployment independency, shorter deployment time, simpler deployment procedures, and zero downtime deployment. The observed modifiability improvements include: shorter cycle time for small incremental functional changes, easier technology selection changes, incremental quality attribute changes, and easier language and library upgrades.[[16]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-Micro_Chen-16)

## Implementation and usage

The CD book written by Jez Humble and David Farley popularized the term, however since its creation the definition has continued to advance and now has a more developed meaning. Companies today are implementing these continuous delivery principles and best practices. Difference in domains, e.g. medical vs. web, are still significant and affect the implementation and usage.[[17]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-:1-17) Well-known companies that have this approach include [Yahoo.com](https://en.wikipedia.org/wiki/Yahoo.com),[[18]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-18) [Amazon.com](https://en.wikipedia.org/wiki/Amazon.com),[[19]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-19) [Facebook](https://en.wikipedia.org/wiki/Facebook),[[20]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-20)[Google](https://en.wikipedia.org/wiki/Google),[[21]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-21) [Paddy Power](https://en.wikipedia.org/wiki/Paddy_Power)[[1]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-CD_LC-1) and [Wells Fargo](https://en.wikipedia.org/wiki/Wells_Fargo).[[22]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-22)

## Benefits and obstacles

Several benefits of continuous delivery have been reported.[[1]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-CD_LC-1)[[17]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-:1-17)

* Accelerated Time to Market: CD lets an organization deliver the business value inherent in new software releases to customers more quickly. This capability helps the company stay a step ahead of the competition.

* Building the Right Product: Frequent releases let the application development teams obtain user feedback more quickly. This lets them work on only the useful features. If they find that a feature isn’t useful, they spend no further effort on it. This helps them build the right product.

* Improved Productivity and Efficiency: Significant time savings for developers, testers, operations engineers, etc. through automation.

* Reliable Releases: The risks associated with a release have significantly decreased, and the release process has become more reliable. With CD, the deployment process and scripts are tested repeatedly before deployment to production. So, most errors in the deployment process and scripts have already been discovered. With more frequent releases, the number of code changes in each release decreases. This makes finding and fixing any problems that do occur easier, reducing the time in which they have an impact.

* Improved Product Quality: The number of open bugs and production incidents has decreased significantly.

* Improved Customer Satisfaction: A higher level of customer satisfaction is achieved.


Obstacles have also been investigated.[[17]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-:1-17)

* Customer preferences: Some customers do not want continuous updates to their systems. This is especially true at the critical stages in their operations.

* Domain restrictions: In some domains, such as telecom and medical, regulations require extensive testing before new versions are allowed to enter the operations phase.

* Lack of test automation: Lack of test automation leads to a lack of developer confidence and can prevent using continuous delivery.

* Differences in environments: Different environments used in development, testing and production can result in undetected issues slipping to the production environment.

* Tests needing a human oracle: Not all quality attributes can be verified with automation. These attributes require humans in the loop, slowing down the delivery pipeline.


Eight further adoption challenges were raised and elaborated by Chen.[[6]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-Overcome-challenges-6) These challenges are in the areas of organizational structure, processes, tools, infrastructure, legacy systems, architecting for CD, continuous testing of non-functional requirements, and test execution optimization.

## Strategies to overcome adoption challenges

Several strategies to overcome continuous delivery adoption challenges have been reported.[[6]](https://en.wikipedia.org/wiki/Continuous_delivery#cite_note-Overcome-challenges-6)

| **Strategy**   | **Description**   | 
|:----|:----|
| Selling CD as a painkiller   | Identify each stakeholder's pain points that CD can solve, and sell CD as a painkiller to that stakeholder. This strategy helps to achieve buy-in from the wide range of stakeholders that a CD implementation requires.   | 
| Dedicated team with multi-disciplinary members   | Without a dedicated team, it can be hard to progress because employees are often assigned to work on other value streams. A multi-disciplinary team not only provides the wide range of skills required for CD implementation but also smooths the communication with related teams.   | 
| Continuous delivery of continuous delivery   | Organize the implementation of CD in a way that delivers value to the company as early as possible, onboarding more projects gradually, in small increments and eventually rolling out CD across the whole organization. This strategy helps justify the investment required by making concrete benefits visible along the way. Visible benefits, in turn, help to achieve the sustained company support and investment required to survive the long and tough journey to CD.   | 
| Starting with easy but important applications   | When selecting the first few applications to migrate to CD, choose the ones that are easy to migrate but that are important to the business. Being easy to migrate helps to demonstrate the benefits of CD quickly, which can prevent the implementation initiative from being killed. Being important to the business helps to secure the required resources, demonstrates clear and unarguable value, and raises the visibility of CD in the organization.   | 
| Visual CD pipeline skeleton   | Give a team a visual CD pipeline skeleton that has the full CD pipeline view but with empty stages for those they cannot implement yet. This helps to build up a CD mindset and maintain the momentum for CD adoption. The pipeline skeleton is especially useful when the team's migration to CD requires a large effort and mindset changes over a long period of time.   | 
| Expert drop   | Assign a CD expert to join tough projects as a senior member of the development team. Having the expert on the team helps to build the motivation and momentum to move to CD from inside the team. It also helps to maintain momentum when the migration requires a large effort and a long period of time.   | 


[https://en.wikipedia.org/wiki/Continuous_delivery](https://en.wikipedia.org/wiki/Continuous_delivery) 
