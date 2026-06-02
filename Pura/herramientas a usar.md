¡Aquí tienes al grano! Lo que hace cada herramienta y su ejemplo de uso rápido.

### Herramientas de Sistema y Comandos

- **`ss`**: Sirve para **investigar sockets de red** (ver qué puertos están abiertos, conexiones activas, etc.). Reemplazó al viejo `netstat`.
    
    > `ss -tuln` _(Muestra todos los puertos TCP y UDP en escucha)._
    
- **`chown`**: Cambia el **propietario y/o grupo** de un archivo o carpeta.
    
    > `sudo chown www-data:www-data /var/www/html` _(Pasa la propiedad al usuario y grupo del servidor web)._
    
- **`chmod`**: Cambia los **permisos de acceso** (lectura, escritura, ejecución) de un archivo o carpeta.
    
    > `chmod 755 script.sh` _(Da todos los permisos al dueño, y solo lectura/ejecución al resto)._
    
- **`tail`**: Muestra las **últimas líneas** de un archivo. Ideal para ver archivos de registro (logs) en tiempo real.
    
    > `tail -f /var/log/apache2/error.log` _(Sigue mostrando los nuevos errores que entren al archivo en vivo)._
    
- **`nano`**: Un **editor de texto** en consola muy sencillo y directo.
    
    > `nano mi_archivo.txt` _(Abre el archivo para editarlo directamente en la terminal)._
    
- **`tee`**: Lee la entrada estándar y la **escribe tanto en la pantalla como en uno o varios archivos**. Útil para redirigir salida cuando necesitas usar `sudo`.
    
    > `echo "nueva_config" | sudo tee -a /etc/config.conf` _(Muestra el texto en pantalla y lo añade al final del archivo protegido)._
    

### Servidor Web y Despliegue

- **`apache`**: El **servidor web HTTP** encargado de recibir las peticiones de los usuarios y devolver páginas web o archivos.
    
    > `sudo systemctl restart httpd` _(Reinicia el servidor para aplicar cambios)._
    
- **`mod_wsgi`**: Un módulo para Apache que le permite **comunicarse con aplicaciones Python** (como Flask o Django). Traduce las peticiones HTTP a algo que Python entienda.