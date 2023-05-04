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

CDE (manual) typically stands for Continuous Delivery Environment. A Continuous Delivery Environment refers to the infrastructure and resources required to automate the delivery of software applications from development to production. It includes the tools, processes, and infrastructure needed to deploy, configure, and manage the application in various environments, such as staging and production. 

#
![CICD1](https://user-images.githubusercontent.com/129315605/236151223-a3e9e954-3bfe-4b06-b09c-36ff38157c10.png)
#
# Connecting Jenkins to a Github Repo containing the APP and running a test for the APP

1. In the Git Bash terminal, we need to create a new SSH key pair for Github and Jenkins:
- `cd` into your .ssh folder and enter `ssh-keygen -t rsa -b 4096 -C jgillatt@spartaglobal.com` to generate a new key
- Name the key and press enter until the key pair is generated
2. Head over to your github repo that contains the APP and select the settings tab
- Under 'Deploy keys' Select 'Add deploy key' 
3. In the gitbash terminal (in the .ssh folder), use `cat <key file name>.pub` to display the public key
- Copy the full key from start to finish
4. Back on Github, paste the key into the 'Key' text box. Then name the deploy key and select 'Add key'
5. In your web browser enter `http://35.178.11.196:8080/` to connect to Jenkins and login with the user and password
6. On the dashboard select 'new item' 
- Enter a name for the item
- Select freestyle project and click ok
- Enter a description
- Select 'Discard old builds' and set it to 3. This prevents old builds from stacking up.
- Under 'Source Code Management' select 'Git'
- Go to your Github repo and select the Code dropdown button. Copy the HTTPS URL and paste it into the 'Repository URL' on Jenkins
- Under 'Credentials' select 'Add/Jenkins'. Then select the 'Kind' dropdown and choose 'SSH Username with private key' 
- Enter a username (should be the same as the key you generated)
- Under 'Private key' select 'Enter directly'
- In the bash terminal use command `cat <key file name>` inside the .ssh folder
- Copy the entire contents of the key and paste it into the 'key' text box on Jenkins
- Select 'Add' and then select the key name for the 'Credentials' 
- Rename the branch to */main
- Under 'Build environment' tick the 'Provide Node & npm bin/ folder to PATH' box
- Under 'Build' select 'Add build step / Execute shell`. Enter the following commands into the box:
```
cd app
npm install
npm test
```
- Save the Item and select 'Build now' from the left.
7. Under 'Build history', you will see "#1", select the dropdown button and select 'Console output'
8. At the bottom of the page you should see:
```
Your app is ready and listening on port 3000
  Homepage
    ✓ should display the homepage at / GET
    ✓ should contain the word Sparta at / GET

  Fibonacci
    ✓ should display the correct fibonacci value at /fibonacci/10 GET


  3 passing (42ms)

Finished: SUCCESS
```
#
# Creating a Webhook

1. On your github repo, go to Settings and select Webhooks
- Select Add webhook 
- Enter `http://35.178.11.196:8080/github-webhook/` into the payload URL
- Select Application/Json
- Add the Webhook
2. To use on Jenkins, select the 'GitHub hook trigger for GITScm polling' option under 'Source code management' within the job config

#
# Merging the Github Dev branch with the Main branch with Jenkins

1. Create new job called jake-merge, we can select our old job template
2. create dev branch on local host and make change to readme
3. we do this by `git branch dev` then `git checkout dev`
4. push to github which should trigger job
5. if test passed, merge code to main branch
6. we now switch to our target branch which is `main` by `git checkout main`
7. We then `git merge dev` which should merge our dev branch into the main branch
8. `git add .` then `git commit -m "xxxx"` then `git push origin main` and it should push the new merged branch, and we can test this by checking Jenkins, and it should show a new build being deployed.
9. Like before, it should show the updated code/readme on your GitHub

# Automating CI pipeline

- Step 1. Create a new branch on GitHub

1. Open your Project in VS code
2. In the Git terminal type `git branch dev` to create a dev branch
3. Use `git checkout dev` to switch to `dev` branch by default
4. Now, use `git push -u origin dev` to push your changes to dev branch

- Step 2. Create a job to test a code from dev branch

1. Go to your Jenkins
2. Create a new task. As it will be identical to the previous task we've created we could use that as a template to copy settings from it
3. In `Source Code Management` change `Branches to build` to `dev`:

4. The rest you can leave as it is. Click `Save`
5. Use `Build Now` in order to test the job manually

- Step 3. Merge dev branch with main branch

1. Create a new task on Jenkins
2. In `General`:

- Write a description
- Set `Discard old builds` to 3
- `GitHub Project` copy https link to your project and paste it there

3. In `Source Code management` copy the settings from the previous task. Then click on `Add` in Additional Behaviours and select `Merge before build`:

- `Name of repository` - set to `origin`
- `Branch to merge to` - set to `main`
- Rest leave to default

4. In `Post-build Action` click on `Add` and select `Git Publisher`:

- Enable `Push only if build successful`
- Enable `Merge Results`

5. Click on save
6. Use `Build Now` in order to test the job manually

- Step 4. Trigger merge job if test is successful

1. Go back to your `CI` job configuration
2. Scroll down to `Post-build actions` and add `Build other projects`
3. In `projects to build` type the name of the project you want to run, for example 'jake-devmain-merge'
4. Save your project
5. Now, to test if it works, push some changes form your local repo and it should trigger the test first, and if the test is successful it will then merge the changes from dev branch to main branch and push the changes to your GitHub

#
# Automating CICD to our application on an EC2 Instance

1. Make a new job, we call it `jake-ci-merge-dev` and we as before create a template from `jake-ci-merge` and scroll down to source code management, additional behaviours and add: `name of repo: origin`, `branch to merge: main`

2. On post build actions select git publisher, then tick 'Push only if build succeeds' and 'Merge results'

3. Go to our `jake-ci-merge` job, scroll all the way down to post build actions and select post-build actions, and select the job we just created.

4. Save and we test as before, on GitBash, change code/readme, `git add .` -> `git commit -m "xxx"` -> `git push origin dev`.
5. once done, it will deploy it on `jake-ci-merge` which will update it on the `dev` branch and as we did post build actions, this triggers the `jake-ci-merge-dev` job to deploy, this will merge the `dev` branch with the new changes with the `main branch`
6. Test this by checking the repo on GitHub and navigating between both branches, and see if the changes made on the `dev` branch are the same as the `main` branch



## create 3rd job to push code to production

1. Launch instance from my app AMI
2. Configure the security groups as follows:
```
SSH - my ip
SSH - Jenkins ip
TCP 3000 - any ip
TCP 8080 - any ip
TCP 80 - any ip
```
3. Launch Instance
4. Create a new job and use the template from the previous job
5. Add an SSH Agent under 'Build environment' and select your credentials
6. In the execute shell enter these commands changing the `ip` to your Public ipv4 addres found on 
EC2 instance.

```
scp -v -r -o StrictHostKeyChecking=no app/ ubuntu@<my-ip>:/home/ubuntu/
ssh -A -o StrictHostKeyChecking=no ubuntu@<my-ip> <<EOF
# sudo apt install clear#

cd app

# sudo npm install pm2 -g
# pm2 kill // if you need to launch again when app is already running
npm install
nohup node app.js > /dev/null 2>&1 &
```
6. Save changes and build to test the deployment of the app
7. Type public ip with `:3000` at the end to see if the app has deployed

#
#
# How to create a Jenkins server on an EC2

1. Create an EC2 Instance in AWS for Jenkins, using the following:
- Ubuntu 18.04
- t2.medium
- Create a new security group and add the following ports:

8080
3000
22
80
443

- Install Java and Jenkins with the following commands:
```
sudo apt-get update
sudo apt-get install default-jdk -y

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

sudo ufw allow OpenSSH
sudo ufw enable

sudo su
ssh -T git@github.com
```

- Create another EC2 instance for our APP with the same ubuntu and the same security group, but this time using t2.micro

- Enter the Jenkins EC2 ip in your browser
- Login to Jenkins with the password from `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
- Create your account
- On Jenkins go to 'Manage Jenkins', 'Manage Plugins', and install the following plugins:

SSH Agent
Nodejs
Office 365
EC2

2. Create a new Jenkins job
- Tick GitHub project and provide the github repo url  (that contains the app)
- Set up the ssh connection between GitHub and Jenkins using your key-pair:
- Paste the public key into your github repo 'Deploy keys'
- Paste the private key into your Jenkins Job (Under source code management / git)
- Set the branch to `*/main`
- Tick 'GitHub hook trigger for GITScm polling'
- Tick 'Provide Node & npm bin/ folder to PATH'
- Select your Nodejs
- Tick SSH Agent and add credentials. Get your 'tech221.pem' key from gitbash with `cat tech221.pem` and paste it in
- Add a build step and enter the following code into a shell script:

```
scp -v -r -o StrictHostKeyChecking=no app/ ubuntu@34.243.194.140:/home/ubuntu/
ssh -A -o StrictHostKeyChecking=no ubuntu@34.243.194.140 <<EOF
# sudo apt install clear#

cd app

# sudo npm install pm2 -g
# pm2 kill # // if you need to launch again when app is already running
npm install
nohup node app.js > /dev/null 2>&1 &
```
  
- Your Job config should look like the following:
![ss1](https://user-images.githubusercontent.com/129315605/236267625-124cc72e-ebc5-454f-8e94-b54f258130d4.png)
![ss2](https://user-images.githubusercontent.com/129315605/236267635-4567c720-6c80-420d-9bb5-1119edf41532.png)
![ss3](https://user-images.githubusercontent.com/129315605/236267648-a2b49fb5-a9a9-428c-9fb5-a42bb1e6508e.png)
![ss4](https://user-images.githubusercontent.com/129315605/236267660-c6f493cf-f875-42a8-b9c8-a432259e1ab4.png)
![ss5](https://user-images.githubusercontent.com/129315605/236267682-e3353fdd-bc1f-4650-b1fd-1a2e0cc2767f.png)
  
 - Save the Job and click 'Build Now'
 - Enter your EC2 ip into your browser with port 3000 to test the app is running.
