Docker Assignment Instructions
How to make containers, Images, and Compose on Docker for Windows
Student Name: Henil Patel
Student Number: 000903845
Email: henilpatelmiteshbhai12@gmail.com
Github Link: 
________________________________________
1. Introduction
This instruction document will guide you through the steps required to complete both challenges in the Docker assignment. Follow each step carefully, and ensure you take screenshots as indicated.
Basic Introduction to Docker: Docker is a tool that allows developers to package an application along with all its dependencies and configurations into a container. This container can be run on any computer that has Docker installed, ensuring consistency across different environments.
Docker Commands Used
During this project, I have used several Docker commands those were used to build, run, and manage containers:
•	docker --version: Verify Docker installation.
•	docker build -t <image_name> .: Build a Docker image from a Dockerfile.
•	docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>: Run a Docker container from an image.
•	docker ps: List running containers.
•	docker stop <container_id>: Stop a running container.
•	docker rm <container_id>: Remove a stopped container.
•	docker-compose up --build: Build and run services defined in a docker-compose.yml file.
•	docker-compose down: Stop and remove containers defined in a docker-compose.yml file.
•	docker logs <container_id>: Fetch the logs of a container.

Importance of Docker in Software Development: Docker is crucial for software development as it ensures that applications run the same way in different environments. This eliminates the "works on my machine" problem, making deployment and testing more reliable.

________________________________________
Steps to Make Docker Work
2. Challenge 1 - Simple Static Page Server
2.1 Install Docker Desktop
1.	Download Docker Desktop for Windows from the Docker website.
2.	Follow the installation wizard and restart your computer.
3.	Verify the installation by running docker --version in the command prompt or PowerShell.
2.2 Clone the Repository
1.	Clone the repository using the following command:
git clone https://github.com/eduluz1976/docker-challenge-template
cd docker-challenge-template/challenge1
2.3 Create Necessary Files
1.	Create a directory named public inside challenge1:
mkdir public
2.	Create an index.html file inside the public directory with the following content:
3.	<html>
4.	<head>
5.	  <title>Docker Challenge</title>
6.	</head>
7.	<body>
8.	  <h1>Henilkumar Patel</h1>
9.	  <p>SAIT ID: 000903845</p>
10.	</body>
11.	</html>
o	Replace Your Name and YourID with your actual name and SAIT ID.
2.4 Create Dockerfile
1.	Create a Dockerfile in the challenge1 directory with the following content:
2.	FROM nginx:latest
3.	COPY public /usr/share/nginx/html
4.	EXPOSE 80
5.	CMD ["nginx", "-g", "daemon off;"]
6.	
2.5 Build the Docker Image
1.	Run the following command to build the Docker image:
docker build -t my-static-site .
 
2.6 Run the Docker Container
1.	Run the container using the following command:
docker run -d -p 8080:80 --name static-site my-static-site
 
2.7 Verify the Setup
1.	Open your web browser and navigate to http://localhost:8080/.
 
2.8 Commit and Push Changes
1.	Add, commit, and push your changes to the repository:
git add Dockerfile public/index.html
git commit -m "Challenge 1 - Static page server"
git push origin main
2.9 Submit the Assignment
1.	Take a screenshot of the running application.
2.	Write a report with all steps, screenshots, and answers to any questions.
3.	Submit the report as a PDF on D2L with the repository URL.
________________________________________
3. Challenge 2 - NodeJS Application
3.1 Folder Setup
1.	Navigate to the challenge2 folder:
cd ../challenge2
2.	Extract the contents of challenge2.zip to the root of challenge2.
3.2 Create Dockerfile
1.	Create a Dockerfile in the challenge2 directory with the following content:
2.	FROM node:alpine
3.	
4.	WORKDIR /usr/src/app
5.	COPY package*.json ./
6.	RUN npm install
7.	COPY . .
8.	EXPOSE 3000
9.	
10.	CMD ["node", "server.js"]
11.	
3.3 Create docker-compose.yml
1.	Create a docker-compose.yml file in the challenge2 directory with the following content:
2.	version: '3'
3.	services:
4.	  nginx:
5.	    image: nginx:latest
6.	    ports:
7.	      - "8080:80"
8.	    volumes:
9.	      - ./nginx.conf:/etc/nginx/nginx.conf:ro
10.	    depends_on:
11.	      - api
12.	
13.	  api:
14.	    build: .
15.	    ports:
16.	      - "3000:3000"
17.	
3.4 Create Nginx Configuration
1.	Create a nginx.conf file in the challenge2 directory with the following content:
2.	events {
3.	    worker_connections 1024;
4.	}
5.	
6.	http {
7.	    server {
8.	        listen 80;
9.	
10.	        location /api {
11.	            proxy_pass http://api:3000;
12.	            proxy_http_version 1.1;
13.	            proxy_set_header Upgrade $http_upgrade;
14.	            proxy_set_header Connection 'upgrade';
15.	            proxy_set_header Host $host;
16.	            proxy_cache_bypass $http_upgrade;
17.	        }
18.	    }
19.	}
20.	
3.5 Build and Run the Docker Services
1.	Rebuild and start your Docker containers:
docker-compose up --build
 
