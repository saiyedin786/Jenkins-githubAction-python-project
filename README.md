

2. GitHub Actions CI/CD Pipeline Flask App
Objective:
Implement a CI/CD workflow using GitHub Actions for a Python application.
Requirements:
1. Setup:
   - Use a provided Python application repository on GitHub (provide a link to a sample Python application repository).
   - Ensure the repository has a main branch and a staging branch.
2. GitHub Actions Workflow:
   - Create a .github/workflows directory in your repository.
   - Inside the directory, create a YAML file to define the workflow.
3. Workflow Steps:
     - Define a workflow that performs the following jobs:
     - Install Dependencies: Install all necessary dependencies for the Python application using pip.
     - Run Tests: Execute the test suite using a framework like pytest.
     - Build: If tests pass, prepare the application for deployment.
     - Deploy to Staging: Deploy the application to a staging environment when changes are pushed to the staging branch.
     - Deploy to Production: Deploy the application to production when a release is tagged.
4. Environment Secrets:
   - Use GitHub Secrets to store sensitive information required for deployments (e.g., deployment keys, API tokens).
5. Documentation:
   - Update the README.md file with instructions on how the GitHub Actions workflow works and how to configure the necessary secrets.
6. Submission:
   - Provide the URL to the GitHub repository with the workflow file and updated README.md.
   - Include screenshots of the GitHub Actions workflow runs showing successful execution of all steps.
Deliverables:
- GitHub repository with the workflow file.
- Documentation in README.md.
- Screenshots of the GitHub Actions workflow runs.

Project Solution:
Step :1 – Cloning flask app from github repository
Repo link: https://github.com/mohanDevOps-arch/flask_practice

Step: 2- creating an virtual environment  and activating it
python -m venv venv
source /venv/scripts/activate
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/42179698-10d3-46fe-8728-4cab9791eee9" />

Step :3 - Installing dependencies from requirements.txt in local computer
Pip install –r requirements.txt
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/2caf1d70-b1d6-4e00-ace8-12fd577c8df6" />

Step-4 : creating an .env file
MONGO_URI=mongodb+srv://admin:admin@cluster0.stznivp.mongodb.net/sms
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/023f470f-8558-4d9f-83b8-8680fa57466e" />

Step:5  running the flask app
Python app.py
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/819f6301-667f-4b06-a7bb-46789d7a5626" />
Accessing application in browser
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e1b1ad20-62af-4303-bb18-253ce860dbcd" />

Creating github repository
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/02d93844-3880-4836-b475-21b3514cb369" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/c8d2126f-6c46-48bc-b931-530c94a53c1a" />

echo "# Jenkins-githubAction-python-project" >> README.md
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/saiyedin786/Jenkins-githubAction-python-project.git
git push -u origin main

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/a6a36428-d46d-4fc4-bd2a-1b2b6feb922d" />

Creating branch staging:
Git branch staging
Git checkout staging
Git merge main
Git add .
Git commit –m ‘s1’
Git push –u origin stagin
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/ae41f20f-a087-438d-ad42-6368a5a472d3" />

Creating github workflow named deploy.yml
Deploy.yml
name: Flask CI/CD Pipeline

on:
  push:
    branches:
      - main
      - staging
  release:
    types: [created]

jobs:
  build-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest

  deploy-staging:
    needs: build-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/staging'

    steps:
      - name: Deploy to Staging
        run: |
          echo "Deploying to STAGING environment"
          echo "Using STAGING_SECRET=${{ secrets.STAGING_SECRET }}"

  deploy-production:
    needs: build-test
    runs-on: ubuntu-latest
    if: github.event_name == 'release'

    steps:
      - name: Deploy to Production
        run: |
          echo "Deploying to PRODUCTION environment"
          echo "Using PROD_SECRET=${{ secrets.PROD_SECRET }}"

ec2 instance setup:
name : stagingServer
ami : ubuntu
type: t2micro

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d8abe353-afc1-417c-83e8-03a9c29243a1" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/19b328cc-1e68-4250-92bc-0416e43eed62" />
Connecting to ec2 instance using instaconnect:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e47093f9-f809-4e1a-8cf3-d59f063472bd" />
Install dependencies:
sudo apt update
sudo apt install -y python3 python3-pip git
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/0ed8397a-08e1-4700-adb1-5b746b01c2e5" />

