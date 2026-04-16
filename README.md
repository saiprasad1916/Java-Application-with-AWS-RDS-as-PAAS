## Java Web App Deployment on AWS (PaaS Model)
This project demonstrates a full-stack deployment of a Java Web Application using Amazon EC2 for the application tier and Amazon RDS as a Platform-as-a-Service (PaaS) database solution.
------------------------------
## 🏗️ Architecture Overview

* Operating System: Ubuntu Server 24.04 LTS
* Application Server: Apache Tomcat 9
* Database: Amazon RDS MySQL 8.0 (Instance: db.t3.micro)
* Build Tool: Maven
* Java Version: OpenJDK 17

------------------------------
## 🛠️ Step-by-Step Setup## 1. Networking & Security
Created a unified Security Group (webappsg) to allow seamless communication:

* SSH (22): Management access.
* HTTP (80) & TCP (8080): Web traffic for Tomcat.
* MySQL (3306): Internal link between EC2 and RDS (Source restricted to webappsg).
* screenshots:
<img width="692" height="303" alt="image" src="https://github.com/user-attachments/assets/b0f3592e-9d38-475e-85db-ad4ca451254c" />

 *  Add inbound rules:
   
<img width="691" height="295" alt="image" src="https://github.com/user-attachments/assets/c0e0734a-ba35-4cea-b2f6-a3707956c598" />
<img width="691" height="183" alt="image" src="https://github.com/user-attachments/assets/7fd31288-0233-44fd-badc-198a33eef02a" />




## 2. RDS (Database-as-a-Service)

* Engine: MySQL (Free Tier).
* DB Name: login_db (or jwt based on application code).
* Connectivity: Public Access enabled with webappsg attached.
* Endpoint: database-1.c1miu8i8yvj3.us-east-1.rds.amazonaws.com
* Steps:  
   1. Navigate to RDS Console > Databases > Create database.
   2. Choose a database creation method: Select Standard create.
   3. Engine options: Select MySQL.
   4. Templates: Select Free tier.
   5. Settings:
    DB instance identifier: mydbinstance.
       Master username: admin.
       Master password: Set a strong password.
   6. Instance configuration: Ensure db.t3.micro is selected (default for Free Tier).
   7. Connectivity:
    Public access: Select Yes.
       VPC security group: Select Choose existing and pick webappsg.
   8. Additional configuration: Under Initial database name, enter login_db.
   9. Click Create database. Copy the Endpoint URL once the status is "Available".
* Screenshots:
<img width="691" height="193" alt="image" src="https://github.com/user-attachments/assets/0e46ca43-872e-44c4-ac2e-8422e29077ff" />
<img width="691" height="236" alt="image" src="https://github.com/user-attachments/assets/a37c9c7f-d001-4436-ac31-ec6eae86933c" />
<img width="691" height="199" alt="image" src="https://github.com/user-attachments/assets/62a7f38d-762b-4d66-83e1-741e3bf6d35e" />
<img width="691" height="233" alt="image" src="https://github.com/user-attachments/assets/b37fc04a-c3c1-480f-a53e-31cb18260a74" />
<img width="691" height="255" alt="image" src="https://github.com/user-attachments/assets/77172a2f-b500-454f-b119-bd41e043dbc4" />







## 3. EC2 Provisioning

* Instance: t3.micro.
* AMI: Ubuntu 24.04 LTS.
* Deployment: Attached the webappsg security group during launch.
* Steps:
   1. Navigate to EC2 Console > Instances > Launch instances.
   2. Application and OS Images (Amazon Machine Image): Select Ubuntu (Ubuntu Server 24.04 LTS).
   3. Instance type: Select t3.micro.
   4. Key pair: Create or select an existing .pem key for SSH.
   5. Network settings:
    Click Edit.
       Security Groups: Select Select existing security group and choose webappsg.
   6. Click Launch instance.
* Screenshots:
<img width="691" height="298" alt="image" src="https://github.com/user-attachments/assets/b41d7c08-6f31-4c49-96f8-87661a8a7da0" />
<img width="692" height="240" alt="image" src="https://github.com/user-attachments/assets/eaa14175-a0ad-490d-a0cb-8974fc6ef7a8" />
<img width="691" height="175" alt="image" src="https://github.com/user-attachments/assets/db54ce68-32ec-4923-9a4e-ec65642b2d12" />


## 4. Software Environment

# System Update
sudo apt update && sudo apt upgrade -y
# Dependency Installation
sudo apt install openjdk-17-jdk maven mariadb-client git tomcat9 -y
# Services
sudo systemctl start tomcat9

