
## Python Pipeline (Lab 5 + Lab 6)
### **requirements.txt**
create a requirements.txt file to install the packages
```
Flask==3.1.2  
SQLAlchemy==2.0.43  
unittest-xml-reporting==3.2.0
```

#### **Explanation (Separate Section)**
- Defines Python dependencies for the project.
- Ensures consistent versions across:
    - Local machine
    - Jenkins controller
    - Jenkins agent
- Installed using: `pip install -r requirements.txt`
- `unittest-xml-reporting` allows Jenkins to read Python test results in JUnit XML format.
  
### **Dockerfile (controller agent and agent1)**
#### **Controller agent**
Dockerfile
```bash
FROM jenkins/jenkins:lts-jdk21  
LABEL maintainer="Your Email"  
USER root  
RUN mkdir /var/log/jenkins  
RUN mkdir /var/cache/jenkins  
RUN chown -R jenkins:jenkins /var/log/jenkins  
RUN chown -R jenkins:jenkins /var/cache/jenkins  
RUN apt-get update  
RUN apt-get install -y python3 python3-pip  
USER jenkins  
ENV JAVA_OPTS="-Xmx4096m"  
ENV JENKINS_OPTS="--logfile=/var/log/jenkins/jenkins.log --  
webroot=/var/cache/jenkins/war --prefix=/jenkins"
# add the --prefix=/jenkins to allow setting reverse proxy
```

build the image according to the dockerfile
```bash
docker build -t myjenkins ~/docker
```

run the image
```bash
docker run -p 8080:8080 -p 50000:50000 --restart always --name=jenkins-controller --mount source=jenkins-log,target=/var/log/jenkins --mount source=jenkins-data,target=/var/jenkins_home -d myjenkins
```

#### **Explanation (Separate Section)**

##### Dockerfile
- Uses official Jenkins LTS image with JDK 21.
- Switches to root to:
    - Create log/cache directories
    - Install Python and pip
- Changes ownership back to `jenkins` user for security.
- Allocates 4GB memory to JVM.
- Configures:
    - Custom log file location
    - Custom webroot
    - Reverse proxy prefix (`--prefix=/jenkins`)

##### Build Command
- Creates a custom Jenkins image named `myjenkins`.

##### Run Command
- Exposes:
    - 8080 → Jenkins Web UI
    - 50000 → Agent communication
- Uses persistent Docker volumes.
- Restarts automatically.
- Runs container in background.
#### **agent1**
dockerfile
```bash
FROM jenkins/ssh-agent:debian-jdk21 AS ssh-agent
RUN apt-get update
RUN apt-get install -y python3 python3-pip
RUN apt-get update && apt-get install -y python3-venv
RUN apt-get install -y pylint
RUN apt-get install -y zip
# Set SSH keep-alive intervals to prevent connection loss

# Sends client keep alive ever 30 seconds and disconnects after 3 failures in a row
RUN echo "ClientAliveInterval 30" >> /etc/ssh/sshd_config
RUN echo "ClientAliveCountMax 3" >> /etc/ssh/sshd_config
```

run docker file
```bash
# create the image
docker build -t jenkins_agent . 

# run the image in a container
docker run -d \
  --name agent1 \
  --restart always \
  -p 4444:22 \
  -e JENKINS_AGENT_SSH_PUBKEY="ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOEKaxTBeM5r07nVnPOPGokoZN6PkGuYii1Cdb+sRJjO azureuser@Jenkins" \
  jenkins_agent

```
You will have to open up this alternate port on your Jenkins VM. To add some security, you can open it up only to itself (i.e., to its own IP)

#### **Explanation (Separate Section)**
##### Dockerfile
- Uses Jenkins SSH agent image.
- Installs:
    - Python
    - pip
    - venv support
    - pylint
    - zip
- Configures SSH keep-alive to prevent dropped builds.

##### Run Command
- Exposes SSH on port 4444.
- Injects Jenkins public key.
- Auto-restarts on crash.
- Container name matches Jenkins node name.

