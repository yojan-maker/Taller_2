# 🧠 Proyecto: Simulación de Robots en Docker con PyBullet

Este proyecto muestra el proceso completo de **dockerización e implementación de simulaciones robóticas** usando **PyBullet**.  
Se desarrollaron tres entornos independientes para los robots:

- 🛸 **Drones (gym-pybullet-drones)**
- 🤖 **Baxter**
- 🦿 **Atlas**

Cada uno fue encapsulado en su propio contenedor Docker con soporte gráfico (X11) para visualizar las simulaciones.

---

## 📦 1️⃣ Simulación de Drones – `gym-pybullet-drones`

Repositorio base: [utiasDSL/gym-pybullet-drones](https://github.com/utiasDSL/gym-pybullet-drones)

### 🧩 Paso 1: Crear y configurar el Dockerfile

En este paso se crea un `Dockerfile` para instalar las dependencias, clonar el repositorio y preparar el entorno.  
El Dockerfile incluye:
- Instalación de **Python, pip y PyBullet**
- Instalación de librerías gráficas (para OpenGL y X11)
- Clonación del repositorio `gym-pybullet-drones`

🖼️ *Imagen 1: Creación del Dockerfile y dockerización*

---

### 🚀 Paso 2: Construir la imagen

```bash
docker build -t gym-drones .
```
🖼️ Imagen 2: Inicio de construcción en Docker

### 🧠 Paso 3: Permitir acceso gráfico
Antes de correr el contenedor, habilitamos el acceso del servidor X11 a Docker:
```bash
xhost +local:docker
```
🖼️ Imagen 3: Permiso de X11 para Docker

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
🖼️ Imagen 4: Ejecución del contenedor Docker

🕹️ Paso 5: Iniciar la simulación
Dentro del contenedor:
```bash
cd ~/gym-pybullet-drones/gym_pybullet_drones/examples/
python3 pid.py
```
🖼️ Imagen 5: Simulación de drones en PyBullet

# 🤖 2️⃣ Simulación del Robot Baxter
 Repositorio base: erwincoumans/pybullet_robots

### 📁 Paso 1: Crear carpeta del proyecto
```bash
mkdir -p ~/pybullet_docker/baxter
cd ~/pybullet_docker/baxter
```
🖼️ Imagen 6: Creación del entorno de trabajo Baxter

### 🧩 Paso 2: Crear el archivo Dockerfile
El Dockerfile instala las dependencias necesarias y clona el repositorio con todos los robots, pero el contenedor solo se usará para el Baxter.
Explicación breve:
- Instala Python, pip, Git y librerías gráficas.
 -Instala PyBullet.
- Clona pybullet_robots y define la carpeta de trabajo.

🖼️ Imagen 7: Edición del Dockerfile Baxter

### ⚙️ Paso 3: Crear run_baxter.sh

Este script automatiza la ejecución del contenedor y configura el acceso gráfico X11.

Explicación breve del código:
- xhost +local:docker habilita la interfaz gráfica.
- docker run ... crea el contenedor con las variables necesarias para mostrar la ventana GUI.

🖼️ Imagen 8: Edición del run_baxter.sh

### 🧱 Paso 4: Construir la imagen y ejecutar
```bash
docker build -t baxter-sim .
./run_baxter.sh
```
🖼️ Imagen 9: Construcción e inicio del contenedor Baxter

### 🕹️ Paso 5: Iniciar la simulación dentro del contenedor
```bash
python3 /root/pybullet_robots/baxter_ik_demo.py
```
🖼️ Imagen 10: Simulación de Baxter funcionando

# 🦿 3️⃣ Simulación del Robot Atlas

Repositorio base: erwincoumans/pybullet_robots

### 📁 Paso 1: Crear carpeta
```bash
mkdir -p ~/pybullet_docker/atlas
cd ~/pybullet_docker/atlas
```
🖼️ Imagen 11: Creación del entorno Atlas

### 🧩 Paso 2: Editar Dockerfile
Explicación breve:
Este archivo instala las dependencias y clona el repositorio pybullet_robots, donde se encuentra el script atlas.py.

🖼️ Imagen 12: Edición del Dockerfile Atlas

### ⚙️ Paso 3: Crear run_atlas.sh

Explicación breve:

- Habilita acceso gráfico con xhost.
- Monta el socket X11.
- Lanza el contenedor atlas-sim.
- Ejecuta automáticamente python3 atlas.py.

🖼️ Imagen 13: Edición del run_atlas.sh

### 🧱 Paso 4: Hacerlo ejecutable y construir la imagen
```bash
chmod +x run_atlas.sh
docker build -t atlas-sim .
```
🖼️ Imagen 14: Construcción de la imagen Atlas

### 🚀 Paso 5: Ejecutar la simulación
```bash
./run_atlas.sh
```
🖼️ Imagen 15: Simulación del robot Atlas en PyBullet

# 🧩 Conclusiones

- Se implementaron tres entornos de simulación en Docker con soporte gráfico, asegurando aislamiento y portabilidad.
- Cada robot (Drones, Baxter y Atlas) fue ejecutado de forma independiente, con su propio Dockerfile y script de arranque.
- Gracias a PyBullet, se logró la visualización y control de los modelos en entornos virtualizados.
- El uso de X11 forwarding permitió que las simulaciones gráficas corrieran directamente desde los contenedores Docker.
