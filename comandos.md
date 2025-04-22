# 📘 Comandos utilizados en el proyecto "Mi App DevOps"

Este documento contiene todos los comandos utilizados en el proyecto, explicados paso a paso. Está diseñado como una guía de referencia rápida y también como parte de la documentación técnica del portafolio DevOps.

---

## 🗂️ 1. Comandos de Linux

| Comando | Explicación |
|--------|-------------|
| `mkdir mi-app-devops` | Crea una nueva carpeta llamada `mi-app-devops`. Usado para organizar el proyecto. |
| `cd mi-app-devops` | Entra a la carpeta `mi-app-devops` para trabajar dentro de ella. |
| `nano app.py` | Abre un archivo llamado `app.py` en el editor de texto de terminal `nano`. |
| `echo "flask" > requirements.txt` | Crea un archivo `requirements.txt` que contiene el texto `flask`, especificando que se debe instalar esa librería. |
| `cat archivo.txt` | Muestra el contenido del archivo `archivo.txt` en pantalla. |
| `chmod 400 mikey.pem` | Cambia los permisos del archivo `.pem` para que solo el propietario pueda leerlo (necesario para conectarse vía SSH con seguridad). |
| `ssh -i mikey.pem ubuntu@<IP>` | Se conecta a una instancia EC2 usando el archivo `.pem` como clave privada. |
| `sudo apt update` | Actualiza la lista de paquetes disponibles desde los repositorios. |
| `sudo apt install <paquete>` | Instala un paquete desde los repositorios. Ej: `python3-venv`, `docker.io`. |
| `newgrp docker` | Aplica cambios de grupo sin tener que cerrar sesión (útil después de agregar tu usuario al grupo `docker`). |

---

## 🐍 2. Comandos de Python y pip

| Comando | Explicación |
|--------|-------------|
| `python3 -m venv venv` | Crea un entorno virtual en la carpeta `venv`, aislando las dependencias del sistema. |
| `source venv/bin/activate` | Activa el entorno virtual para que puedas instalar paquetes localmente. |
| `pip install -r requirements.txt` | Instala las librerías listadas en `requirements.txt`. Usado para instalar Flask. |
| `deactivate` | Sale del entorno virtual activo y vuelve al sistema base. |

---

## 🌳 3. Comandos de Git

| Comando | Explicación |
|--------|-------------|
| `git init` | Inicializa un nuevo repositorio Git en la carpeta actual. |
| `git status` | Muestra los archivos modificados, nuevos o listos para hacer commit. |
| `echo "venv/" > .gitignore` | Crea un archivo `.gitignore` que le dice a Git que ignore la carpeta `venv/`. |
| `git add .` | Agrega todos los archivos al área de staging para prepararlos para el commit. |
| `git commit -m "mensaje"` | Guarda una nueva versión del código con un mensaje descriptivo. |
| `git branch -M main` | Renombra la rama actual a `main` (rama principal por convención moderna). |
| `git remote add origin <URL>` | Conecta el repositorio local con el remoto en GitHub. |
| `git push -u origin main` | Sube la rama principal al repositorio remoto y establece seguimiento. |

---

## 🔐 4. Configuración de GitHub con token

| Comando | Explicación |
|--------|-------------|
| `git config --global credential.helper cache` | Guarda tus credenciales en memoria por un tiempo para no tener que escribirlas cada vez. |
| `git config --global credential.helper store` | Guarda tus credenciales de manera permanente (solo recomendable en equipo personal). |

---

## 🐳 5. Comandos de Docker

| Comando | Explicación |
|--------|-------------|
| `nano Dockerfile` | Crea y edita el archivo `Dockerfile` que define cómo construir la imagen del contenedor. |
| `docker build -t mi-app-devops .` | Construye una imagen Docker desde el `Dockerfile`, con nombre `mi-app-devops`. El punto `.` indica que el contexto de construcción es el directorio actual. |
| `docker run -p 5000:5000 mi-app-devops` | Ejecuta un contenedor y conecta el puerto 5000 del host al del contenedor. |
| `docker run -d -p 80:5000 mi-app-devops` | Igual al anterior, pero corre el contenedor en segundo plano (`-d`) y expone en el puerto 80. |
| `docker ps` | Lista los contenedores en ejecución. |
| `docker ps -a` | Lista todos los contenedores, incluso los que están detenidos. |
| `docker stop <id>` | Detiene un contenedor en ejecución usando su ID. |
| `docker rm <id>` | Elimina un contenedor (detenido). |
| `docker images` | Lista las imágenes Docker disponibles en el sistema. |
| `docker rmi <id>` | Elimina una imagen Docker por su ID. |
| `sudo usermod -aG docker $USER` | Agrega tu usuario al grupo `docker` para que puedas usar Docker sin `sudo`. |

---
# ☁️ Despliegue de la App Flask en AWS EC2 usando Docker

Este documento explica paso a paso cómo desplegué una aplicación web desarrollada en Flask, contenida con Docker, en una instancia EC2 de AWS. Todos los comandos están explicados para facilitar el aprendizaje y la reutilización en otros proyectos DevOps.

---

## 🧱 Fase 1: Crear la aplicación con Flask

### 1. Crear el entorno de trabajo

```bash
mkdir mi-app-devops
cd mi-app-devops
```

### 2. Crear la aplicación `app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "¡Hola DevOps desde Flask!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### 3. Crear el archivo de dependencias

```bash
echo "flask" > requirements.txt
```

### 4. Crear y activar entorno virtual

```bash
sudo apt install python3-venv -y         # Solo si no tienes venv instalado
python3 -m venv venv                     # Crea un entorno virtual
source venv/bin/activate                 # Activa el entorno
```

### 5. Instalar dependencias

```bash
pip install -r requirements.txt
```

---

## 🌳 Fase 2: Control de versiones con Git y GitHub

### 1. Inicializar repositorio Git

```bash
git init
git status
echo "venv/" > .gitignore
git add .
git commit -m "Primer commit: app Flask"
```

### 2. Crear repositorio en GitHub y conectarlo

```bash
git remote add origin https://github.com/tu-usuario/mi-app-devops.git
git branch -M main
git push -u origin main
```

---

## 🐳 Fase 3: Contenerizar la app con Docker

### 1. Crear `Dockerfile`

```Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

### 2. Construir imagen Docker

```bash
docker build -t mi-app-devops .
```

### 3. Ejecutar la app en contenedor local

```bash
docker run -p 5000:5000 mi-app-devops
```

---

## ☁️ Fase 4: Despliegue en AWS EC2

### 1. Crear instancia EC2 (en consola AWS)
- AMI: Ubuntu Server 22.04 LTS
- Tipo: t2.micro (gratis)
- Abrir puertos: 22 (SSH), 80 (HTTP)
- Descargar archivo `.pem`

### 2. Conectarse vía SSH

```bash
chmod 400 mikey.pem
ssh -i mikey.pem ubuntu@<IP_PUBLICA>
```

### 3. Instalar Docker en EC2

```bash
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker ubuntu
newgrp docker
docker --version
```

### 4. Clonar y desplegar la app

```bash
git clone https://github.com/tu-usuario/mi-app-devops.git
cd mi-app-devops
docker build -t mi-app-devops .
docker run -d -p 80:5000 mi-app-devops
```

### 5. Ver la app en navegador

```
http://<IP_PUBLICA>
```

---

📌 Este archivo forma parte de la documentación oficial del portafolio DevOps de Mike Velázquez.
