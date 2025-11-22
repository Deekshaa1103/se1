Build pipeline using Jenkins script

 
Pipeline for maven java project
 
 
 
 

Tomcat
 




Pipeline for maven web project
 
 
 
 

Creating a Class Diagram in StarUML is straightforward once you know the steps. Here's a clear, quick guide you can follow:
________________________________________
⭐ How to Create a Class Diagram in StarUML
1. Create/Open a Project
•	Launch StarUML
•	Go to File → New (or open an existing project)
2. Add a Class Diagram
•	In the Model Explorer (left panel):
o	Right-click on the Model
o	Select Add Diagram → UML → Class Diagram
A blank class diagram canvas will appear.
________________________________________
⭐ 3. Add Classes
•	Go to the Toolbox (right side)
•	Under UML Structural, select Class
•	Click anywhere on the canvas to place the class
Rename the class:
•	Double-click the class name and type (e.g., Student)
Add attributes and methods:
•	Double-click inside the class, then type:
o	Attributes (e.g., -name: String)
o	Operations/methods (e.g., +getName(): String)
________________________________________
⭐ 4. Add Relationships
In the Toolbox, choose the relationship type:
Association
•	Solid line between two classes
•	Use for general links (e.g., Person — Owns — Car)
Multiplicity
•	Click a line end → set multiplicity in the properties (e.g., 1..*, 0..1)
Generalization (Inheritance)
•	Arrow with hollow triangle (Child → Parent)
Aggregation
•	Hollow diamond (Whole → Part)
Composition
•	Filled diamond (Strong Whole → Part)
Dependency
•	Dashed line
Click the tool → click class A → click class B to connect.
________________________________________
⭐ 5. Arrange and Format
•	Drag classes to clean up layout
•	Use Auto Layout (if available)
•	Customize colors in the Properties panel
________________________________________
⭐ 6. Save or Export
•	Save: File → Save
•	Export image: File → Export Diagram as JPG/PNG/SVG/PDF
________________________________________

Here is a clear, simple guide to creating a Use Case Diagram in StarUML, similar to the class-diagram instructions you asked for earlier.
________________________________________
⭐ How to Create a Use Case Diagram in StarUML
1. Create or Open a Project
•	Open StarUML
•	Go to File → New (or open an existing project)
________________________________________
⭐ 2. Add a Use Case Diagram
•	In the Model Explorer (left side):
o	Right-click Model
o	Select Add Diagram → UML → Use Case Diagram
A blank use case diagram canvas will appear.
________________________________________
⭐ 3. Add Actors
•	From the Toolbox (right side):
o	Under UML Behavioral, select Actor
o	Click on the diagram to place the actor symbol
Rename the actor:
•	Double-click the actor name (e.g., Customer, Admin)
________________________________________
⭐ 4. Add Use Cases
•	In the Toolbox, choose Use Case
•	Click on the diagram to place it
Rename:
•	Double-click the oval name (e.g., Login, Place Order, Register)
________________________________________
⭐ 5. Add Relationships
Association (Actor ↔ Use Case)
•	In the Toolbox, select Association
•	Click the actor → click the use case
(creates a straight line showing interaction)
Include Relationship
•	Used when a use case always includes another (shared behavior)
•	Toolbox → Include
•	Click the base use case → required use case
(arrow with <<include>>)
Extend Relationship
•	Used when a use case optionally adds behavior
•	Toolbox → Extend
•	Click the extending use case → base use case
(arrow with <<extend>>)
Generalization
•	Actors or use cases can have inheritance
•	Toolbox → Generalization
•	Click child → parent
(e.g., Registered User → User)
________________________________________
⭐ 6. Organize and Format
•	Drag actors and use cases into position
•	Group related use cases visually
•	Adjust line anchors for clarity
________________________________________
⭐ 7. Save or Export
•	File → Save
•	File → Export Diagram as PNG/JPG/SVG/PDF
________________________________________
Nagios steps
 
MAVEN JAVA – FREESTYLE PIPELINE

Job 1: MavenJava_Build
New Item → Freestyle Project
Project name: MavenJava_Build

Source Code Management
Git URL, Branch: */main or */master

Add Build Step → Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: clean

Add Build Step → Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: install

Post-Build Actions
Archive the artifacts
Files to archive: **/*

Build other projects
Projects to build: MavenJava_Test
Trigger: Only if build is stable

---

Job 2: MavenJava_Test
New Item → Freestyle Project
Project name: MavenJava_Test

Build Environment
Check: Delete workspace before build starts

Build Steps
Copy artifacts from another project
Project name: MavenJava_Build
Build: Stable build only
Artifacts to copy: **/*

Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: test

Post-Build Actions
Archive the artifacts
Files to archive: **/*
---

Pipeline View for Maven Java
Dashboard → + → Build Pipeline View
Name: MavenJava_Pipeline
Layout: Based on upstream/downstream
Initial Job: MavenJava_Build
---

MAVEN WEB – PIPELINE (BUILD → TEST → DEPLOY)

Job 1: MavenWeb_Build
New Item → Freestyle Project
Project name: MavenWeb_Build

Source Code Management
Git URL, Branch: */main or master

