
mkdir dockerkodkod
cd dockerkodkod

# 1. main.py
cat <<EOF > main.py
from fastapi import FastAPI
import httpx

app = FastAPI()

@app.get("/ip/{ip_address}")
async def get_ip_info(ip_address: str):
    async with httpx.AsyncClient() as client:
        response = await client.get(f"https://ipinfo.io/{ip_address}/json")
        return response.json()
EOF

# 2. requirements.txt
cat <<EOF > requirements.txt
fastapi
uvicorn
httpx
EOF

# 3. Dockerfile
cat <<EOF > Dockerfile
# Use official Python image
FROM python:3.11-slim

# Set the working directory
WORKDIR /app

# Copy dependencies file
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the app
COPY . .

# Expose the port the app runs on
EXPOSE 8000

# Start the FastAPI app
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
EOF

# 4. README.md
cat <<EOF > README.md
# IP Info Lookup - FastAPI App (Dockerized)

This project is a containerized **FastAPI** application that allows users to look up IP address information using the [ipinfo.io](https://ipinfo.io) API.

---

## 🗂️ Project Structure

\`\`\`
.
├── main.py              # FastAPI application
├── requirements.txt     # Python dependencies
├── Dockerfile           # Docker build instructions
└── README.md            # Project documentation
\`\`\`

---

## 🚀 How to Run This Project (Step-by-Step)

### 1. 🔍 Clone the Repository

\`\`\`bash
git clone https://github.com/omor13/dockerkodkod.git
cd dockerkodkod
\`\`\`

---

### 2. 📦 Build the Docker Image

\`\`\`bash
docker build -t fastapi-ipinfo-app .
\`\`\`

---

### 3. ▶️ Run the Container

\`\`\`bash
docker run -d -p 8000:8000 fastapi-ipinfo-app
\`\`\`

Now the app is running at: [http://localhost:8000](http://localhost:8000)

---

### 4. 🔬 Test the API

#### Docs UI:
Open your browser at:
\`\`\`
http://localhost:8000/docs
\`\`\`

#### Example request:
\`\`\`
GET /ip/{ip_address}
\`\`\`

---

## ✅ Challenge Objectives Completed

- ✅ Create a proper Dockerfile for FastAPI
- ✅ Build the Docker image
- ✅ Run and test the application
- ✅ Confirm accessibility and functionality
EOF
# dockerkodkod