Create App Directory
mkdir -p /home/ubuntu/flask-app
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/b86187c2-cb3a-4240-8ede-80868a896027" />
Generate SSH Key (Local Machine)
ssh-keygen -t rsa -b 4096 -C "github-actions"
this will generate public and private key at location ~/.ssh
~/.ssh/id_rsa        (private key)
~/.ssh/id_rsa.pub    (public key)

Copy content of 
cat ~/.ssh/id_rsa.pub 
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/47f556a2-78da-4d90-ba13-563705d373ac" />

paste it into ec2 instance at location
nano ~/.ssh/authorized_keys
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/c4a8a83f-2388-49ef-ad2b-fe53b35f84e2" />

Configure GitHub Secrets
Repo → Settings → Secrets and variables → Actions
EC2_HOST	:      3.91.215.197  
EC2_USER:               	ubuntu
EC2_SSH_KEY:    	contents of id_rsa
EC2_APP_DIR	:  /home/ubuntu/gitbhu-repo
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/1fd075db-f572-4335-9412-bba32465e019" />

GIthub workflow -> deply.yml

name: Flask CI/CD with EC2

on:
  push:
    branches:
      - staging
  release:
    types: [created]

jobs:
  build-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Run tests
        env:
          MONGO_URI: ${{ secrets.MONGO_URI }}
        run: pytest

  deploy-staging:
    needs: build-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/staging'

    steps:
      - name: Deploy to EC2 (Staging)
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt update && sudo apt upgrade -y
            git clone https://github.com/saiyedin786/Jenkins-githubAction-python-project.git
            cd Jenkins-githubAction-python-project.git
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install gunicorn
            gunicorn --bind 0.0.0.0:8000 app:app
            sudo nginx -t
            sudo systemctl restart nginx

  deploy-production:
    needs: build-test
    runs-on: ubuntu-latest
    if: github.event_name == 'release'

    steps:
      - name: Deploy to EC2 (Production)
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt update && sudo apt upgrade -y
            git clone https://github.com/saiyedin786/Jenkins-githubAction-python-project.git
            cd Jenkins-githubAction-python-project.git
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install gunicorn
            gunicorn --bind 0.0.0.0:8000 app:app
            sudo nginx -t
            sudo systemctl restart nginx
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/cb8919f1-f583-4cc1-adc6-2a7cee4cd692" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/47a6860a-366e-44f3-a8fb-a0c7a7e376e8" />

Make some changes in the add student.html and save it
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e6309167-551f-4d14-819e-2810d6616271" />

Git add .
Git commit  -m ‘s25’
Git push origin staging
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/a2c85b61-74d5-4335-bdd1-3c27cec970f2" />


Now deploy.yml triggered
Script in deploy ran as per written steps in script
Now my app deployed in ec2 instance
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/5ff96ed6-01aa-4baa-8bf3-45f0668be601" />

Testing in browser:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/43ff0a47-bb90-45b2-a060-24f0b3949dc3" />
Merging it with main branch
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/34fa70d8-ab0c-4abb-b62c-820ba30c2f93" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/36918d2e-41c4-4797-9e99-6995c7761966" />
Creating a release event:

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/601f1cb2-f87c-41d3-86e1-cc871d32a3eb" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/a946c1a9-9374-4e34-9bce-ffa5332d26cb" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/2e8bf212-c90c-49e7-b451-6e8447a0ff9a" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/f9d1f8a0-34e1-4dbe-9d56-c4d8320c2f13" />

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/826d0c02-d7df-4c92-8e65-6c5565841c51" />
Deploy.yml triggered:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/16e9e817-5763-4a49-9b86-5802c2e68960" />

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e10f818f-b503-4c93-b04f-90f463ae8d57" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/23588695-769f-451d-9a49-c74aa7851746" />


Testing in browser
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/5fe14ad0-7ffa-4e41-9bcf-3e75462f1ece" />




























<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/37fdcfd8-31ee-4d0f-90ef-039e349db70d" />

Step :1 – Cloning flask app from github repository
Repo link: https://github.com/mohanDevOps-arch/flask_practice