------------------------------
## 🚀 Application Deployment## Build Process

   1. Clone Source: git clone https://github.com/ashureev/aws-rds-java
   2. Configuration: Update DBConnection.java or JSP files with the RDS Endpoint, Username (admin), and Password.
   3. Compilation:
   
   mvn clean package
SSH into your Ubuntu server: ssh i yourkey.pem ubuntu@yourec2ip

   1. Update and install dependencies:
   
   sudo apt update && sudo apt upgrade y
   sudo apt install openjdk17jdk maven mariadbclient git y
   
   2. Install Tomcat 9:
   
   sudo apt install tomcat9 y Start it
   sudo systemctl start tomcat9
   
   3. Clone & Build:
   
   git clone https://github.com/ashureev/aws-rds-java
   cd awsrdsjava
   
   4. Update Database Config:
   Update the DBConnection.java or config file in the project with your RDS Endpoint, admin username, and password.
   5. Build:
   
   mvn clean package
6. Deploy:
   
   sudo cp target/LoginWebApp.war /var/lib/tomcat9/webapps/

* Screenshots:
*  <img width="692" height="357" alt="image" src="https://github.com/user-attachments/assets/ba8ee963-ecd7-49b1-9089-9c0ebd2b6f25" />
*  database creation
<img width="692" height="330" alt="image" src="https://github.com/user-attachments/assets/8a00079f-dd52-4ad5-baff-c261528fdbc0" />

<img width="692" height="378" alt="image" src="https://github.com/user-attachments/assets/622f1ff3-3431-4e16-9843-cf24d8194dab" />

* Install Tomcat
<img width="692" height="256" alt="image" src="https://github.com/user-attachments/assets/0482ebad-32a7-4ef5-a290-2743b1f0d746" />

* Update Login.jsp

<img width="692" height="197" alt="image" src="https://github.com/user-attachments/assets/ef7b7b88-ecf9-4209-9138-1f5cf0ca7129" />

* Update UserRegistration.jsp:

 <img width="691" height="239" alt="image" src="https://github.com/user-attachments/assets/43eb1503-1385-4ccf-aba8-9e29975e27d9" />

* Add user 

* Add user in tomcat-users.xml

<img width="692" height="56" alt="image" src="https://github.com/user-attachments/assets/79f308ad-083c-4744-b5d3-5cb452e6277e" />

<img width="692" height="250" alt="image" src="https://github.com/user-attachments/assets/66cd4f49-38d6-430e-aa2e-59f51e246d1a" />

  * Edit Meta-Inf

<img width="692" height="128" alt="image" src="https://github.com/user-attachments/assets/7ab05cb0-4730-48ec-9831-19cd77e946b7" />

 * Commented out the following in context.xml

<img width="691" height="270" alt="image" src="https://github.com/user-attachments/assets/8b3b872c-b391-4363-9536-ce6b030ce29a" />

## Tomcat Configuration
<img width="692" height="256" alt="image" src="https://github.com/user-attachments/assets/47921d96-fdbd-4112-961f-7888788b7fb7" />

   1. Deploy WAR:
   
   sudo cp target/LoginWebApp.war /var/lib/tomcat9/webapps/
   
   2. Context Overrides: Edited context.xml and tomcat-users.xml to allow Manager access and define resource links.
   3. Security Tweak: Commented out the Remote Address Valve in context.xml to allow external access to management consoles (for dev environments).

<img width="691" height="248" alt="image" src="https://github.com/user-attachments/assets/62ddfe34-77cd-4f2f-938a-a2b1b2340ee4" />

------------------------------
## 🚦 Validation & Testing
The application is successfully accessible via the Public IPv4 address:

* Registration: http://54.92.212.129:8080/LoginWebApp/register.jsp
* Login: http://54.92.212
 * Login page

Result: User data is successfully persisting in the RDS instance and reflecting in the UI upon login.
<img width="693" height="234" alt="image" src="https://github.com/user-attachments/assets/cc3e436d-a3e5-4592-a18d-5990d6797fa5" />
 
  * Registration Page
<img width="691" height="337" alt="image" src="https://github.com/user-attachments/assets/252acb34-bce4-483e-a59f-54174e428026" />

  * User login
<img width="691" height="333" alt="image" src="https://github.com/user-attachments/assets/8b874274-cffa-41a7-9410-9f8c15d5b65f" />

------------------------------
Next Step: Would you like to automate this deployment using a Shell Script or Ansible Playbook?

