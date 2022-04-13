## CI/CD
**What is continuous integration**
Ref: https://aws.amazon.com/devops/continuous-integration/

_Continuous integration_ refers to the build and unit testing stages of the software release process. Every revision that is committed triggers an automated build and test.

With [continuous delivery](https://aws.amazon.com/devops/continuous-delivery/), code changes are automatically built, tested, and prepared for a release to production. Continuous delivery expands upon continuous integration by deploying all code changes to a testing environment and/or a production environment after the build stage.

![](Pasted%20image%2020220329135439.png)

![](Pasted%20image%2020220329134858.png)

# Jenkins labels
When creating a new slave node, Jenkins allows us to tag a slave node with a label. Labels **represent a way of naming one or more slaves**. We leverage this labeling system to tie the execution of a job directly to one or more slave nodes.

# Build triggers
- build whenever a snapshot dependency is built (MANUAL BUILD)
- Trigger build remotely (from a script)
- Build periodically (Build the job per specified schedule)
- Poll SCM (Build the job per specified schedule only if the code changes)

**Build Periodically VS Poll SCM**
- Both follow the cron schedule to build the job.
- Build periodically will build the job at specified schedule irrespective of code changes
- Poll SCM will build the job at specified schedule only if code changes detected.
 