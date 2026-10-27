# Library Management System — ready-to-run project

This project uses Java 21, Maven, Spring Boot, and Thymeleaf. It is adapted from the supplied project files. Docker Hub image names are set to `jino2006/library-management-system:v1`.

## Run locally in VS Code / terminal
1. Install JDK 21 and Maven.
2. Open this folder in VS Code.
3. In a terminal at this folder, run:
   ```bash
   mvn clean package
   mvn spring-boot:run
   ```
4. Open http://localhost:8082

If `mvn` is not recognized, install Maven and add its `bin` directory to PATH. Confirm with `java -version` and `mvn -version`.

## Run with Docker manually
Make sure Docker Desktop is running, then run:
```bash
mvn clean package -DskipTests
docker build -t jino2006/library-management-system:v1 .
docker run --rm -p 8082:8082 jino2006/library-management-system:v1
```
Then open http://localhost:8082.

## Jenkins / Docker Hub
- Add a Jenkins credential of kind **Username with password** with ID `dockerhub-creds`.
- Username: `jino2006`; password: a Docker Hub access token with write permissions.
- Ensure Docker CLI is available to the Windows account/service running Jenkins. In an Administrator terminal, `where docker` should locate `docker.exe`. If Jenkins runs as a Windows service and cannot find Docker, add the Docker CLI folder to the **System** PATH and restart the Jenkins service. Do not put a Docker Hub token in this file.
- Jenkinsfile pushes `jino2006/library-management-system:v1` and applies the Kubernetes YAML files.
- Kubernetes deployment requires a working `kubectl` context/cluster on the Jenkins machine. For Docker Desktop Kubernetes, enable Kubernetes in Docker Desktop first and verify `kubectl get nodes`.

## Kubernetes check
After the pipeline deploys successfully, run:
```bash
kubectl get deployments
kubectl get pods
kubectl get services
```
For Docker Desktop Kubernetes, the service is configured as NodePort 30082; try http://localhost:30082.

Note: the supplied code's home controller serves the `index` Thymeleaf template. The included Java module files are part of the supplied project, but confirm their screens/features after starting the app.
