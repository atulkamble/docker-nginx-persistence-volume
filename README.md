## 📦 Project: Simple Web App with Persistent Data using Docker Volumes

**Use Case:**
Run an Nginx container that serves content from a volume so that data persists even if the container is removed.

---

## 📂 Project Structure:

```
docker-volume-project/
├── data/
│   └── index.html
├── docker-compose.yml
└── Dockerfile
```

---

## 📜 index.html (in `data/` folder)

```html
<!DOCTYPE html>
<html>
<head>
  <title>Docker Volume Test</title>
</head>
<body>
  <h1>Hello from Docker Volume!</h1>
  <p>This page is served using a Docker Volume.</p>
</body>
</html>
```

---

## 🐳 Dockerfile

(Here we use the official Nginx image, no need for custom build in this case, but for practice let's create one.)

```dockerfile
# Use official Nginx image as base
FROM nginx:latest

# Expose port 80
EXPOSE 80
```

---

## 🐳 docker-compose.yml

(Defines service, volume, and volume binding)

```yaml
version: '3.8'

services:
  webserver:
    build: .
    ports:
      - "8080:80"
    volumes:
      - web_data:/usr/share/nginx/html:ro
    container_name: nginx_volume_test

volumes:
  web_data:
    driver: local
```

---

## 📦 Load initial content into the volume

After defining the Docker Compose setup, you need to copy the `index.html` file into the volume. Since volumes are managed by Docker, one way is:

### Option 1: Copy data to volume manually

Run container:

```bash
docker compose up -d --build
```

Find volume name:

```bash
docker volume ls
```

Find mount path:

```bash
docker volume inspect docker-nginx-persistence-volume_web_data
```

Copy file:

```bash
docker compose up -d --build
docker cp ./data/index.html nginx_volume_test:/usr/share/nginx/html/index.html
```

---

## ✅ Test the setup

Visit: [http://localhost:8080](http://localhost:8080)
You should see your `index.html` page.

---

## 📌 Verify persistence

Stop & remove the container:

```bash
docker-compose down
```

Check if the volume still exists:

```bash
docker volume ls
```

You’ll notice `dockervolumeproject_web_data` still exists — confirming data persists independently of the container.

---

## 📦 Repo Name Suggestion:

`docker-volume-nginx-project`

---

## 🎁 Bonus: Clean up volumes if needed

```bash
docker volume rm dockervolumeproject_web_data
```

---

## 📓 Summary:

✅ Created a volume
✅ Mapped it to Nginx’s content directory
✅ Verified content persists even after container removal