### **Create a Node**
For the Node configuration in Jenkins:  
- Use `agent1` as the name (to match the container name)  
- Use `/home/jenkins/agent` as the remote root directory  
- Set the agent label to `python_agent`  
- For the Launch Agent use “Launch agents via SSH”  
- Use the DNS of your Jenkins VM as the hostname  
- Use your Jenkins credential you created as the credential  
- Use Manually trusted key verification strategy  
- Under Advanced, set the port of your SSH connection (set on your docker run command). It is also a good idea to increase the timeout of the SSH connection to 600 seconds or more.
![[How to Setup Pipeline-3.png]]

![[How to Setup Pipeline-4.png]]
  
### **Jenkinsfile**
- Add a folder to your project in `GitLab` called ‘ci’.
- Create a `Jenkinsfile` in the ci folder as follows (note the agent name):
```groovy
pipeline {
    agent { 
        label 'python_agent' 
    }

    stages {
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt --break-system-packages'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'python3 -m unittest test_point_manager'
            }
        }
    }
}

```

#### **Explanation (Separate Section)**
- Runs pipeline on node labeled `python_agent`.
- Build stage installs dependencies.
- Unit Test stage runs Python unit tests.
- Simple two-stage CI workflow.
### **Create a Jenkins Build Pipeline**
Under the Pipeline section of the job configuration: 
- Selected Definition as `Pipeline script from SCM`
- Select your SCM as Git and configure it for your Point repository in Git using valid credentials  
- Set the Script Path as `ci/Jenkinsfile`

### Add Test Results  
- Jenkins only supports Java junit based test results. 
- Modify the Python unit tests in your GitLab project as follows to produce junit style test results:  
- In test_point_manager.py, add `import xmlrunner`
- Add the following to the end of `test_point_manager.py`:  
```python
if __name__ == "__main__":  
	unittest.main( testRunner=xmlrunner.XMLTestRunner(output="test-reports"), failfast=False, buffer=False, catchbreak=False)  
```
- This will run the tests once to generate the test-reports and then again for the normal Python unit test output.  
- Update the Unit Test stage in your Jenkinsfile with the updated shell command and a post action as follows:  
```groovy
stage('Unit Test') {  
	steps {  
		sh 'python3 test_point_manager.py'  
	}  
	post {  
		always {  
			junit 'test-reports/*.xml'  
		}  
	}  
}
```

### Add Integration Test
```python
import unittest
import points_api
import xmlrunner
import os
import inspect

from point_manager import PointManager
from sqlalchemy import create_engine
from base import Base


class TestPointApi(unittest.TestCase):

    def setUp(self):
        engine = create_engine('sqlite:///test_points.sqlite')
        # Creates all the tables
        Base.metadata.create_all(engine)
        Base.metadata.bind = engine
        points_api.point_mgr = PointManager('test_points.sqlite')
        points_api.app.testing = True
        self.app = points_api.app.test_client()
        self.logPoint()

    def tearDown(self):
        os.remove('test_points.sqlite')
        self.logPoint()

    def logPoint(self):
        currentTest = self.id().split('.')[-1]
        callingFunction = inspect.stack()[1][3]
        print('in %s - %s()' % (currentTest, callingFunction))

    def test_points_all(self):
        rv = self.app.get('/points/all')
        self.assertEqual(rv.status, '200 OK')


if __name__ == "__main__":
    unittest.main(
        testRunner=xmlrunner.XMLTestRunner(output="api-test-reports"),
        failfast=False,
        buffer=False,
        catchbreak=False
    )
```

- Add a new stage to your Jenkinsfile. It should be identical to the Unit Test stage but it is called  `Integration Test` and runs the integration tests in `test_points_api.py`. 
- Make sure the test results for these new tests show up after you run the job. 
- It should get the results from `api-test-reports` (not test-reports) 
	- so make sure to change `test-reports/*.xml` to `api-test-reports/*.xml`

#### **Explanation (Separate Section)**
- Creates temporary SQLite database.
- Binds SQLAlchemy engine.
- Sets Flask app to testing mode.
- Uses test client to simulate API calls.
- Deletes database after test.
- Generates XML results in `api-test-reports`.