Build Steps
Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: clean

Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: install

Post-Build Actions
Archive the artifacts
Files to archive: **/*

Build other projects
Projects to build: MavenWeb_Test
Trigger: Only if build is stable
---

Job 2: MavenWeb_Test
New Item → Freestyle Project
Project name: MavenWeb_Test

Build Environment
Check: Delete the workspace before build starts

Build Steps
Copy artifacts from another project
Project name: MavenWeb_Build
Build: Stable build only
Artifacts to copy: **/*

Invoke top-level Maven targets
Maven version: MAVEN_HOME
Goals: test

Post-Build Actions
Archive the artifacts
Files to archive: **/*

Build other projects
Projects to build: MavenWeb_Deploy
---

Job 3: MavenWeb_Deploy
New Item → Freestyle Project
Project name: MavenWeb_Deploy

Build Environment
Check: Delete the workspace before build starts

Build Steps
Copy artifacts from another project
Project name: MavenWeb_Test
Build: Stable build only
Artifacts to copy: **/*

Post-Build Actions
Deploy WAR/EAR to a container
WAR/EAR File: **/*.war
Context path: Webpath
Add container: Tomcat 9.x remote
Credentials: admin / 1234
Tomcat URL: [https://localhost:8080/]
---

Pipeline View for Maven Web
Dashboard → + → Build Pipeline View
Name: MavenWeb_Pipeline
Layout: Based on upstream/downstream
Initial Job: MavenWeb_Build
---
SCRIPTED PIPELINE 
Advanced Project Options
Definition: Choose “Pipeline script”

Pipeline Script-

pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'maven'
    }

    stages {

        stage('git repo & clean') {
            steps {
                bat "rmdir /s /q 23BD1A051X_se_java || exit 0"
                bat "git clone https://github.com/SarvikaSomishetty/23BD1A051X_se_java.git"
                bat "mvn clean -f 23BD1A051X_se_java/pom.xml"
            }
        }

        stage('install') {
            steps {
                bat "mvn install -f 23BD1A051X_se_java/pom.xml"
            }
        }

        stage('test') {
            steps {
                bat "mvn test -f 23BD1A051X_se_java/pom.xml"
            }
        }

        stage('package') {
            steps {
                bat "mvn package -f 23BD1A051X_se_java/pom.xml"
            }
        }
    }
}

---
MINIKUBE
minikube start
minikube kubectl -- get pods -A

kubectl create deployment mynginx --image=nginx
kubectl get deployments
kubectl get pods
kubectl describe pods
kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80
kubectl scale deployment mynginx --replicas=4
kubectl get service mynginx
kubectl port-forward svc/mynginx 8081:80

NAGIOS
docker pull jasonrivers/nagios:latest
docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest
http://localhost:8888
	Username: nagiosadmin
	Password: nagios
---
NGROK
Get your auth token from dashboard.
Run in ngrok terminal:
ngrok config add-authtoken <your_token>
ngrok http 8888(copy url)
Open GitHub → Repo → Settings → Webhooks → Add Webhook
Payload URL:
https://your-ngrok-url/github-webhook/
Content type: application/json
Jenkins Dashboard → Select your job → Configure
Build Triggers → enable:
GitHub hook trigger for GITScm polling

---
GMAIL
Go to Security → App Passwords
App: Other
Name: Jenkins-Demo
Generate → copy 16-digit password.
Configure Jenkins Email Settings
Manage Jenkins → Configure System

A. Email Notification
SMTP Server: smtp.gmail.com
Use SMTP Auth: enabled
Username: sarvikashetty30@gmail.com
Password: 16-digit App Password
Use SSL: enabled
SMTP Port: 465
Reply-To: sarvikashetty30@gmail.com

B. Extended Email Notification
SMTP Server: smtp.gmail.com
SMTP Port: 465
Use SSL: enabled
Credentials: Gmail + App Password
Content type: text/html

Configure Job-level Email Notification
Job → Configure → Post-build Actions
Add: Editable Email Notification
Recipient list: somisettysarvika@gmail.com
Triggers:Always
---

AWS
Open course invitation mail → Start → AWS Academy → Student login → Modules → Launch AWS Academy Lab → Wait for AWS to turn green → Click AWS.
Click EC2 → Launch Instance
Stage 1: Name → ubuntu
Stage 2: AMI → Ubuntu Server (Free Tier Eligible)
Stage 3: Architecture → 64-bit
Stage 4: Instance type → t2.micro
Stage 5: Create new keypair (.pem file) → save to AWS folder
Launch Instance
Connect
Copy SSH command from AWS (ssh -i …)
Open PowerShell or Git Bash → cd to folder containing .pem file
Run ssh command to connect to EC2 Ubuntu.

sudo apt update
sudo apt-get install docker.io
sudo apt install git
sudo apt install nano

git clone <repo-url>
cd into folder and run ls

sudo docker build -t mywebapp .
sudo docker run -d -p 80:80 mywebapp

If app does not load → fix Security Group
Go to EC2 → Security → Security Groups
Edit inbound rules → Add Rule
Custom TCP → Port 9090 → Source 0.0.0.0/0
