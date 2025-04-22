# 📘 Comandos utilizados en el proyecto "Mi App DevOps"

Este documento contiene todos los comandos utilizados hasta ahora en el desarrollo del proyecto, junto con su explicación. Ideal para documentar el aprendizaje y reutilizar en futuros proyectos DevOps.

---

## 🗂️ 1. Comandos de Linux

| Comando | Explicación |
|--------|-------------|
| `mkdir mi-app-devops` | Crea la carpeta del proyecto |
| `cd mi-app-devops` | Entra en la carpeta del proyecto |
| `nano app.py` | Abre un editor de texto en terminal |
| `echo "flask" > requirements.txt` | Crea el archivo de dependencias |
| `cat archivo` | Muestra el contenido de un archivo |
| `sudo apt install python3-venv -y` | Instala soporte para entornos virtuales |
| `python3 -m venv venv` | Crea un entorno virtual llamado `venv` |
| `source venv/bin/activate` | Activa el entorno virtual |
| `deactivate` | Sale del entorno virtual |

---

## 🐍 2. Comandos de Python/pip

| Comando | Explicación |
|--------|-------------|
| `pip install -r requirements.txt` | Instala dependencias listadas en el archivo `requirements.txt` |

---

## 🌳 3. Comandos de Git

| Comando | Explicación |
|--------|-------------|
| `git init` | Inicializa un repositorio Git |
| `git status` | Muestra el estado del repositorio |
| `git add .` | Agrega todos los archivos al área de staging |
| `git commit -m "mensaje"` | Guarda los cambios con un mensaje |
| `git log` | Muestra el historial de commits |
| `echo "venv/" > .gitignore` | Ignora la carpeta del entorno virtual |
| `git branch -M main` | Renombra la rama actual como `main` |
| `git remote add origin <url>` | Conecta el repo local con GitHub |
| `git push -u origin main` | Sube el repositorio a GitHub por primera vez |

---

## 🔐 4. GitHub Token

| Comando | Explicación |
|--------|-------------|
| `git config --global credential.helper cache` | Guarda tus credenciales temporalmente |
| `git config --global credential.helper store` | Guarda tus credenciales de forma permanente (solo si es tu equipo personal) |

---

## ✅ Notas

- Usamos un entorno virtual para mantener las dependencias del proyecto aisladas.
- Utilizamos `Flask` como micro-framework para la aplicación web.
- Subimos el proyecto a GitHub utilizando autenticación con token personal (PAT).

---

📌 *Este documento forma parte de la documentación del portafolio DevOps de Mike Velázquez.*