### **Final Jenkinsfile**
Updated `Jenkinfiles`
```groovy
pipeline {

    // Defines where the pipeline will run (a Jenkins agent/node with this label)
    agent { label 'python_agent' }

    stages {

        stage('Prepare Workspace') {
            steps {

                // Prints build ID and workspace path to the Jenkins console
                echo "Running ${env.BUILD_ID} with workspace ${env.WORKSPACE}"

                // Run custom Groovy logic
                script {

                    // List of directories we want to clean before the build
                    def foldersToDelete = ['test-reports', 'api-test-reports', 'venv']

                    // Loop through each folder and delete it if it exists
                    for (folder in foldersToDelete) {
                        if (fileExists(folder)) {
                            dir(folder) {
                                deleteDir()   // Deletes the directory and its contents
                            }
                            echo "Deleted directory: ${folder}"
                        } else {
                            echo "Directory not found, skipping: ${folder}"
                        }
                    }
                }

                // Show directory contents (won’t fail if folder doesn’t exist)
                sh 'ls -la test-reports || true'
                sh 'ls -la api-test-reports || true'
                sh 'ls -la venv || true'

                // Create a Python virtual environment (ignore error if already exists)
                sh 'python3 -m venv venv || true'
            }
        }

        stage('Build') {
            steps {
                // Install coverage tool inside the virtual environment
                sh 'venv/bin/pip install coverage'

                // Install project dependencies
                sh 'venv/bin/pip install -r requirements.txt'
            }
        }

        stage('Lint') {
            steps {
                // Run pylint on all Python files
                // --fail-under 5 means the build fails if score < 5
                sh 'pylint --fail-under 5 "*.py"'
            }
        }

        stage('Test and Coverage') {
            steps {
                script {

                    // Find all test files matching test*.py
                    def files = findFiles(glob: 'test*.py')

                    // Fail the build if no tests are found
                    if (files.size() == 0) {
                        error "No test files found!"
                    }

                    // Run each test file with coverage
                    for (file in files) {
                        echo "Running test file: ${file.path}"
                        sh "venv/bin/coverage run --omit 'venv/*' ${file}"
                    }

                    // Print coverage summary to console
                    sh 'venv/bin/coverage report'
                }
            }

            post {
                always {
                    script {

                        // Publish unit test reports if they exist
                        def test_reports_exist = fileExists 'test-reports'
                        if (test_reports_exist) {
                            junit 'test-reports/*.xml'
                        }

                        // Publish API test reports if they exist
                        def api_test_reports_exist = fileExists 'api-test-reports'
                        if (api_test_reports_exist) {
                            junit 'api-test-reports/*.xml'
                        }
                    }
                }
            }
        }

        stage('Zip Artifact') {
            steps {
                // Create a zip file containing all Python files
                sh 'zip app.zip *.py'
            }

            post {
                always {
                    script {
                        // Archive the zip so it’s downloadable from Jenkins
                        if (fileExists('app.zip')) {
                            echo "Archiving app.zip artifact"
                            archiveArtifacts artifacts: 'app.zip', fingerprint: true
                        }
                    }
                }
            }
        }
    }
}

```

#### **Explanation (Separate Section)**
##### Prepare Workspace
- Cleans old reports and virtual environment.
- Prevents stale data issues.
- Creates fresh venv.

##### Build
- Installs coverage tool.
- Installs dependencies.

##### Lint
- Runs pylint.
- Fails build if score < 5.

##### Test and Coverage
- Automatically finds all test files.
- Runs coverage for each.
- Publishes test reports.
- Fails if no tests exist.

##### Zip Artifact
- Zips all Python files.
- Archives artifact in Jenkins.

### **Create Jenkins Job**
- Create a new build job:  
	- Select New Item  
	- Name it “Point”  
	- Select Freestyle Project
	- Press OK  

- Configure the build job:  
	- In Source Code Management, select Git  
	- Repository URL is the https URL of you Point project from GitLab (i.e., clone URL)  
		- You should see an error that it cannot connect to the repo  
	- Select Add -> Credentials and enter your Access Token name and Password. Give it an ID of jenkins_svc.  
		- The error should go away if the Access Token is correct  
	- Press Save  
	- Note: The name of the default branch in GitLab *may* have changed from master to main. You will have to change the branch name in the Git configuration in this Jenkins  build job to match yours.
	  
