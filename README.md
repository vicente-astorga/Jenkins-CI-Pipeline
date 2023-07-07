## Jenkins CI Pipeline

### **Infrastructure**
The project uses three EC2 instances, on which the Jenkins, SonarQube, and Nexus Repository services run. Each service runs under its own *security group*, ensuring controlled traffic to only the necessary sources.
The SonarQube service runs with its PostgreSQL database, over a Nginx reverse proxy (9000 > 80 port).
To version the artifacts, the Timestamp plugin is used, which generates a unique date token with which each artifact uploaded to the Nexus Repository is named.

### **Pipeline in action**
When Developers make a change to the code, the commit is synced to the remote Github repository. Through the Github Webhook the Jenkins service detects and brings the latest changes. Unit tests and code analysis are then executed. The analysis reports are published to the SonarQube service, which presents the results and *passes* or *fails* according to the configured quality gates. Once the analysis is done, the artifact is built with Maven. If any dependencies are needed in the build, they will be stored in Nexus Repository. Once the artifact is packaged, it will be uploaded to Nexus Repository with its corresponding version. Every time the *continuous integration* process is triggered, a newz artifact will be created and stored in the Nexus Repository.

![Alt text](/files/diagrama.jpg "Image")
