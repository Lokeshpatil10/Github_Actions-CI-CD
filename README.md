# GitHub Actions CI/CD with Next.js on AWS EC2

This project is a Next.js application configured for deployment to an AWS EC2 instance using GitHub Actions and PM2.

## Project Overview

- Framework: Next.js
- Language: TypeScript
- Styling: Tailwind CSS
- Deployment target: AWS EC2
- CI/CD: GitHub Actions
- Process manager: PM2

## Features

- Modern Next.js app with a polished landing page
- Production build support
- Automated deployment from GitHub to EC2
- Simple PM2-based process management

## Local Development

Install dependencies:

```bash
npm install
```

Run the development server:

```bash
npm run dev
```

Open http://localhost:3000 in your browser.

## Production Build

Build the app:

```bash
npm run build
```

Start the app locally:

```bash
npm start
```

## AWS EC2 Deployment Setup

### 1. Launch an EC2 instance

Create an Ubuntu or Amazon Linux EC2 instance and make sure these ports are open:

- 22 for SSH
- 80 for HTTP
- 3000 for the Next.js app (optional if using a reverse proxy)

### 2. Install required packages on the server

```bash
sudo apt update
sudo apt install -y git curl
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2
```

### 3. Clone the repository on EC2

```bash
cd /home/ubuntu
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
npm install
npm run build
```

### 4. Start the app with PM2

```bash
pm2 start npm --name "100devs-ci-cd" -- start
pm2 save
pm2 startup systemd
```

## GitHub Actions Deployment

This repository includes a GitHub Actions workflow in `.github/workflows/test.yml` that deploys the app to EC2 over SSH.

### Required GitHub Secret

Create a secret named:

```text
PRIVATE_SSH_KEY
```

This should contain the private SSH key for your EC2 instance.

## Deployment Script

The deployment script is located at `deploy.sh`.

It performs the following tasks:

- pulls the latest code from GitHub
- installs dependencies
- builds the app
- restarts the app using PM2

## Notes

- Make sure the repository folder name on EC2 matches the path used in `deploy.sh`.
- If you want to expose the app on port 80, consider using Nginx as a reverse proxy to `localhost:3000`.

## Project Structure

```text
.github/workflows/     GitHub Actions deployment workflow
public/                 Static assets
src/                    Application source code
deploy.sh               Deployment script for EC2
package.json            Project dependencies and scripts
```

## License

This project is for learning and demonstration purposes.
   