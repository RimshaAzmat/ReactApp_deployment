# React App Deployment Guide

This guide provides instructions on how to deploy a React application to an AWS EC2 instance using Nginx. It covers cloning the app from GitHub, setting up the environment on the EC2 instance, and configuring Nginx to serve the application. Yarn is used for managing dependencies and building the project.

## Prerequisites

1. **AWS EC2 Instance**: You should have an EC2 instance running Ubuntu.
2. **GitHub Repository**: Your React app should be hosted on GitHub.
3. **Nginx**: Used to serve the React app.
4. **Yarn**: A package manager for Node.js.

## Step 1: Connect to Your EC2 Instance
Through putty or terminal
## Step 2: Install Node.js, Yarn, and Nginx
sudo apt update

sudo apt install -y nodejs npm nginx

npm install -g yarn
## Step 3: Clone Your React App from GitHub
git clone https://github.com/your-username/your-react-app.git

cd your-react-app
## Step 4: Install Dependencies and Build the React App
yarn install

yarn build

The yarn build command creates a build directory with a production-ready version of your React app.
## Step 5: Configure Nginx
Create an Nginx configuration file for your React app:

sudo nano /etc/nginx/sites-available/react-app

Add the following configuration:


server {
    listen 80;
    server_name your-ec2-public-ip;  # Change to your domain if applicable

    location / {
        root /home/ubuntu/your-react-app/build;
        try_files $uri /index.html;
    }

    # Serve static assets directly
    location /static/ {
        alias /home/ubuntu/your-react-app/build/static/;
        expires 30d;  # Cache static assets for 30 days
        add_header Cache-Control "public, must-revalidate";
    }

    # Optional: Serve favicon.ico if present
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    # Logging
    access_log /var/log/nginx/react_app_access.log;
    error_log /var/log/nginx/react_app_error.log;
}

Create a symbolic link to enable this configuration:

sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/

## Step 6: Test and Restart Nginx
Test the Nginx configuration to ensure there are no syntax errors:

sudo nginx -t

If the test is successful, restart Nginx to apply the changes:

sudo systemctl restart nginx
## Step 7: Verify the Deployment
Visit your EC2 instance’s public IP address in a web browser. You should see your React app running!

## Conclusion
Congratulations! You've successfully deployed your React app to an EC2 instance using Nginx. This setup ensures that your application is served efficiently and can handle production traffic. Feel free to customize and expand upon this configuration to fit your needs.

For further customization and troubleshooting, refer to the Nginx documentation and React documentation.
