
# Docker Cross-Account Deployment Project

Welcome to the Docker Cross-Account Deployment Project! This collaborative project pairs you with a partner (Player A) to build, share, and deploy a simple web application using Docker and AWS services. You’ll create a Docker container image, store it in Amazon Elastic Container Registry (ECR), share it securely with your partner, and deploy their application using AWS Elastic Beanstalk. This project introduces you to containerization, cloud storage, secure access management, and application deployment, equipping you with practical DevOps skills.

## Project Goals
- **Containerization**: Build a Docker image for a simple web app.
- **Cloud Storage**: Store and manage container images in Amazon ECR.
- **Secure Sharing**: Configure AWS IAM for cross-account image access.
- **Deployment**: Run and deploy your partner’s app locally and on Elastic Beanstalk.
- **Resource Management**: Clean up resources to avoid AWS charges.

## Table of Contents
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Step 1: Install Docker](#step-1-install-docker)
- [Step 2: Create Dockerfile and Web Application](#step-2-create-dockerfile-and-web-application)
- [Step 3: Build Your Container Image](#step-3-build-your-container-image)
- [Step 4: Log in to AWS with an IAM User](#step-4-log-in-to-aws-with-an-iam-user)
- [Step 5: Set Up Amazon ECR](#step-5-set-up-amazon-ecr)
- [Step 6: Install and Configure AWS CLI](#step-6-install-and-configure-aws-cli)
- [Step 7: Push Your Docker Image to ECR](#step-7-push-your-docker-image-to-ecr)
- [Step 8: Pull and Run Your Partner’s Container Image](#step-8-pull-and-run-your-partners-container-image)
- [Step 9: Resolve Permission Issues](#step-9-resolve-permission-issues)
- [Step 10: Deploy Your Partner’s App on Elastic Beanstalk](#step-10-deploy-your-partners-app-on-elastic-beanstalk)
- [Step 11: Clean Up Resources](#step-11-clean-up-resources)
- [Share Your Success](#share-your-success)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Conclusion](#conclusion)

## Project Overview
In this project, you (Player B) and your partner (Player A) will each create a simple web application, package it into a Docker container image, and store it in a private Amazon ECR repository. You’ll configure AWS Identity and Access Management (IAM) to securely share your images with each other, pull your partner’s image, run it locally, and deploy it on AWS Elastic Beanstalk. This simulates real-world DevOps workflows, where teams collaborate across accounts to build and deploy applications. By the end, you’ll understand containerization, cloud registries, secure access control, and scalable deployment.

### Why Containers?
Containers solve the problem of application portability. Without containers, applications may behave differently across computers due to variations in operating systems, libraries, or configurations. Containers bundle an application with its dependencies (e.g., code, runtime, libraries) into a single, portable package, ensuring consistent execution anywhere a container platform (like Docker) is available.

### Why Docker and AWS?
- **Docker**: Simplifies creating, managing, and running containers with an intuitive interface (Docker Desktop) and powerful command-line tools.
- **Amazon ECR**: A secure, managed registry for storing container images in the cloud, ideal for private team projects.
- **AWS IAM**: Enables fine-grained access control, allowing secure sharing of resources like ECR repositories.
- **Elastic Beanstalk**: Automates application deployment, scaling, and management, making it easy to host containerized apps.

### Collaboration Tip
Communicate frequently with Player A. Share progress, troubleshoot together, and ensure both of you complete each step. The more you discuss, the more you’ll learn!

## Prerequisites
Before starting, ensure you have:
- A computer with a reliable internet connection.
- An AWS account (sign up at the AWS website if you don’t have one).
- Basic familiarity with terminal commands (e.g., navigating directories, running commands).
- A project partner (Player A) to collaborate with.
- Access to a community platform (e.g., a forum or Discord) for support if you get stuck.
- A text editor (e.g., VS Code, Notepad++, or Notepad) for editing files.
- A web browser (e.g., Chrome, Firefox) for testing and accessing AWS.

## Step 1: Install Docker
Docker is a platform for building, managing, and running containers. Docker Desktop provides a user-friendly interface and command-line tools for this project.

1. **Check if Docker is Installed**:
   - Open your terminal:
     - **Mac/Linux**: Use Terminal.
     - **Windows**: Use Command Prompt or PowerShell.
   - Run:
     ```bash
     docker --version
     ```
   - If a version number appears (e.g., `Docker version 20.10.0`), Docker is installed. Verify with Player A that they have Docker installed, then proceed to [Step 2](#step-2-create-dockerfile-and-web-application).
   - If you see `command not found`, proceed with installation.

2. **Download Docker Desktop**:
   - Visit the official Docker website and navigate to the Docker Desktop download page.
   - Select the version for your operating system:
     - **Mac**:
       - Go to Apple menu > About This Mac.
       - Note your chip type: **Apple** (e.g., M1, M2) or **Intel**.
       - Download the appropriate version (Apple Silicon or Intel).
     - **Windows**:
       - Go to Start > Search for “System Information”.
       - Note your System Type: **x64-based** or **ARM-based**.
       - Download the matching version.
     - **Linux**:
       - Open a terminal and run:
         ```bash
         uname -m
         ```
       - Note the output: `x86_64` (Intel/AMD) or `aarch64/arm64` (ARM).
       - Download the correct version for your distribution (e.g., Ubuntu, Fedora).

3. **Install Docker Desktop**:
   - **Mac**:
     - Double-click the `Docker.dmg` file.
     - Drag the Docker icon to the Applications folder.
     - Open Docker from Applications.
   - **Windows**:
     - Double-click `Docker Desktop Installer.exe`.
     - Follow the installer prompts (default installation path: `C:\Program Files\Docker\Docker`).
     - Launch Docker Desktop.
   - **Linux**:
     - Follow your distribution’s instructions (e.g., for Ubuntu, use `apt` or `snap` to install Docker Desktop).
     - Launch Docker Desktop.
   - If prompted with a confirmation popup (e.g., “Do you want to open this file?”), select **Open** or **Okay**.

4. **Verify Installation**:
   - Open Docker Desktop to ensure it’s running.
   - In your terminal, run:
     ```bash
     docker --version
     ```
   - Expect output like `Docker version 20.10.0, build abc123`.
   - If you see `command not found`:
     - Restart Docker Desktop and your terminal.
     - Open Docker Desktop’s terminal (bottom bar) and run `docker --version`.
     - If the version appears in Docker’s terminal but not your regular terminal, Docker is installed but not added to your system’s PATH. Check Docker’s notification bell for repairable issues or reinstall.
     - If issues persist, reinstall Docker or seek community help.

5. **Best Practices**:
   - Ensure Docker Desktop is running before executing Docker commands.
   - Keep Docker updated to avoid compatibility issues.
   - Coordinate with Player A to confirm they’ve completed this step.

## Step 2: Create Dockerfile and Web Application
Create a project directory, a Dockerfile to define your container, and an `index.html` file for your web app.

1. **Create Project Directory**:
   - In your terminal, navigate to your Desktop and create a folder named `DockerECR`:
     ```bash
     cd ~/Desktop
     mkdir DockerECR
     cd DockerECR
     ```
   - **Purpose**: This directory will hold all files for your container image, keeping your project organized.

2. **Create Dockerfile**:
   - Create a file named `Dockerfile`:
     ```bash
     touch Dockerfile
     ```
     - **Windows Users**: If `touch` fails, use:
       ```bash
       New-Item -Path . -Name "Dockerfile" -ItemType "File"
       ```
   - Open `Dockerfile` in a text editor (e.g., VS Code, Notepad++, TextEdit in plain text mode).
   - Add the following:
     ```dockerfile
     FROM nginx:latest
     COPY index.html /usr/share/nginx/html/
     ```
   - Save the file (`Ctrl+S` or `Command+S`).
   - **Explanation**:
     - `FROM nginx:latest`: Uses the latest Nginx image as the base. Nginx is a lightweight web server that serves your web app.
     - `COPY index.html /usr/share/nginx/html/`: Copies your `index.html` file to Nginx’s default directory, replacing its default webpage.
   - **Why Nginx?**: Nginx is efficient and widely used for serving static web content, making it ideal for this simple project.

3. **Create index.html**:
   - Create an `index.html` file:
     ```bash
     touch index.html
     ```
     - **Windows Users**: If `touch` fails, use:
       ```bash
       New-Item -Path . -Name "index.html" -ItemType "File"
       ```
   - Open `index.html` in a text editor and add:
     ```html
     <html>
       <head>
         <title>Hello [Player A's Name]!</title>
       </head>
       <body>
         <h1>Hello from [Your Name]!</h1>
         <h1>If you can see this, you've deployed my app... nice work!</h1>
         <h1>You've unlocked my secret code: [Your Secret Code]</h1>
       </body>
     </html>
     ```
   - **Customizations**:
     - Replace `[Player A’s Name]` with your partner’s name (e.g., “Alice”).
     - Replace `[Your Name]` with your name (e.g., “Bob”).
     - Replace `[Your Secret Code]` with your favorite food, movie, or song (e.g., “Sushi”, “The Matrix”, “Imagine”). This is a fun secret for Player A to discover when they deploy your app—don’t share it!
   - **Optional Enhancements**:
     - Add more content to make the page engaging, e.g.:
       ```html
       <h1>Something I learned about you today is...</h1>
       <h2>Here’s a special image chosen by me:</h2>
       <img src="[Public Image URL]" alt="Custom Image" style="width:100%; max-width:300px;">
       ```
     - Find a public image online (e.g., via Unsplash or Pexels), right-click, copy the image address, and paste it into `[Public Image URL]`.
     - Example:
       ```html
       <img src="https://images.unsplash.com/photo-1507525428034-b723cf961d3e" alt="Beach" style="width:100%; max-width:300px;">
       ```
   - Save the file.

4. **Verify index.html**:
   - Open `index.html` in a browser:
     - Navigate to `~/Desktop/DockerECR`.
     - Right-click `index.html` > Open With > Browser (e.g., Chrome, Firefox).
   - Confirm the page displays your customized content without errors.
   - **Common Issues**:
     - If using TextEdit on Mac, it may save as rich text (`.rtf`), causing errors. Open in VS Code or another code editor, ensure plain text, and resave as `index.html`.
     - If the browser shows garbled text or errors, check for typos in the HTML or incorrect file encoding. Fix and resave.
     - Seek community help if issues persist.

5. **Best Practices**:
   - Use a code editor like VS Code for editing files to avoid formatting issues.
   - Test your HTML locally before building the container to catch errors early.
   - Share customization ideas with Player A to make the project fun and interactive.

## Step 3: Build Your Container Image
Build your Docker container image using the `Dockerfile` and `index.html`.

1. **Understand the Build Process**:
   - A **Dockerfile** is a recipe with instructions for creating a container image.
   - A **container image** is a template containing your app, dependencies, and configurations.
   - The **build** process compiles these instructions into an image that can be run as a container.

2. **Build the Image**:
   - Ensure you’re in the `DockerECR` directory:
     ```bash
     cd ~/Desktop/DockerECR
     ```
   - Run:
     ```bash
     docker build -t cross-account-docker-app .
     ```
     - **Explanation**:
       - `docker build`: Initiates the build process.
       - `-t cross-account-docker-app`: Tags the image with the name `cross-account-docker-app`.
       - `.`: Specifies the current directory as the build context (includes `Dockerfile` and `index.html`).
   - Grant any permissions requested by your terminal (Docker requires access to the daemon).

3. **Monitor the Build**:
   - Your terminal will display progress, showing steps like downloading the Nginx image and copying files.
   - Expect output like:
     ```
     Step 1/2 : FROM nginx:latest
     Step 2/2 : COPY index.html /usr/share/nginx/html/
     Successfully built abc123def456
     Successfully tagged cross-account-docker-app:latest
     ```

4. **Verify the Build**:
   - Open Docker Desktop and navigate to the **Images** tab.
   - Confirm `cross-account-docker-app:latest` appears.
   - Alternatively, run:
     ```bash
     docker images
     ```
     - Look for `cross-account-docker-app` in the output.
   - **Common Issues**:
     - **Docker Daemon Error**: Ensure Docker Desktop is running. Restart it if needed.
     - **File Not Found**: Verify `Dockerfile` and `index.html` are in the `DockerECR` directory.
     - Seek community help for persistent errors.

5. **Best Practices**:
   - Name images descriptively (e.g., `cross-account-docker-app`) for clarity.
   - Verify the image immediately after building to catch errors early.
   - Coordinate with Player A to ensure they’ve built their image.

## Step 4: Log in to AWS with an IAM User
Use an IAM user for AWS access to follow security best practices, avoiding the root user.

1. **Understand IAM**:
   - The **root user** is the primary account owner with full access. AWS recommends limiting its use to protect your account.
   - **IAM users** are sub-accounts with specific permissions, safer for daily tasks.

2. **Check Existing IAM User**:
   - Log in to the AWS Management Console with an IAM user.
   - If you have an IAM user with **AdministratorAccess**, skip to [Step 5](#step-5-set-up-amazon-ecr).

3. **Create an IAM User**:
   - Log in to the AWS Management Console as the root user.
   - Navigate to **IAM** > **Users** > **Create user**.
   - Set **User name** to `Yourname-IAM-Admin` (e.g., `Bob-IAM-Admin`).
   - Check **Provide user access to the AWS Management Console**.
   - If prompted with “Are you providing access to a person?”, select **I want to create an IAM user**.
   - Choose **Custom password** and set a secure password (store it securely, e.g., in a password manager).
   - Uncheck **Users must create a new password at next sign-in**.
   - Click **Next**.
   - Select **Attach policies directly**.
   - Choose **AdministratorAccess** from the permissions policies list.
   - Click **Next** > **Create user**.
   - Download the `.csv` file with the user’s credentials (contains username, password, and sign-in URL).
   - Copy the **Console sign-in URL** from the user creation page.

4. **Log in with IAM User**:
   - Log out of the root user.
   - Paste the Console sign-in URL in your browser.
   - Log in using the IAM user’s username and password from the `.csv` file.
   - Verify you’re logged in as `Yourname-IAM-Admin` (check the top-right corner of the AWS Console).

5. **Best Practices**:
   - Never use the root user for routine tasks.
   - Store IAM credentials securely and avoid sharing them.
   - Ensure Player A creates their IAM user to maintain security.

## Step 5: Set Up Amazon ECR
Amazon Elastic Container Registry (ECR) is a managed service for storing and sharing container images.

1. **Understand ECR**:
   - **ECR** is a cloud-based registry for Docker images, similar to Docker Hub but private and integrated with AWS.
   - **Repositories** in ECR are like folders, each storing versions of a specific image.
   - **Why ECR?**: It ensures secure, centralized storage and easy sharing with team members, avoiding manual file transfers.

2. **Create an ECR Repository**:
   - In the AWS Management Console, navigate to **Elastic Container Registry (ECR)**.
   - Click **Create repository**.
   - Set **Repository name** to `cross-account-docker-app`.
   - Set **Image tag mutability** to **Mutable** (allows overwriting tags like `latest` for updates).
     - **Note**: Immutable tags prevent overwriting, ensuring stability but less flexibility.
   - Leave **Encryption configuration** as default (uses AES-256 for security).
   - Click **Create repository**.
   - **Verification**:
     - Confirm the repository appears in the ECR console.
     - Select the repository and click **View push commands** (you’ll use these in [Step 7](#step-7-push-your-docker-image-to-ecr)).

3. **Best Practices**:
   - Use descriptive repository names to avoid confusion.
   - Enable encryption for security.
   - Coordinate with Player A to create their ECR repository.

## Step 6: Install and Configure AWS CLI
The AWS Command Line Interface (CLI) allows you to manage AWS resources from your terminal.

1. **Understand AWS CLI**:
   - The AWS CLI enables programmatic access to AWS services, complementing the AWS Management Console.
   - It’s essential for pushing Docker images to ECR and managing permissions.

2. **Check if AWS CLI is Installed**:
   - Run:
     ```bash
     aws --version
     ```
   - If a version number appears (e.g., `aws-cli/2.13.0`), proceed to configure it.
   - If you see `command not found`, install the CLI.

3. **Install AWS CLI**:
   - **Mac**:
     ```bash
     curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
     sudo installer -pkg AWSCLIV2.pkg -target /
     ```
     - Enter your Mac password when prompted.
   - **Windows**:
     ```bash
     msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
     ```
     - Follow the installer prompts.
   - **Linux**:
     ```bash
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     unzip awscliv2.zip
     sudo ./aws/install
     ```
   - **Verification**:
     - Run:
       ```bash
       aws --version
       ```
     - Expect output like `aws-cli/2.13.0 Python/3.9.0`.
     - If no version appears:
       - Check for typos in the installation commands.
       - Visit the AWS CLI installation page for your OS.
       - Seek community help.

4. **Create an IAM User for CLI Access**:
   - In the IAM console, go to **Users** > **Create user**.
   - Set **User name** to `ECR-Access`.
   - Skip the checkbox for console access (this user is for CLI only).
   - Click **Next**.
   - Select **Attach policies directly**.
   - Search for and select **AmazonEC2ContainerRegistryFullAccess**.
     - **Purpose**: Grants full ECR access for pushing/pulling images.
   - Click **Next** > **Create user**.
   - In the user’s summary, click **Create access key**.
   - Select **Command Line Interface (CLI)**.
   - Add description: `Created for ECR access in cross-account deployment project`.
   - Check the acknowledgement box (acknowledges that IAM Identity Center is preferred for teams but not used here for simplicity).
   - Click **Create access key**.
   - Download the `.csv` file with **Access Key ID** and **Secret Access Key**.
     - **Warning**: You cannot retrieve the Secret Access Key later. Store the `.csv` securely.

5. **Configure AWS CLI**:
   - Run:
     ```bash
     aws configure
     ```
   - Enter:
     - **Access Key ID**: From the `.csv` file.
     - **Secret Access Key**: From the `.csv` file.
     - **Default region name**: Your ECR repository’s region (e.g., `us-west-2`).
       - Find it in the AWS Console’s top-right Region dropdown or ECR console.
       - Use the code (e.g., `us-west-2`), not the name (e.g., “Oregon”).
     - **Default output format**: Press Enter (defaults to JSON).
   - **Verification**:
     - In the ECR console, select your repository and click **View push commands**.
     - Copy the first command (e.g., `aws ecr get-login-password --region YOUR-REGION | docker login ...`).
     - Run it in your terminal.
     - If you see an error like `command not found: aws`, recheck CLI installation.
     - If you see an authentication error (e.g., `IncompleteSignatureException`), ensure the Access Key ID and Secret Access Key are correct. Seek community help if needed.

6. **Best Practices**:
   - Use a dedicated IAM user for CLI access with minimal permissions.
   - Securely store access keys and delete them after the project.
   - Ensure Player A configures their AWS CLI.

## Step 7: Push Your Docker Image to ECR
Upload your Docker image to your ECR repository for Player A to access.

1. **Authenticate Docker to ECR**:
   - In the ECR console, select your repository and click **View push commands**.
   - Ensure the correct OS is selected (macOS/Linux or Windows).
   - Copy the first command, e.g.:
     ```bash
     aws ecr get-login-password --region YOUR-REGION | docker login --username AWS --password-stdin YOUR-ACCOUNT-ID.dkr.ecr.YOUR-REGION.amazonaws.com
     ```
   - Run it in your terminal.
     - **Purpose**: Authenticates Docker with your ECR registry using a temporary password.
   - Expect output like `Login Succeeded`.

2. **Tag the Image**:
   - Copy the third push command, e.g.:
     ```bash
     docker tag cross-account-docker-app:latest YOUR-ACCOUNT-ID.dkr.ecr.YOUR-REGION.amazonaws.com/cross-account-docker-app:latest
     ```
   - Run it to tag your image with the ECR repository URI.
     - **Purpose**: Tags link your local image to the ECR repository for pushing.
   - **Note**: You already built the image in Step 3, so skip the second push command (rebuild).

3. **Push the Image**:
   - Copy the fourth push command, e.g.:
     ```bash
     docker push YOUR-ACCOUNT-ID.dkr.ecr.YOUR-REGION.amazonaws.com/cross-account-docker-app:latest
     ```
   - Run it to upload your image to ECR.
   - Expect output showing upload progress, ending with a success message.
   - **Verification**:
     - In the ECR console, refresh the repository page.
     - Confirm your image appears with the `latest` tag.

4. **Common Issues**:
   - **No Such Image**: If the third command fails, run the second push command to rebuild, then retry the third and fourth.
   - **Authentication Errors**: Ensure AWS CLI is configured correctly (re-run `aws configure` if needed).
   - **Network Issues**: Check your internet connection.
   - Seek community help for persistent errors.

5. **Best Practices**:
   - Verify each push command before running to avoid typos.
   - Use the `latest` tag for simplicity, but consider versioned tags (e.g., `v1.0`) in production.
   - Ensure Player A pushes their image to their ECR repository.

## Step 8: Pull and Run Your Partner’s Container Image
Pull Player A’s Docker image and run it locally to view their web app.

1. **Authenticate to Player A’s ECR**:
   - Ask Player A for their first push command (from their ECR repository’s **View push commands**).
   - Run it in your terminal to authenticate with their ECR.
   - Expect `Login Succeeded`.
     - **Note**: This authenticates your AWS credentials but doesn’t guarantee access to their repository (permissions are set in Step 9).

2. **Pull Player A’s Image**:
   - Ask Player A for their image’s URI:
     - In their ECR console, they should select their repository, find the `latest` image, and click **Copy URI**.
   - Run:
     ```bash
     docker pull [player-a-uri]
     ```
     - Replace `[player-a-uri]` with their URI (e.g., `123456789012.dkr.ecr.us-west-2.amazonaws.com/cross-account-docker-app:latest`).
   - If you get a `403 Forbidden` error, proceed to [Step 9](#step-9-resolve-permission-issues).

3. **Run the Container**:
   - Run:
     ```bash
     docker run -d -p 80:80 [player-a-uri]
     ```
     - **Explanation**:
       - `-d`: Runs the container in detached mode (background).
       - `-p 80:80`: Maps port 80 on your machine to port 80 in the container (Nginx’s default port).
   - If port 80 is in use, try:
     ```bash
     docker run -d -p 8080:80 [player-a-uri]
     ```
   - Open your browser and navigate to:
     - `localhost` (if using port 80).
     - `localhost:8080` (if using port 8080).
   - You should see Player A’s web app, including their secret code!

4. **Common Issues**:
   - **Port Conflict**: If port 80 is taken, try `8080:80`, `8081:80`, etc.
   - **Blank Page**: Ensure you’re using the correct port in your browser (e.g., `localhost:8080`).
   - Seek community help if the app doesn’t load.

5. **Best Practices**:
   - Verify the image URI with Player A to avoid typos.
   - Test locally before proceeding to cloud deployment.
   - Share your image URI with Player A so they can pull your image.

## Step 9: Resolve Permission Issues
If you received a `403 Forbidden` error when pulling Player A’s image, their ECR repository lacks permissions for your AWS account.

1. **Understand ECR Permissions**:
   - ECR repositories are private by default, requiring explicit permissions to allow access.
   - You’ll grant Player A’s `ECR-Access` IAM user permission to pull from your repository, and they’ll do the same for you.

2. **Update Your ECR Repository Policy**:
   - In the AWS ECR console, select your `cross-account-docker-app` repository.
   - Click **Actions** > **Permissions** > **Edit policy JSON**.
   - Add the following policy:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "AWS": "[player-a-ARN]"
           },
           "Action": [
             "ecr:BatchCheckLayerAvailability",
             "ecr:BatchGetImage",
             "ecr:GetDownloadUrlForLayer"
           ]
         }
       ]
     }
     ```
   - **Customization**:
     - Replace `[player-a-ARN]` with Player A’s `ECR-Access` user ARN.
     - To find your ARN for Player A:
       - Go to IAM > Users > `ECR-Access`.
       - Copy the ARN from the Summary panel (e.g., `arn:aws:iam::123456789012:user/ECR-Access`).
       - Share your ARN with Player A.
   - Click **Save**.

3. **Retry Pulling and Running**:
   - Ask Player A to update their repository policy with your `ECR-Access` ARN.
   - Re-run:
     ```bash
     docker pull [player-a-uri]
     ```
   - If successful, run:
     ```bash
     docker run -d -p 80:80 [player-a-uri]
     ```
   - Open `localhost` (or `localhost:8080`) in your browser.
   - **Architecture Error**:
     - If you see `no matching manifest for linux/amd64` (e.g., due to Apple M1 vs. Intel), rebuild your image for multiple architectures:
       ```bash
       docker buildx create --use
       docker buildx build --platform linux/amd64,linux/arm64 -t [your-image-URI] --push .
       ```
       - Replace `[your-image-URI]` with your ECR image URI (e.g., `123456789012.dkr.ecr.us-west-2.amazonaws.com/cross-account-docker-app:latest`).
     - Ask Player A to do the same, then retry pulling.
     - **Why?**: Different CPU architectures (e.g., `amd64` for Intel, `arm64` for Apple Silicon) require compatible images. `buildx` creates multi-architecture images.

4. **Common Issues**:
   - **Incorrect ARN**: Verify the ARN with Player A.
   - **Policy Errors**: Ensure the JSON is valid and actions are correct.
   - Seek community help if errors persist.

5. **Best Practices**:
   - Grant minimal permissions (only the listed ECR actions).
   - Double-check ARNs to avoid access issues.
   - Test permissions immediately after updating policies.

## Step 10: Deploy Your Partner’s App on Elastic Beanstalk
Deploy Player A’s Docker image to AWS Elastic Beanstalk, a service for hosting and scaling web applications.

1. **Understand Elastic Beanstalk**:
   - Elastic Beanstalk automates deployment, scaling, and monitoring, abstracting infrastructure management.
   - You’ll deploy Player A’s container image as a web app accessible via a public URL.

2. **Create an Elastic Beanstalk Application**:
   - In the AWS Console, go to **Elastic Beanstalk** > **Create application**.
   - Set **Application name** (e.g., `cross-account-app`).
   - Choose **Platform**: **Docker**.
   - For **Application code**, select **Upload your code**.
   - Create a file named `Dockerrun.aws.json` in your `DockerECR` directory:
     ```json
     {
       "AWSEBDockerrunVersion": "1",
       "Image": {
         "Name": "[player-a-uri]",
         "Update": "true"
       },
       "Ports": [
         {
           "ContainerPort": 80,
           "HostPort": 80
         }
       ]
     }
     ```
     - Replace `[player-a-uri]` with Player A’s image URI.
     - **Purpose**: Instructs Elastic Beanstalk to pull and run Player A’s Docker image on port 80.
   - Save the file.
   - In the AWS Console, click **Choose file** and upload `Dockerrun.aws.json`.
   - Click **Create application**.

3. **Monitor and Verify Deployment**:
   - Wait for the environment to turn green (healthy), which may take 5–10 minutes.
   - In the Elastic Beanstalk console, find the environment URL (e.g., `http://cross-account-app-env.eba-xxxx.us-west-2.elasticbeanstalk.com`).
   - Open the URL in your browser to view Player A’s app.
   - **Verification**:
     - Confirm the app displays Player A’s customized content, including their secret code.
     - Take a screenshot for your project documentation.

4. **Common Issues**:
   - **Environment Fails**: Check the Elastic Beanstalk logs (Events tab) for errors (e.g., incorrect image URI).
   - **Permission Errors**: Ensure Player A’s ECR policy allows your AWS account to pull the image.
   - Seek community help if the app doesn’t deploy.

5. **Best Practices**:
   - Validate `Dockerrun.aws.json` for correct syntax before uploading.
   - Monitor deployment progress to catch issues early.
   - Share the deployed URL with Player A to celebrate their app going live!

## Step 11: Clean Up Resources
Delete all AWS and Docker resources to avoid charges.

1. **Why Clean Up?**:
   - AWS charges for active resources (e.g., ECR repositories, Elastic Beanstalk environments).
   - Deleting resources ensures your account remains cost-free.

2. **Delete ECR Repository**:
   - In the ECR console, select `cross-account-docker-app`.
   - Click **Delete** and confirm.

3. **Delete IAM User**:
   - In the IAM console, go to **Users**.
   - Select `ECR-Access` > **Delete**.
   - Enter `ECR-Access` to confirm.
   - On your computer, delete `ECR-Access_accessKeys.csv` from your Downloads folder.

4. **Delete Docker Resources**:
   - Open Docker Desktop.
   - **Containers**:
     - Go to the **Containers** tab.
     - Select all containers, click **Delete**, and choose **Delete forever**.
   - **Images**:
     - Go to the **Images** tab.
     - Delete `cross-account-docker-app`.
   - **Builds**:
     - Go to the **Builds** tab.
     - Delete any builds related to this project.

5. **Delete Elastic Beanstalk Resources**:
   - In the Elastic Beanstalk console, select your application (e.g., `cross-account-app`).
   - Click **Actions** > **Delete application**.
  
     
## Best Practices:
   - Showcase specific skills (e.g., Docker, AWS, IAM) to impress potential employers.
   - Credit Player A for collaboration.
   - Keep your documentation professional and concise.

## Troubleshooting Common Issues
If you encounter problems, try these solutions:

1. **Docker Issues**:
   - **Command Not Found**: Ensure Docker Desktop is running and added to your PATH.
   - **Build Errors**: Verify `Dockerfile` and `index.html` are in the correct directory with no typos.
   - **Daemon Errors**: Restart Docker Desktop or your computer.

2. **AWS CLI Issues**:
   - **Command Not Found**: Reinstall AWS CLI and verify with `aws --version`.
   - **Authentication Errors**: Re-run `aws configure` with correct Access Key ID and Secret Access Key.
   - **Region Errors**: Ensure the region matches your ECR repository (e.g., `us-west-2`).

3. **ECR Issues**:
   - **Push Failures**: Check AWS CLI configuration and Docker authentication.
   - **403 Forbidden**: Update repository policies with correct ARNs (Step 9).

4. **Elastic Beanstalk Issues**:
   - **Deployment Fails**: Validate `Dockerrun.aws.json` and ensure Player A’s image is accessible.
   - **Timeout Errors**: Check logs in the Elastic Beanstalk console and extend timeout settings if needed.


## Conclusion
Congratulations on completing the Docker Cross-Account Deployment Project! You and Player A have demonstrated teamwork and technical prowess by:
- Building a custom Docker image with a personalized web app.
- Storing and sharing images securely in Amazon ECR.
- Configuring IAM policies for cross-account access.
- Running and deploying your partner’s app locally and on Elastic Beanstalk.
- Cleaning up resources to manage costs.

These skills—containerization, cloud storage, secure access management, and application deployment—are foundational for DevOps and cloud engineering roles. Share your success on LinkedIn and in your community to showcase your achievements and connect with others. Keep exploring Docker and AWS, and consider advanced projects like CI/CD pipelines or Kubernetes!

