#  What is Jenkins?

Jenkins is an open-source automation server that helps to automate parts of the software development process, such as building, testing, and deploying applications. It is one of the most widely used automation servers and is used by many organizations to streamline their software development and delivery processes.

With Jenkins, developers can create jobs that automate the building, testing, and deployment of their applications. These jobs can be triggered manually or automatically, based on predefined schedules or events. Jenkins also provides support for Continuous Integration (CI) and Continuous Delivery (CD) practices.

#
# What other tools are available?

There are other tools similar to Jenkins that are available:

- CircleCI: A cloud-based continuous integration and delivery platform that can be used to automate the software development process.

- GitLab CI/CD: An open-source continuous integration and continuous deployment platform that is integrated with GitLab, a web-based Git repository manager.

- TeamCity: A commercial continuous integration and build management server that supports multiple build agents and can be used to build, test, and deploy applications.

- Bamboo: A commercial continuous integration and deployment server that can be used to automate the building, testing, and deploying of applications.

- Buildkite: A cloud-based continuous integration and delivery platform that can be used to automate software development workflows.

Each of these tools has its strengths and weaknesses, and the choice of which tool to use depends on the specific requirements of the project and the team's preferences.

#
# Jenkins Stages

In Jenkins, stages are a way to organize the steps involved in a pipeline, which is a sequence of jobs that are executed in a particular order to build, test, and deploy an application. Stages help to break down the pipeline into smaller, more manageable units, which can be executed in parallel or sequentially, depending on the requirements of the project.

Some of the stages commonly used in Jenkins pipelines:

- Checkout: This stage involves checking out the code from the version control system, such as Git or SVN.

- Build: This stage involves compiling the source code and creating the executable files or artifacts.

- Test: This stage involves running unit tests, integration tests, and other types of tests to verify that the code works as expected.

- Static code analysis: This stage involves running static code analysis tools to detect coding errors, security vulnerabilities, and other issues.

- Code coverage: This stage involves measuring the code coverage of the tests, which indicates how much of the code is executed during the tests.

- Deploy: This stage involves deploying the application to a test environment, a staging environment, or a production environment.

- Release: This stage involves creating a release version of the application and publishing it to a repository or a distribution platform.

Each of these stages can be customized and configured to meet the specific requirements of the project.

#
# The differences between CI, CD and CDE

CI is a practice where developers frequently integrate code changes into a shared repository, and automated tests are run to detect and fix issues early in the development process. The goal of CI is to ensure that code changes are continuously integrated and tested, allowing developers to identify and fix issues quickly and prevent errors from propagating into the final product.

CD is a practice that automates the process of deploying code changes to production or staging environments. The goal of CD is to ensure that code changes are deployed quickly, reliably, and consistently, and that the software can be released to customers with minimal manual intervention.

CDE is 

#
# Connecting Jenkins to a Github Repo containing the APP

1. In the Git Bash terminal, we need to create a new SSH key pair for Github and Jenkins:
- `cd` into your .ssh folder and enter `ssh-keygen -t rsa -b 4096 -C jgillatt@spartaglobal.com` to generate a new key
- Name the key and press enter until the key pair is generated
2. Head over to your github repo that contains the APP and select the settings tab
- Under 'Deploy keys' Select 'Add deploy key' 
3. In the gitbash terminal (in the .ssh folder), use `cat <key file name>.pub` to display the public key
- Copy the full key from start to finish
4. Back on Github, paste the key into the 'Key' text box. Then name the deploy key and select 'Add key'
