# 🧪 Selenium Grid Deployment on AWS EC2 (Chrome & Firefox)

## 🚀 Step 1: Launch AWS EC2 Instance

1. Go to [AWS EC2 Console](https://console.aws.amazon.com/ec2).
2. Click **Launch Instance** and add the inbound security rule:
     - `Custom TCP` — Port 4444 — Anywhere (for Grid UI)
---

## 🔗 Step 2: Connect to EC2 via any CLI MobaXtreme or GitBash

- Use your `.pem` key file and connect using MobaXterm or any SSH client.

---

## 🐳 Step 3: Install Docker and Docker Compose

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker

# Install Docker Compose (standalone)
sudo curl -L "https://github.com/docker/compose/releases/download/v2.17.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Check versions
docker --version
docker-compose --version
````
---

## ⚙️ Step 4: Create Selenium Grid with Docker Compose

```bash
mkdir selenium-grid && cd selenium-grid
nano docker-compose.yml
```

Paste the following content:

```yaml
version: "3"
services:
  selenium-hub:
    image: selenium/hub:4.0.0-rc-2-20210930
    container_name: seleniumHub
    ports:
      - "4444:4444"

  chrome:
    image: selenium/node-chrome:4.0.0-rc-2-20210930
    container_name: chromeNode
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
    shm_size: 2g

  firefox:
    image: selenium/node-firefox:4.0.0-rc-2-20210930
    container_name: firefoxNode
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
    shm_size: 2g
```

Save and exit (`Ctrl + O`, `Enter`, `Ctrl + X`).

---

## ▶️ Step 5: Start Selenium Grid

```bash
sudo docker-compose up -d
sudo docker ps  # To verify running containers
```

---

## 🌐 Step 6: Access Grid UI

Open your browser and go to:

```
http://<Your-EC2-Public-IP>:4444/ui
```

---

## 🧪 Step 7: Run a Sample Python Test

### Install Python & Selenium

```bash
sudo apt install python3-venv python3-full -y
python3 -m venv venv
source venv/bin/activate
pip install selenium
```

### Create a test script

```bash
nano test_grid.py
```

Paste the following code:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

browser = "chrome"  # Change to "firefox" for Firefox testing
GRID_URL = "http://localhost:4444/wd/hub"

options = None
if browser == "chrome":
    options = webdriver.ChromeOptions()
elif browser == "firefox":
    options = webdriver.FirefoxOptions()
else:
    raise Exception("Unsupported browser!")

driver = webdriver.Remote(
    command_executor=GRID_URL,
    options=options
)

driver.get("https://www.google.com")
print("Title:", driver.title)
driver.quit()
```

Run the script:

```bash
python3 test_grid.py
```

---

## ✅ Done!
