# Demo de Predicción de Churn

Este proyecto contiene un notebook de demo para **predicción de abandono de clientes (customer churn)** usando el dataset **Telco Customer Churn**.

El objetivo es poder levantarlo rápido en una máquina virtual como:

- DigitalOcean Droplets
- AWS EC2
- Azure Virtual Machines
- Cualquier servidor Linux con acceso por SSH

## Contenido del proyecto

- `main.ipynb`: notebook principal del demo
- `requirements.txt`: dependencias del entorno
- `telco_churn.csv`: dataset esperado en la raíz del proyecto

## Requisitos mínimos sugeridos de máquina

Para una demo fluida en una máquina virtual, se recomienda como mínimo:

- `RAM`: 4 GB
- `Procesador`: 2 vCPU
- `Almacenamiento`: 20 GB SSD

Configuración recomendada para una experiencia más cómoda:

- `RAM`: 8 GB
- `Procesador`: 2 a 4 vCPU
- `Almacenamiento`: 30 GB SSD

## Requisitos previos

- Python 3.11 o superior
- `uv` instalado
- Acceso SSH a la máquina virtual

## 1. Conectarse a la máquina virtual

Ejemplo:

```bash
ssh usuario@IP_PUBLICA
```

## 2. Instalar Python y utilidades básicas

En Ubuntu/Debian:

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip curl git
```

## 3. Instalar uv

Instala `uv` con el método oficial:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Luego recarga tu shell:

```bash
source ~/.bashrc
```

Si el comando no queda disponible, también puedes abrir una nueva sesión SSH.

## 4. Clonar o copiar el proyecto

Si estás usando Git:

```bash
git clone <URL_DEL_REPOSITORIO>
cd ml_churn
```

Si no estás usando Git, copia la carpeta del proyecto al servidor y entra al directorio:

```bash
cd ml_churn
```

## 5. Crear el entorno virtual con uv

```bash
uv venv
```

Activa el entorno:

```bash
source .venv/bin/activate
```

## 6. Instalar dependencias

```bash
uv pip install -r requirements.txt
```

## 7. Verificar archivos requeridos

Asegúrate de tener estos archivos en la raíz del proyecto:

- `main.ipynb`
- `telco_churn.csv`

Puedes validarlo con:

```bash
ls -lah
```

## 8. Levantar Jupyter Lab

Para trabajar remotamente en la VM:

```bash
jupyter lab --ip 0.0.0.0 --port 8888 --no-browser
```

Si quieres dejar Jupyter ejecutándose en segundo plano:

```bash
nohup jupyter lab \
  --ip 0.0.0.0 \
  --port 8888 \
  --no-browser \
  --allow-root \
  --ServerApp.token='jY3VgAYDQJFi41W2yI' \
> jupyter.log 2>&1 &
```

Puedes revisar el log con:

```bash
tail -f jupyter.log
```

### Acceso desde navegador

Abre en tu navegador:

```text
http://IP_PUBLICA:8888
```

Notas importantes:

- Debes abrir el puerto `8888` en el firewall o security group.
- Jupyter te mostrará un token en consola para iniciar sesión.
- En producción real conviene proteger el acceso con túnel SSH, proxy reverso o HTTPS.

## 9. Ejecutar el notebook

Dentro de Jupyter Lab:

1. Abre `main.ipynb`
2. Ejecuta las celdas de arriba hacia abajo
3. Revisa métricas, visualizaciones y predicción demo

## 10. Ejecutar MLflow UI

Para ver experimentos y modelos registrados:

```bash
mlflow ui --host 0.0.0.0 --port 5000
```

Si quieres dejar MLflow ejecutándose en segundo plano:

```bash
nohup mlflow ui \
  --host 0.0.0.0 \
  --port 5000 \
  --backend-store-uri sqlite:///mlflow.db \
  --default-artifact-root ./mlruns \
  --allowed-hosts '*' \
  --cors-allowed-origins '*' \
> mlflow.log 2>&1 &
```

Puedes revisar el log con:

```bash
tail -f mlflow.log
```

Luego abre:

```text
http://IP_PUBLICA:5000
```

Recuerda habilitar el puerto `5000` en el firewall de la VM.

## 11. Puertos a habilitar

Según cómo quieras trabajar, normalmente necesitarás:

- `22` para SSH
- `8888` para Jupyter Lab
- `5000` para MLflow UI

## 12. Comandos rápidos de referencia

Crear entorno:

```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

Levantar Jupyter:

```bash
jupyter lab --ip 0.0.0.0 --port 8888 --no-browser
```

Levantar Jupyter en background:

```bash
nohup jupyter lab \
  --ip 0.0.0.0 \
  --port 8888 \
  --no-browser \
  --allow-root \
  --ServerApp.token='jY3VgAYDQJFi41W2yI' \
> jupyter.log 2>&1 &
```

Levantar MLflow:

```bash
mlflow ui --host 0.0.0.0 --port 5000
```

Levantar MLflow en background:

```bash
nohup mlflow ui \
  --host 0.0.0.0 \
  --port 5000 \
  --backend-store-uri sqlite:///mlflow.db \
  --default-artifact-root ./mlruns \
  --allowed-hosts '*' \
  --cors-allowed-origins '*' \
> mlflow.log 2>&1 &
```

## 13. Solución de problemas

### `uv: command not found`

Recarga la shell o vuelve a conectarte por SSH:

```bash
source ~/.bashrc
```

### No abre Jupyter o MLflow desde el navegador

Revisa:

- que el servicio esté corriendo
- que la IP pública sea correcta
- que el firewall permita el puerto
- que el proveedor cloud tenga abierto el security group o regla de red

### Falta el dataset

El notebook espera el archivo:

```text
telco_churn.csv
```

Debe estar en la misma carpeta que `main.ipynb`.

## 14. Recomendación para demo

Antes de presentar:

- valida que el dataset cargue correctamente
- ejecuta todo el notebook una vez completa
- deja corriendo Jupyter Lab
- deja corriendo MLflow UI si vas a mostrar experiment tracking

Con eso tendrás un entorno simple, reproducible y listo para una demo técnica o de negocio.
