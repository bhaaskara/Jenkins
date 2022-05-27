# JENKINS_HOME
https://dzone.com/articles/jenkins-02-changing-home-directory

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

## CI/CD
**What is continuous integration**
Ref: https://aws.amazon.com/devops/continuous-integration/

_Continuous integration_ refers to the build and unit testing stages of the software release process. Every revision that is committed triggers an automated build and test.

With [continuous delivery](https://aws.amazon.com/devops/continuous-delivery/), code changes are automatically built, tested, and prepared for a release to production. Continuous delivery expands upon continuous integration by deploying all code changes to a testing environment and/or a production environment after the build stage.

![](Pasted%20image%2020220329135439.png)

![](Pasted%20image%2020220329134858.png)

# Scripted VS Declarative pipeline
**The Declarative** pipeline is a recent feature that offers richer syntactical features over **Scripted Pipeline syntax**. The declarative pipeline is also designed to make writing and reading Pipeline code easier than the scripted pipeline.

Declarative Pipeline encourages a **declarative programming model,** whereas Scripted Pipelines follow a more **imperative programming model**.

## Groovy scripted pipelines
When the Jenkins pipeline was first introduced, the scripted pipeline was the only available option. Software developers enthusiastically embraced the scripted pipeline for two big reasons:

-  It provided a domain specific language that simplified many tasks a Jenkins developer 
    would perform.
-  At the same time, it allowed developers to inject [Groovy](https://www.theserverside.com/definition/Groovy) code into their pipelines any  time.

Groovy is a [JVM-based programming language](https://www.theserverside.com/news/252447729/Developers-favor-JVM-languages-for-mobile-enterprise), which means a scripted Jenkins pipeline that uses Groovy has access to the vast array of APIs that are packaged with the JDK. The scripted pipeline developer has an immense amount of power.

### Scripted pipeline drawbacks

The development community found that pipeline builders with strong Java and Groovy skills, but little experience with Jenkins, would often write complicated Groovy code to add functionality that's already available through the Jenkins DSL.

A Jenkins pipeline should be a simple, easy-to-read and easy-to-manage component. Excessive code in a scripted pipeline violates one of the [fundamental CI/CD principles](https://www.techtarget.com/searchitoperations/tip/Adopt-these-continuous-delivery-principles-organization-wide).

As a result, Jenkins corrected this trend when it introduced the declarative pipeline.

## Declarative pipeline
In contrast to the scripted pipeline, the declarative Jenkins pipeline doesn't permit a developer to inject code. [A Groovy script](https://www.techtarget.com/searchapparchitecture/feature/Exploring-the-versatility-of-Groovy-programming) or a Java API reference in a declarative pipeline will cause a compilation failure.

## Pipeline syntax differences
Declarative pipelines always begin with the word pipeline. Scripted pipelines, on the other hand, always begin with the word node. Declarative pipelines break down stages into individual stages that can contain multiple steps. Scripted pipelines use Groovy code and references to the Jenkins pipeline DSL within the stage elements without the need for steps.

The development industry has largely moved toward a declarative programming model for CI/CD pipelines. Both [GitHub Actions](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-publish-GitHub-Actions-artifacts-example) and GitLab CI support only [YAML pipelines](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Jenkins-YAML-Pipeline-Example), which are very similar to declarative Jenkins pipelines.

Furthermore, declarative pipelines are easier to maintain and they tend to have a lower learning curve. The declarative syntax is the best approach to use when new [CI/CD workflows](https://www.jenkins.io/doc/book/pipeline/syntax/) are built.

![](Pasted%20image%2020220522195435.png)
