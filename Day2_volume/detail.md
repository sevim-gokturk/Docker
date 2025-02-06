## **📌 Explanation of the Command**  
```sh
 docker run -it -v my_volume:/data --name persistent_container alpine sh
```

🔹 **1️⃣ Volume is Created (On the Host)**  
- If the volume `my_volume` does not already exist, Docker **automatically creates it**.  
- Volumes are stored on the host in **`/var/lib/docker/volumes/`**.  
- This ensures that **data persists even if the container is removed**.  

🔹 **2️⃣ Container is Created and Runs**  
- A new container named **`persistent_container`** is created using the **Alpine** image.  
- The shell (`sh`) is started inside the container.  

🔹 **3️⃣ Volume is Mounted to `/data` Inside the Container**  
- The `-v my_volume:/data` option **mounts the volume inside the container at `/data`**.  
- Any files written to `/data` in the container are **stored in the volume on the host**.  
- **Even if the container is deleted, data remains in the volume.**  

---

## **📌 General Formula**  
```sh
 docker run -it -v <volume_name>:<container_path> --name <container_name> <image_name> <command>
```
📌 **Explanation of Parameters:**  
- `<volume_name>` → The **name of the volume** to be used  
- `<container_path>` → The **mount point inside the container**  
- `<container_name>` → The **name of the container**  
- `<image_name>` → The **Docker image** to be used  
- `<command>` → The **command executed when the container starts**  

---

## **🚀 Summary**  
✅ **A volume is created and stored on the host**  
✅ **Data persists even if the container is deleted**  
✅ **Multiple containers can share the same volume**  
✅ **Ideal for persistent storage and data sharing**  

