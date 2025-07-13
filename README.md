# E-commerce-with-Github-actions

## Project Setup

* Create a git repository naming it E-commerce-platform

![image](https://github.com/user-attachments/assets/009540d1-85e3-4395-9e48-f52775779b05)

* Inside the directory create web app and api folder
  ![image](https://github.com/user-attachments/assets/fbcd6209-56ab-4e1f-b6a1-6e0b26437788)

  
  ## Initialize Github actions

  * Initialize the git repository and add your code structure
    
   ![image](https://github.com/user-attachments/assets/7fe5f491-273e-454d-89d2-8df17716db4f)

  * Create your workflows folder
    ![image](https://github.com/user-attachments/assets/b80b18e0-5c01-4049-a8bf-202acae9f9a0)

  ## Backend api setup

  * Set up a simple node js express for e commerce operations

    ![image](https://github.com/user-attachments/assets/964e6b77-beee-4bcf-860a-92d695297f5d)

  * Initialize node js application
 
    ![image](https://github.com/user-attachments/assets/567c12b9-d8fa-4658-aec5-aacd0dd8da99)

  * Install express
 
    ![image](https://github.com/user-attachments/assets/04601081-aa9c-409a-bf45-3f033e421a6d)

  * Implement unit tests for the api
 
    ![image](https://github.com/user-attachments/assets/166cae3b-342c-4c3a-baad-7e86814d0c56)

 
    ![image](https://github.com/user-attachments/assets/0a30b169-d788-419c-9c86-be3ea5501afd)

  ## Front end web application setup

  * Create simple web application
 
  * npx create-react-app .
  *  npm install axios
    ![image](https://github.com/user-attachments/assets/a2b64934-3794-4b4a-b49b-554b88d54111)

* Both folders have been setup
  ![image](https://github.com/user-attachments/assets/687e2153-0547-43b4-bf24-69aa57f509ec)


## CI Workflow

- Workflow that installs dependencies, runs tests and builds the application

       ```yaml
       name: CI/CD Pipeline

         on:
       push:
         branches: [main]

         jobs:
          build-and-test:
          runs-on: ubuntu-latest
          steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      # Backend
      - name: Install backend dependencies
        working-directory: ./server
        run: npm install

      - name: Run backend tests
        working-directory: ./server
        run: npm test

      # Frontend
      - name: Install frontend dependencies
        working-directory: ./public
        run: npm install

      - name: Build frontend
        working-directory: ./public
        run: npm run build

          ```
          ---

  ## Docker integration

* Create dockerfiles for the frontend and backend folders
  <img width="986" height="482" alt="image" src="https://github.com/user-attachments/assets/d548ab54-9682-40ae-b01a-e565947cc7c0" />

  <img width="1136" height="673" alt="image" src="https://github.com/user-attachments/assets/a5289b8e-a09a-4e91-9965-2b6c7a339326" />

* Modify your workflow to build docker the images

  

      # Dockerisation
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Backend image
        uses: docker/build-push-action@v4
        with:
          context: .
          file: ./server/Dockerfile
          push: true
          tags: chykmanish/ecommerce-app-backend:latest

      - name: Build and push Frontend image
        uses: docker/build-push-action@v4
        with:
          context: ./public
          file: ./public/Dockerfile
          push: true
          tags: chykmanish/ecommerce-app-frontend:latest

  ## Deploy to the Cloud
  * Choose a Cloud platform
    <img width="1920" height="852" alt="image" src="https://github.com/user-attachments/assets/adb3be62-1063-4243-9456-8987c5542b5d" />
 ---

        Deployment
      - name: Deploy Containers
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.EC2_IP_ADDRESS }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            set -ex
            echo "=== Starting Deployment ==="
            cd /home/ubuntu
            
            # Stop and clean up old containers
            docker-compose down || true
            docker system prune -af  # Cleanup unused images
            docker pull chykmanish/ecommerce-app-backend:latest
            docker pull chykmanish/ecommerce-app-frontend:latest
            docker-compose up -d --force-recreate
            echo "=== Running Containers ==="
            docker ps -a
            echo "=== Network Status ==="
            docker network ls
---

* Ensure all required credentials are stored github secrets

  <img width="1920" height="941" alt="image" src="https://github.com/user-attachments/assets/51d4775a-87ac-4cd1-ae76-db6fb6f7bcbd" />

  ## Configure workflow to deploy updates when codes are pushed to main

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/60c21d2e-556a-4f7a-9132-8d60c387f4a2" />

  <img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/64bee24a-bb7f-4eb4-b788-1a3b99927e48" />

  ## Implementing caching
  * It was implemented after the steps for installing the modules so that it can be cached
    
  <img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/c4fc6332-645a-49f6-81c7-74021d9bcefd" />

  <img width="1920" height="827" alt="image" src="https://github.com/user-attachments/assets/2f82df57-eb06-4993-98fe-078325622eb8" />


