3.6 Verify the Setup
1.	Open your browser and navigate to http://localhost:8080/api/books.
 
2.	Navigate to http://localhost:8080/api/books/1.
 
3.7 Commit and Push Changes
1.	Add, commit, and push your changes to the repository:
git add Dockerfile docker-compose.yml nginx.conf
git commit -m "Challenge 2 - NodeJS application"
git push origin main
________________________________________

4. Troubleshooting
4.1 Common Issues and Solutions
Issue: Docker is not installed correctly.
•	Solution: Verify the Docker installation by running docker --version. If the command is not recognized, reinstall Docker Desktop and ensure it is set up correctly.
Issue: Docker image build fails.
•	Solution: Check the Dockerfile for syntax errors. Ensure all necessary files are in the correct directories. Refer to the Docker build output for specific error messages and address them accordingly.
Issue: Container does not start or exits immediately.
•	Solution: Run docker logs <container_id> to view the error messages. Common issues include missing files, incorrect file paths, and syntax errors in the Dockerfile or application code.
Issue: "404 Not Found" when accessing the static page or API.
•	Solution:
o	For static pages, ensure that the index.html file is correctly placed in the public directory and that the Dockerfile correctly copies this directory.
o	For API endpoints, ensure the NodeJS application is running correctly and that the Nginx configuration is properly set up to proxy requests to the NodeJS service.
Issue: "403 Forbidden" error from Nginx.
•	Solution: Check the permissions of the files and directories being served by Nginx. Ensure they are readable by the Nginx process. You can set the permissions using:
chmod -R 755 ./public
Issue: Port conflicts.
•	Solution: Ensure no other services are running on the ports required by your Docker containers (e.g., 8080 and 3000). Stop any conflicting services or change the ports in your Docker configuration.
4.2 Detailed Troubleshooting Steps
Step 1: Check Container Logs
1.	Use the following command to check the logs of a running container:
docker logs <container_id>
2.	Analyze the logs for any error messages or warnings that can help identify the issue.
Step 2: Verify File Permissions
1.	Ensure that all files in the public directory have the correct permissions:
chmod -R 755 ./public
Step 3: Inspect Docker Network
1.	Check the network configuration of your Docker containers to ensure they are correctly connected:
docker network ls
docker network inspect <network_name>
Step 4: Rebuild and Restart Containers
1.	If changes are made to the configuration files, rebuild and restart the containers:
docker-compose down
docker-compose up --build
Step 5: Verify Nginx Configuration
1.	Ensure the nginx.conf file is correctly configured to serve static files and proxy API requests.
Step 6: Test API Endpoints
1.	Use a tool like curl or Postman to test the API endpoints directly:
curl http://localhost:8080/api/books
By following these troubleshooting steps, you should be able to resolve common issues encountered during the Docker challenges. If problems persist, review the Docker documentation and seek additional support from the community or your instructor.
________________________________________
5. Lessons Learned
During this assignment, several challenges were encountered and resolved, providing valuable learning experiences in Docker containerization. Key takeaways include:
•	Understanding Docker Basics: Gained practical experience in creating Dockerfiles and building Docker images.
•	Container Management: Learned how to run, stop, and manage Docker containers effectively.
•	Troubleshooting Skills: Developed problem-solving skills by troubleshooting and resolving issues related to Docker configuration and network setup.
•	Importance of Consistency: Recognized the importance of Docker in ensuring consistency across different development and production environments.
6. References
•	Docker Official Documentation: https://docs.docker.com/
•	YouTube Tutorial: Docker Tutorial for Beginners
•	Book: "Docker Deep Dive" by Nigel Poulton
•	Online Tutorial: GeeksforGeeks Docker Tutorial

