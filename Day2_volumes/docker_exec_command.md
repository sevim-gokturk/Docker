# Docker Exec Command Guide

## 1️⃣ Open Bash or Shell Inside a Running Container

To open an interactive shell inside a running container:

- **Open Bash (if installed):**
  ```sh
  docker exec -it my_container bash
  ```
- **For lightweight Linux distributions (like Alpine), use `sh` or `ash`:**
  ```sh
  docker exec -it my_container sh
  ```
  or
  ```sh
  docker exec -it my_container ash
  ```

---

## 2️⃣ Execute a Command Inside a Running Container

To run a specific command inside a container without opening an interactive shell:

- **List files in `/app` directory:**
  ```sh
  docker exec my_container ls -l /app
  ```
- **Check environment variables:**
  ```sh
  docker exec my_container env
  ```
- **Check if Python is installed:**
  ```sh
  docker exec my_container python --version
  ```

---

## 3️⃣ Run a Command as a Different User

You can specify a user inside the container when executing a command:

- **Run Bash as the `root` user:**
  ```sh
  docker exec -u root -it my_container bash
  ```

This is useful when a process is running with restricted permissions.

---

## 4️⃣ Access Running Databases or Services

If a database or another service is running inside a container, you can interact with it using `docker exec`:

- **Access PostgreSQL inside a container:**
  ```sh
  docker exec -it postgres_container psql -U postgres
  ```
- **Access MySQL inside a container:**
  ```sh
  docker exec -it mysql_container mysql -u root -p
  ```

---

## 🔥 Key Differences: `docker exec` vs `docker attach`

| Feature | `docker exec` | `docker attach` |
|---------|--------------|----------------|
| Enter a running container | ✅ | ✅ |
| Start a new process inside the container | ✅ | ❌ |
| Exit without stopping the container | ✅ | ❌ (Exiting may stop the container) |
| Detach from the shell without stopping | ✅ (`exit` or `Ctrl + D`) | ❌ (Use `Ctrl + P + Q`) |

### 🛑 **Important:** When using `docker attach`, press `Ctrl + P + Q` to detach without stopping the container.

---

### 📌 Useful Additional Commands
- **View container logs:**
  ```sh
  docker logs my_container
  ```
- **Check the container's filesystem:**
  ```sh
  docker exec my_container ls /var/www
  ```


