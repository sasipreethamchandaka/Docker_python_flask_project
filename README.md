# 🚀 Python Flask Application Deployment Using Docker

### Goal

Run a **Python Flask application inside a Docker container** with full automation.

---
## Create base image 
```
yum install docker -y
systemctl start docker
docker pull ubuntu
docker images
docker run -dt python

```

### Step 1: Prepare the Project Folder
git clone https://github.com/sasipreethamchandaka/Docker_python_flask_project.git
- Correct project structure is **mandatory**:

```
docker_python_flask-project/
├── Dockerfile
├── app.py
├── requirements.txt
├── README.md
```
# vi app.py
```
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def hello():
    return "updated Flask sample application on azure hghapp service updated verrsion-4"


if __name__ == "__main__":
    port = int(os.environ.get("PORT", 5000))
    app.run(debug=True,host='0.0.0.0',port=port)
```

# vi requirements.txt
```
flask
```


> ⚠️ Common mistake: Dockerfile or requirements.txt placed outside the project folder, which causes files not to be copied into the Docker image.

---

### Step 2: Test Flask App Locally (Without Docker)

```bash
pip3 install -r requirements.txt
python3 app.py
```

Expected output:

```
Running on http://0.0.0.0:5000
```

This confirms:

* app.py is correct
* requirements.txt is correct
* Flask app works properly

Stop the server using **CTRL + C**.

---

### Step 3: Create Dockerfile (Automation Step) (instrution)

The Dockerfile is responsible for:

* Installing Python
* Installing dependencies
* Running the application automatically

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

---

### Step 4: Build Docker Image  (custom image)

```bash
cd docker_python_flask-project
docker build -t flask-app .
```

✅ The image now contains:

* Python runtime
* Flask and dependencies
* Application code

---

### Step 5: Run Container

```bash
docker run -d -p 5000:5000 flask-app
```

Verify container:

```bash
docker ps
```

Open in browser:

```
http://<EC2_PUBLIC_IP>:5000
```

---

### Step 6: Common Practice Mistakes (And Fixes)

❌ `docker run python python app.py`

* app.py does not exist inside the image by default

❌ Using `-d` without port mapping

* App runs in background but is not accessible

❌ Running `pip install -r requirements.txt` in wrong directory

* File not found error occurs

---

### Final Understanding

* **Dockerfile provides full automation**
* Image = blueprint
* Container = running application
* Flask apps should always be run via Dockerfile, not manual docker run commands

✅ Python Flask application successfully running inside a Docker container
