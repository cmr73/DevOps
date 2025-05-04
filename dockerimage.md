# 🐳 Deploying a Node.js App on EC2 and Pushing Docker Image to Docker Hub

---

## 🔹 A) Create an EC2 Instance

* Allow **HTTP traffic** during instance creation.
* Use **Git Bash** or **MobaXterm** to SSH into the EC2 instance.

---

## 🔹 B) Install Prerequisites

### Switch to root user:

```bash
sudo su -
apt update -y
```

### Install required packages:

```bash
apt install nginx -y         # Nginx
apt install nodejs -y        # Node.js
apt install npm -y           # NPM
npm install -g pm2           # PM2 process manager
```

---

## 🔹 C) Create a Node.js Application

### Create and open the file:

```bash
cd /home
mkdir node
cd node
nano hello.js
```

### Paste the following code:

```js
const http = require('http');

const hostname = '0.0.0.0';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### Start the app:

```bash
node hello.js
```

### Run the app with PM2:

```bash
pm2 start hello.js --name app
```

---

## 🔹 D) Configure Nginx as Reverse Proxy

### Create config file:

```bash
nano /etc/nginx/sites-available/example.com
```

### Add the following:

```nginx
server {
    listen 80;
    server_name YOUR_EC2_PUBLIC_IP;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Enable the site and restart Nginx:

```bash
ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
systemctl restart nginx
```

### 🧪 Open your EC2 IP in browser — you should see “Hello World!”

---

## 🔹 E) Dockerize the Application

### Install Docker and Docker Compose:

```bash
apt install -y docker.io
apt install -y docker-compose
```

### Create Dockerfile:

```bash
cd /home/node
nano Dockerfile
```

#### Paste the following:

```Dockerfile
FROM node:12
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "hello.js"]
```

### Create `.dockerignore`:

```bash
nano .dockerignore
```

#### Paste:

```
node_modules
npm-debug.log
```

---

## 🔹 F) Create a Docker Hub Account

1. Go to [https://hub.docker.com/](https://hub.docker.com/)
2. Sign up with your email and create a username and password.
3. You’ll use this username for tagging and pushing your image.

---

## 🔹 G) Build and Push Docker Image

### Build the Docker image:

```bash
docker build -t your_dockerhub_username/node-app:latest .
```

### Log in to Docker Hub:

```bash
docker login
```

> Enter your Docker Hub **username** and **password** when prompted.

### Push image to Docker Hub:

```bash
docker push your_dockerhub_username/node-app:latest
```

### ✅ Go to [Docker Hub](https://hub.docker.com/) and confirm your image is uploaded under your repositories.
### ✅Done
