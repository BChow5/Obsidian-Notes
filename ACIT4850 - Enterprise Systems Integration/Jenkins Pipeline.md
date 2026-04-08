# PART 3 — CREATE PIPELINE IN JENKINS
Go to Jenkins Dashboard.

## Step 1 — Create New Item
1. Click **New Item**
2. Name it:
    `PointPipeline`
3. Select:
    `Pipeline`
4. Click OK

## Step 2 — Configure Source Code

Under:
### Pipeline → Definition
Select:
`Pipeline script from SCM`

### SCM:
Select:
`Git`

### Repository URL:
Paste your GitLab HTTPS clone URL.
If it errors:
- Click **Add Credentials**
- Add:
    - Username: GitLab username
    - Password: Access Token
    - ID: `jenkins_svc`
Select the credential.

### Branch Specifier
Use:
`*/main`
(or `*/master` depending on your repo)

---

### Script Path
Set:
ci/Jenkinsfile
Click Save.

# ✅ PART 4 — CREATE THE JENKINSFILE
Inside your GitLab repo:
Create folder:
`ci/`
Inside it create:
`Jenkinsfile`

---

## Example Exam-Ready Pipeline Template
You will adjust stages during exam.
```
pipeline {  
  
    agent any  
  
    stages {  
  
        stage('Build') {  
            steps {  
                sh 'python3 -m venv venv'  
                sh 'venv/bin/pip install -r requirements.txt'  
            }  
        }  
  
        stage('Lint') {  
            steps {  
                sh 'venv/bin/pip install pylint'  
                sh 'venv/bin/pylint *.py || true'  
            }  
        }  
  
        stage('Unit Test') {  
            steps {  
                sh 'venv/bin/pip install unittest-xml-reporting'  
                sh 'venv/bin/python test_point_manager.py'  
            }  
            post {  
                always {  
                    junit 'test-reports/*.xml'  
                }  
            }  
        }  
    }  
}
```
Commit and push to GitLab.

---

# ✅ PART 5 — RUN THE PIPELINE
Go to Jenkins → Your Job → Click:
Build Now
Watch:
- Console Output
- Test Results tab
- Build Status
    

---

# 🔄 HOW IT WORKS INTERNALLY (Step-by-Step Runtime Flow)

When you click **Build Now**, this happens:

---

### 1️⃣ Jenkins Pulls Code from GitLab

It clones your repo into the Jenkins workspace.

---

### 2️⃣ Jenkins Reads `ci/Jenkinsfile`

It parses the pipeline instructions.

---

### 3️⃣ Jenkins Executes Stages in Order

Example:

### Build Stage

- Creates virtual environment
    
- Installs dependencies
    

### Lint Stage

- Runs pylint
    

### Unit Test Stage

- Executes test file
    
- xmlrunner generates:
    
    test-reports/*.xml
    

---

### 4️⃣ Jenkins Parses Test Results

The line:

junit 'test-reports/*.xml'

tells Jenkins:

> "Look in this folder and parse test results."

Jenkins then:
- Shows number of passed tests
- Shows failed tests
- Marks build unstable if failures exist