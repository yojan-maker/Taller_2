# 🧠 Proyecto: Simulación de Robots en Docker con PyBullet

Este proyecto muestra el proceso completo de **dockerización e implementación de simulaciones robóticas** usando **PyBullet**.  
Se desarrollaron tres entornos independientes para los robots:

- 🛸 **Drones (gym-pybullet-drones)**
- 🤖 **Baxter**
- 🦿 **Atlas**

Cada uno fue encapsulado en su propio contenedor Docker con soporte gráfico (X11) para visualizar las simulaciones.

---

## 📦 1️⃣ Simulación de Drones – `gym-pybullet-drones`

Repositorio base: https://github.com/utiasDSL/gym-pybullet-drones

### 🧩 Paso 1: Crear y configurar el Dockerfile

En este paso se crea un `Dockerfile` para instalar las dependencias, clonar el repositorio y preparar el entorno.  
El Dockerfile incluye:
- Instalación de **Python, pip y PyBullet**
- Instalación de librerías gráficas (para OpenGL y X11)
- Clonación del repositorio `gym-pybullet-drones`
  
![Image](https://github.com/user-attachments/assets/8626fcf8-4c67-4ae8-a1f0-017e821d0c97)


---

### 🚀 Paso 2: Construir la imagen

```bash
docker build -t gym-drones .
```
![Image](https://github.com/user-attachments/assets/b03e7565-0822-4e50-a0a7-e6148e29e9dc)

### 🧠 Paso 3: Permitir acceso gráfico
Antes de correr el contenedor, habilitamos el acceso del servidor X11 a Docker:
```bash
xhost +local:docker
```
![Image](https://github.com/user-attachments/assets/757fc2c5-8053-4686-809a-f3024c363123)

### 🧩 Paso 4: Ejecutar el contenedor
```bash
docker run -it --rm \
  --name drones-sim \
  -v ~/gym-pybullet-drones:/home/droneuser/gym-pybullet-drones \
  --env="DISPLAY=$DISPLAY" \
  --env="QT_X11_NO_MITSHM=1" \
  -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  gym-drones:latest
```
![Image](https://github.com/user-attachments/assets/7640755f-91f4-41aa-98a4-d9c56e1a012a)

🕹️ Paso 5: Iniciar la simulación
Dentro del contenedor:
```bash
cd ~/gym-pybullet-drones/gym_pybullet_drones/examples/
python3 pid.py
```
![Image](https://github.com/user-attachments/assets/483c5030-3050-48cb-826f-ba60521b3ea5)

![Image](https://github.com/user-attachments/assets/a9f46194-4079-43d2-8c2b-3d6bbc5313bb)

# 🤖 2️⃣ Simulación del Robot Baxter
 Repositorio base: erwincoumans/pybullet_robots

### 📁 Paso 1: Crear carpeta del proyecto
```bash
mkdir -p ~/pybullet_docker/baxter
cd ~/pybullet_docker/baxter
```
![Image](https://github.com/user-attachments/assets/43ea5bc8-fe17-49a6-bf5a-7e89e07e6f63)

### 🧩 Paso 2: Crear el archivo Dockerfile
El Dockerfile instala las dependencias necesarias y clona el repositorio con todos los robots, pero el contenedor solo se usará para el Baxter.
Explicación breve:
- Instala Python, pip, Git y librerías gráficas.
 -Instala PyBullet.
- Clona pybullet_robots y define la carpeta de trabajo.

![Image](https://github.com/user-attachments/assets/a2e0559d-b9d0-43f5-b1ec-003ebf59296c)

### ⚙️ Paso 3: Crear run_baxter.sh

Este script automatiza la ejecución del contenedor y configura el acceso gráfico X11.

Explicación breve del código:
- xhost +local:docker habilita la interfaz gráfica.
- docker run ... crea el contenedor con las variables necesarias para mostrar la ventana GUI.

![Image](https://github.com/user-attachments/assets/48e81b02-92f3-42c0-a8c9-784b68f592b6)

### 🧱 Paso 4: Construir la imagen y ejecutar
```bash
docker build -t baxter-sim .
./run_baxter.sh
```
![Image](https://github.com/user-attachments/assets/c347ac00-c881-4c9d-bb7e-784914e22b46)

### 🕹️ Paso 5: Iniciar la simulación dentro del contenedor
```bash
python3 /root/pybullet_robots/baxter_ik_demo.py
```
![Image](https://github.com/user-attachments/assets/cf4c1563-23a4-455c-8c50-b54bbbfb8567)

# 🦿 3️⃣ Simulación del Robot Atlas

Repositorio base: erwincoumans/pybullet_robots

### 📁 Paso 1: Crear carpeta
```bash
mkdir -p ~/pybullet_docker/atlas
cd ~/pybullet_docker/atlas
```
![Image](https://github.com/user-attachments/assets/5a84de0a-16a6-47e4-8086-c51a8079684d)

### 🧩 Paso 2: Editar Dockerfile
Explicación breve:
Este archivo instala las dependencias y clona el repositorio pybullet_robots, donde se encuentra el script atlas.py.

![Image](https://github.com/user-attachments/assets/114c0340-3270-4be2-b6bd-5a1dee6f232a)

### ⚙️ Paso 3: Crear run_atlas.sh

Explicación breve:

- Habilita acceso gráfico con xhost.
- Monta el socket X11.
- Lanza el contenedor atlas-sim.
- Ejecuta automáticamente python3 atlas.py.

![Image](https://github.com/user-attachments/assets/973a9a56-1bcd-46ea-a081-592a62248ff6)

### 🧱 Paso 4: Hacerlo ejecutable y construir la imagen
```bash
chmod +x run_atlas.sh
docker build -t atlas-sim .
```
![Image](https://github.com/user-attachments/assets/57ee7108-fb59-47a6-8881-eeceb2fae790)

### 🚀 Paso 5: Ejecutar la simulación
```bash
./run_atlas.sh
```
![Image](https://github.com/user-attachments/assets/6af3e503-6aa7-4534-aaf1-e873a571ae32)
![Image](https://github.com/user-attachments/assets/c83396d5-a7d8-48f3-9c96-4d9bf8fe13b9)
![Image](https://github.com/user-attachments/assets/80f0697d-0f63-4b46-b35a-f358a4313236)

# 🧩 Conclusiones

- Se implementaron tres entornos de simulación en Docker con soporte gráfico, asegurando aislamiento y portabilidad.
- Cada robot (Drones, Baxter y Atlas) fue ejecutado de forma independiente, con su propio Dockerfile y script de arranque.
- Gracias a PyBullet, se logró la visualización y control de los modelos en entornos virtualizados.
- El uso de X11 forwarding permitió que las simulaciones gráficas corrieran directamente desde los contenedores Docker.

## 🙌 Créditos

Este proyecto se desarrolló utilizando y adaptando recursos de código abierto disponibles en GitHub.  
Se agradece especialmente a los autores y comunidades que han contribuido con las siguientes herramientas y repositorios:

- **PyBullet** — Entorno de simulación física en tiempo real.  
  📦 Repositorio: [bulletphysics/bullet3](https://github.com/bulletphysics/bullet3)  
  👤 Autor principal: *Erwin Coumans*

- **Gym-PyBullet-Drones** — Entorno de simulación de drones para investigación y control.  
  📦 Repositorio: [utiasDSL/gym-pybullet-drones](https://github.com/utiasDSL/gym-pybullet-drones)  
  👥 Desarrollado por el *Dynamic Systems Lab (DSL)* de la *University of Toronto Institute for Aerospace Studies (UTIAS)*.  
  👤 Autor principal: *Simone Scherer* y colaboradores.

- **PyBullet Robots** — Conjunto de modelos robóticos para simulación (Baxter, Atlas, Cassie, Panda, entre otros).  
  📦 Repositorio: [erwincoumans/pybullet_robots](https://github.com/erwincoumans/pybullet_robots)  
  👤 Autor principal: *Erwin Coumans*

- **Docker** — Plataforma de contenedorización utilizada para aislar los entornos de simulación.  
  🌐 Sitio oficial: [https://www.docker.com](https://www.docker.com)

***Este proyecto se realizó con fines académicos y de aprendizaje, sin fines de lucro.***

---
