## 1. Instalar Apache y el módulo WSGI

Primero, necesitamos instalar el servidor web Apache (`httpd`) y el módulo que le permite entender Python (`mod_wsgi`).

```bash
sudo pacman -S apache mod_wsgi
```

## 2. Estructura recomendada del proyecto

Para evitar problemas de permisos, lo ideal es colocar tu proyecto en `/srv/http/` (el directorio por defecto de Apache en Arch) o en `/var/www/`. Vamos a asumir esta estructura base:

```bash
/srv/http/mi_proyecto/
├── venv/                 # Tu entorno virtual de Python
├── app.py                # Tu archivo principal de Flask (donde está `app = Flask(__name__)`)
└── mi_proyecto.wsgi      # El archivo que conectará Apache con Flask
```

> ⚠️ **Importante:** Asegúrate de que el usuario `http` (el que usa Apache en Arch) tenga permisos para leer tu proyecto:

```bash
sudo chown -R http:http /srv/http/mi_proyecto
```

## 3. Crear el archivo `.wsgi`

Este archivo es el "puente". Le dice a Apache cómo activar tu entorno virtual y dónde encontrar la aplicación de Flask.

Crea el archivo `/srv/http/mi_proyecto/mi_proyecto.wsgi` con el siguiente contenido:

```python
import sys
import logging

# Configurar el registro de errores por si algo falla
logging.basicConfig(stream=sys.stderr)

# Ruta del proyecto
sys.path.insert(0, '/srv/http/mi_proyecto')

# Activar el entorno virtual de Python (asegúrate de apuntar a tu venv)
activate_this = '/srv/http/mi_proyecto/venv/bin/activate_this.py'
with open(activate_this) as file_:
    exec(file_.read(), dict(__file__=activate_this))

# Importar la aplicación de Flask
from app import app as application
```

_(Nota: Si tu `venv` no tiene el archivo `activate_this.py`, puedes activar el entorno virtual directamente manipulando el `sys.path` antes de importar tu `app`, asegurándote de incluir el `site-packages` de tu `venv`)._

## 4. Configurar el VirtualHost en Apache

Ahora hay que decirle a Apache que escuche las peticiones y se las pase a nuestro archivo WSGI.

1. Abre el archivo de configuración principal de Apache:
    
    ```bash
    sudo nano /etc/httpd/conf/httpd.conf
    ```
    
2. Asegúrate de que la línea que carga `mod_wsgi` esté activa (descomentada). Arch suele añadirla automáticamente al final del archivo o en un archivo en `conf/extra/`, pero verifica que exista:
    
    Apache
    
    ```bash
    LoadModule wsgi_module modules/mod_wsgi.so
    ```
    
3. Al final de ese mismo archivo `httpd.conf` (o en un archivo separado en `conf/extra/` si prefieres mantenerlo limpio), añade la configuración de tu sitio:
    
```bash
<VirtualHost *:80>
    ServerName tu_dominio_o_ip

    # Configuración de WSGI en modo Demonio (Recomendado para rendimiento)
    WSGIDaemonProcess mi_proyecto python-home=/srv/http/mi_proyecto/venv user=http group=http threads=5
    WSGIProcessGroup mi_proyecto
    WSGIScriptAlias / /srv/http/mi_proyecto/mi_proyecto.wsgi

    <Directory /srv/http/mi_proyecto>
        Require all granted
    </Directory>

    # Rutas para los logs de errores y accesos
    ErrorLog "/var/log/httpd/mi_proyecto-error_log"
    CustomLog "/var/log/httpd/mi_proyecto-access_log" common
</VirtualHost>
```

## 5. Iniciar y habilitar el servicio

Una vez guardado todo, verifica que la sintaxis de configuración de Apache sea correcta:

```bash
sudo apachectl configtest
```

Si te devuelve `Syntax OK`, puedes proceder a iniciar y habilitar el servicio para que arranque con el sistema:

```bash
sudo systemctl enable --now httpd
```

## 💡 Tips de debugging (Por si algo sale mal)

En Arch, Apache es bastante estricto con los permisos. Si al entrar al navegador ves un error **500 Internal Server Error** o **403 Forbidden**, no entres en pánico. Corre esto en tu terminal para ver exactamente qué está pasando en tiempo real:

```bash
sudo tail -f /var/log/httpd/mi_proyecto-error_log
```

Los culpables más comunes suelen ser:

- El módulo `mod_wsgi` se queja de la versión de Python (recuerda que en Arch Python se actualiza rápido, tu `venv` debe estar creado con la misma versión de Python del sistema).
    
- Permisos de lectura/ejecución en la carpeta del proyecto o en el archivo `.wsgi`.