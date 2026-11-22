React + Docker + Jenkins demo

Run locally: npm install && npm run dev

Docker: docker build -t react-jenkins-demo .
docker run -d --name react-jenkins-demo -p 8081:80 react-jenkins-demo

Open http://localhost:8081.

Jenkins: push this project to GitHub, create a Pipeline job using Pipeline script from SCM > Git, set branch */main and script path Jenkinsfile. Ensure Docker is installed and usable by the Jenkins agent. Install Email Extension Plugin and configure SMTP under Manage Jenkins > System > Extended E-mail Notification. The Jenkinsfile sends to vijay@gmail.com and praveen@gmail.com. Configure SMTP credentials securely in Jenkins, not in this repository. Allow inbound TCP 8081 for remote access.
