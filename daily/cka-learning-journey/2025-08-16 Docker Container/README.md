
## 🔧 Activity
Built and ran a containerized Flask web application using Docker in my Linux PC, including creating a Docker image, running a container.

1. Created a simple Flask application in Python.
2. Wrote a Dockerfile to containerize the Flask app.
3. Built a Docker image and ran a container, exposing the app on port 5000.
4. Configured a Docker volume to persist application logs.
5. Tested the setup by accessing the app via a browser

## 🔍 Key Learnings
- Containerization: Understood how Docker containers isolate applications and their dependencies, ensuring consistency across environments.
- Dockerfile Structure: Learned key Dockerfile instructions (FROM, WORKDIR, COPY, RUN, EXPOSE, CMD) and their role in building images.
- Image vs. Container: Grasped the difference between a Docker image (static template) and a container (running instance).
- Volumes: Discovered how Docker volumes persist data outside the container lifecycle, critical for stateful applications.
- Networking: Learned to map container ports to host ports to access services (e.g., Flask app on port 5000).
- Troubleshooting: Gained experience debugging issues like missing dependencies, incorrect port mappings, and container crashes by checking logs with docker logs.