- Run the build job:  
	- Select Build Now  
	- It should be successful after a few seconds  
	- View the Workspace. All the files from the GitLab project should be in the Workspace.

### GitLab Triggers
#### On Jenkins
- Install Plugin:
  Make sure you have the GitLab Plugin installed (Manage Jenkins -> Manage Plugins). It should show up in the Installed plugins tab.

- Go to the Configure page of your `PointPipeline` job.  
- Under `Build Triggers` section select the Build when a change is pushed to GitLab option.  
- Make note of the URL value (you’ll need this shortly).  
- Select the `Advanced` button for this option.  
- Generate a `Secret Token`. Make note of this value (you’ll also need it soon)

#### On GitLab
- Go to the Point project  
- On the left, select Settings -> Webhooks  
- Use the URL and Secret Token from Jenkins to populate the URL and Secret Token fields  
- Make sure Push events is checked  
- Since we are now using a signed certificate, make sure the Enable SSL Verification option is checked
- Select Add webhook and then test it out. A Jenkins job should be triggered (note: this may take a few seconds to show up)


## Build Shared Library

put it inside an independent project called `ci_functions` and put inside `vars` folder
path: `ci_functions/vars/python_build.groovy`
```groovy
def call() {
    pipeline {
    agent { label 'python_agent' }
    stages {
        stage('Setup') {
            steps {
                echo "Running ${env.BUILD_ID} with workspace ${env.WORKSPACE}"

                script {
                    def foldersToDelete = ['test-reports', 'api-test-reports', 'venv']

                    for (folder in foldersToDelete) {
                        if (fileExists(folder)) {
                            dir(folder) {
                                deleteDir()
                            }
                            echo "Deleted directory: ${folder}"
                        } else {
                            echo "Directory not found, skipping: ${folder}"
                        }
                    }
                }

                sh 'ls -la test-reports || true'
                sh 'ls -la api-test-reports || true'
                sh 'ls -la venv || true'
                sh 'python3 -m venv venv || true'
            }
        }
        stage('Build') {
            steps {
                sh 'venv/bin/pip install coverage'
                sh 'venv/bin/pip install -r requirements.txt'
            }
        }
        stage('Lint') {
            steps {
                sh 'pylint --fail-under 5 "*.py"'
    
            }
        }
        stage('Test and Coverage') {
            steps {
                script { 
                    def files = findFiles(glob: 'test*.py') 
                    if (files.size() == 0) { 
                        error "No test files found!" } 
                    for (file in files) { 
                        echo "Running test file: ${file.path}" 
                        sh "venv/bin/coverage run --omit 'venv/*' ${file}"
                        }
                    sh 'venv/bin/coverage report'
                }
            }
            post {
                
                always {
                    script {
                        def test_reports_exist = fileExists 'test-reports'
                        if (test_reports_exist) {
                            junit 'test-reports/*.xml'
                        }
                        def api_test_reports_exist = fileExists 'api-test-reports'
                        if (api_test_reports_exist) {
                            junit 'api-test-reports/*.xml'
                        }
                    }
                }
            }
        }
        stage('Zip Artifact') {
            steps {
                sh 'zip app.zip *.py'
            }
            post {
                always {
                    script {
                        if (fileExists('app.zip')) {
                            echo "Archiving app.zip artifact"
                            archiveArtifacts artifacts: 'app.zip', fingerprint: true
                        }
                    }
                }
            }
        }
    }
}
}
```

to call the shared library in each project in `/ci/Jenkinsfile`
```groovy
@Library('ci_functions@main') _
python_build()
```

### Setup the Shared Library
Update your Jenkins installation (i.e., Manage Jenkins -> Configure Jenkins -> Global Trusted Pipeline Libraries) 
so that `ci_functions` is a Global Trusted Pipeline Library. (on the main branch).

![[How to Setup Pipeline.png]]

![[How to Setup Pipeline-1.png]]