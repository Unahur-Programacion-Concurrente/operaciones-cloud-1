# Laboratorio Cloud Computing

## Regiones y Servicios AWS

[https://aws.amazon.com/about-aws/global-infrastructure/\#AWS\_Global\_Infrastructure\_Map](https://aws.amazon.com/about-aws/global-infrastructure/)

## 1. Inicio del Laboratorio

1. Buscar email de: **AWS Academy**, Asunto: **Invitación al curso**

2. Hacer click en **Comenzar**

3. Seguir las instrucciones para habilitar la cuenta y conectarse.

4. En la página de inicio: **AWS Academy Learner Lab**, hacer click en **Módulos*

5. Seleccionar y hacer click en: **Iniciar el Laboratorio de aprendizaje de AWS Academy**

6. Seleccionar: **Cargar e Iniciar el Laboratorio de aprendizaje de AWS Academy en una ventana nueva**

7. En la pagina del laboratorio, hacer click en **Start Lab**

8. Esperar hasta que el punto **AWS** pase de amarillo a verde

9. Cuando el indicador **AWS** este verde, hacer click en **AWS**. Con eso se abre la consola de AWS (en inglés, se recomienda dejar en este idioma)

## 2. Creación de una instancia EC2

1. Ir al servicio EC2

2. Seleccionar **Launch Instances**

3. Colocar un nombre, por ej: **laboratorio1**

4. En **Application and OS images**, seleccionar **Amazon Linux 2023 AMI** (default)

5. En **instance type** seleccionar: **t2.micro**

6. En **Key pair name**, seleccionar **Create new key pair
   
   Ingresar:
   
   - **Nombre**: **laboratorio**
   
   - **Key Pair Type**: **RSA**
   
   - **File format**: **.pem**
   
   Se va a descargar un archivo **laboratorio.pem**, guardarlo.

7. En **Firewall**, elegir **Create Security Group**
   
    Ingresar:
   
   - Seleccionar **Allow SSH** 

8. hacer click en **Launch Instance**

9. Una vez completado el lanzamiento, hacer click en **“instances**”

10. Esperar  hasta que** Instance state**= **Running** y Status check = **2/2 checks passed**.
    
    Puede refrescar la pantalla con el boton de refresco.

11. Conectarse a la instancia con cliente SSH
    
    1. Seleccionar la instancia
    
    2. Click en Details, copiar el valor de Public IPv4 DNS
    
    3. Colocar el archivo laboratorio.pem en su directorio actual.
    
    4. Cambiar los permisos del archivo: chmod 400 laboratorio.pem
    
    5. Iniciar conexión: `ssh \-i laboratorio.pem ec2-user@\<Public IPv4 DNS del punto anterior\>`

12. Ejemplo:
    
    `ssh -i laboratorio.pem ec2-user@ec2-54-152-76-222.compute-1.amazonaws.com`

13. Conectarse con EC2 instance connect

14. Detener y arrancar instancia (y observar observar lo que ocurrió con Public IPv4 y Public IPv4 DNS)

15. Terminar la instancia

## 3. Base de Datos MySQL conectada a instancia EC2

### 3.1. Crear base de datos MySQL

1. En la consola de AWS buscar el servicio **RDS** 

2. Hacer click en **Crear base de datos** 

3. Seleccionar **Creación Standard** en método de creación

4. Seleccionar **MySQL** en **Tipo de motor**

5. Seleccionar **Capa gratuita** en **Plantillas**

6. Identificador de instancias de bases de datos: **database-1**

7. Nombre de usuario maestro: **admin**

8. Contraseña maestra: **password**

9. Confirmar la contraseña maestra

10. Dejar el resto de los campos en sus valores por defecto

11. Bajar al final de la página y hacer click en **Crear base de datos**

12. Si aparece un pop-up con complementos sugeridos hacer click en Cerrar

13. Esperar hasta que el estado de la base de datos sea **Disponible**, esto puede demorar unos minutos.

14. En la lista de bases de datos hacer click en **database-1**

15. Ir a la pestaña Conectividad y seguridad y copiar el **Punto de enlace**
    
    Ejemplo: `database-1.cjxro5kry7ld.us-east-1.rds.amazonaws.com`

16. (Opcional) probar conectividad desde PC local.

### 3.2. Crear una instancia EC2

1. En la consola de AWS buscar el servicio **EC2**

2. Hacer click en **Lanzar instancia** 

3. En el campo Nombre ingresar: **server**  

4. En Amazon Machine Image elegir: **AMI de Ubuntu**  (la mas reciente de la capa gratuita)

5. En Tipo de instancia elegir: **t2.micro**  

6. Hacer click en **Crear un nuevo par de claves** (si ya tiene una clave creada en el ejercicio 1 puede omitir este paso) 
   
   - En Nombre del par de claves ingresar: **laboratorio**  
   
   - Tipo de par de claves: **RSA**  
   
   - Formato de archivo: **.pem**  
   
   - Click en **Crear par de claves**  

7. Verificar que el explorador bajo el archivo laboratorio.pem  

8. En Nombre del par de claves ingresar: **laboratorio**  

9. En Configuración de red hacer click en **Editar**  

10. En Firewall seleccionar: **Crear un grupo de seguridad**  

11. En Nombre del grupo de seguridad ingresar: **ec2-ssh-http**  

12. En Descripción ingresar: **Habilita acceso ssh y http desde internet**  

13. En Reglas de entrada:  
    
    - dejar la regla **ssh**  
    
    - Hacer click en **Agregar regla del grupo de seguridad**  
    
    - En Tipo, seleccionar: **HTTP** 
    
    - En Tipo de origen seleccionar: **Cualquier lugar**  

14. Ir al final de la página y hacer click en **Lanzar instancia**

15. En el menú de la izquierda seleccionar Instancias. 

16. Esperar hasta que el estado de la instancia esté **En ejecución** y Comprobación de estado en **2/2 comprobaciones superadas**.

17. Probar la conexión de consola (shell) con la instancia.

### 3.3. Configurar la conexión entre la base de datos y la instancia EC2

1. En la consola de AWS buscar el servicio **RDS**

2. En el panel de la izquierda seleccionar **Bases de datos** 

3. Hacer click en el nombre de **database-1** y seleccionar **Conectividad y seguridad**

4. Recursos de computación conectados y hacer click en **Configuración de conexión de EC2**  

5. En **Instancia EC2** seleccionar la instancia **server**  

6. Hacer click en **Continuar**  

7. Revisar los cambios de configuración en la base de datos **database-1** y la instancia **EC2**.  

8. Ir al final de la página y hacer click en **Configurar**

### 3.4. Instalar Docker en la instancia EC2

1. Abrir una conexión de consola con la instancia EC2 (usando ssh o por EC2 instance connect)

2. Instalar paquetes de requisitos previos para que apt pueda usar paquetes por HTTPS  
   
   ```
   sudo apt update  
   sudo apt install apt-transport-https ca-certificates curl software-properties-common  
   ```

3. Instalar el repositorio de Docker
   
   ```
   # Add Docker's official GPG key:
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   
   # Add the repository to Apt sources:
   echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   
   sudo apt-get update
   ```

4. Instalar paquetes de Docker
   
   ```
   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

5. Comprobar que el demonio Docker está corriendo
   
   ```
   sudo systemctl status docker
   ```

6. Verificar la instalación de Docker Engine ejecutando la imagen “hello-world”
   
   ```
   sudo docker run hello-world
   ```

7. Configurar Docker para arrancar automáticamente cuando se reinicia el sistema
   
   ```
   sudo systemctl enable docker.service  
   sudo systemctl enable containerd.service  
   ```

8. Instalar Docker Compose
   
   ```
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   
   sudo chmod +x /usr/local/bin/docker-compose
   
   docker-compose --version
   ```

### 3.5. Probar conectividad de la instancia EC2 con la base de datos

   Debe tener a mano el valor del endpoint de la base de datos copiado en el paso 1.

   Ejemplo: `database-1.cjxro5kry7ld.us-east-1.rds.amazonaws.com`

   `sudo docker run -it --rm mysql mysql -h<endpoint> -uadmin -p  -P3306`

   Ejemplo:

   `sudo docker run -it --rm mysql mysql -hdatabase-1.cjxro5kry7ld.us-east-1.rds.amazonaws.com -uadmin -p  -P3306`

*Opcional: Probar conectividad instalando un cliente mysql en instancia EC2*

```
sudo dnf update -y
sudo dnf install mariadb105
mysql -h <endpoint> -u admin -p
```

   Ejemplo

   `mysql -h database-1.cjxro5kry7ld.us-east-1.rds.amazonaws.com -u admin -p`

   Si está todo bien, se conectará a MySQL.

## 4. Instalacion de Wordpress en Contenedores y Nube

### 4.1 Crear la base de datos de Wordpress

Abrir sesion mysql e ingresar:

```
create user 'wp-user'@'%' identified by 'password';

CREATE DATABASE `wordpress` DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

grant all privileges on `wordpress`.* to 'wp-user'@'%';

FLUSH PRIVILEGES;

exit
```

### 4.2 Crear directorio en instancia para proyecto

En la instancia EC2:

```
mkdir ~/wordpress
cd ~/wordpress
```

### 4.3 Crear o editar archivo de variables de entorno

`nano .env`

Agregar las siguientes líneas (utiizar el <endpoint> configurado en el ejercicio anterior:

```
MYSQL_USER=wpuser

MYSQL_PASSWORD=password

DB_HOST=<endpoint>:3306

DB_NAME=wordpress
```

### 4.3 Crear o editar archivo de configuración de nginx

`nano nginx.conf`

Agregar o revisar las siguientes líneas

```
server {

        listen 80;

        listen [::]:80;

        server_name www.ejemplo.com;

        index index.php index.html index.htm;

        root /var/www/html;

        location / {

                try_files $uri $uri/ /index.php$is_args$args;

        }

        location ~ \.php$ {

                try_files $uri =404;

                fastcgi_split_path_info ^(.+\.php)(/.+)$;

                fastcgi_pass wordpress:9000;

                fastcgi_index index.php;

                include fastcgi_params;

                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

                fastcgi_param PATH_INFO $fastcgi_path_info;

        }

        location ~ /\.ht {

                deny all;

        }

        location = /favicon.ico {

                log_not_found off; access_log off;

        }

        location = /robots.txt {

                log_not_found off; access_log off; allow all;

        }

        location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {

                expires max;

                log_not_found off;

        }

}
```

### 4.3 Crear o editar el archivo docker-compose.yaml

Agregar las siguientes líneas:

```
version: '3'
services:
  wordpress:
    image: wordpress:5.1.1-fpm-alpine
    container_name: wordpress
    restart: unless-stopped
    env_file: .env
    environment:
      - WORDPRESS_DB_HOST=$DB_HOST
      - WORDPRESS_DB_USER=$MYSQL_USER
      - WORDPRESS_DB_PASSWORD=$MYSQL_PASSWORD
      - WORDPRESS_DB_NAME=$DB_NAME
    volumes:
      - wordpress:/var/www/html
    networks:
      - app-network

  webserver:
    depends_on:
      - wordpress
    image: nginx:1.15.12-alpine
    container_name: webserver
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - wordpress:/var/www/html
      - ./nginx-conf:/etc/nginx/conf.d
    networks:
      - app-network
volumes:
    wordpress:
networks:
    app-network:
        driver: bridge
```

### 4.4 Arrancar los contenedores

`docker-compose up`

### 4.5 Probar acceso al blog

Con un explorador abrir:

`http://<EC2 Instance Public IPv4 DNS>`
