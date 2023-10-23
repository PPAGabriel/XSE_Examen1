# GUÍA PARA LA INSTALACIÓN DE PRESTASHOP

Para el siguiente servicio, empleamos un contenedor Docker en el cual instalamos el servicio de PrestaShop con una base de datos MySQL.

Los pasos para levantar este servicio, fueron los siguientes:

## 1. Crear un nuevo proyecto y añadir los archivos correspondientes

Una vez creado, añadimos un archivo llamado "docker-compose.yml", añadiendo el siguiente codigo:

```dockerfile
version: '3'
services:
  mysql:
    container_name: some-mysql
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: prestashop
    ports:
      - '3306:3306'
    expose:
      - '3306'
    networks:
      - prestashop_network
  prestashop:
    container_name: prestashop
    image: prestashop/prestashop:latest
    restart: unless-stopped
    depends_on:
      - mysql
    ports:
      - 8080:80
    environment:
      DB_SERVER: some-mysql
      DB_NAME: prestashop
      DB_USER: root
      DB_PASSWD: admin
      #El siguiente comando permite que el PrestaShop sea instalado sin TestData
      PS_INSTALL_AUTO: 1
      PS_DOMAIN: localhost:8080
    networks:
      - prestashop_network
networks:
  prestashop_network:
```

Como bien se puede observar en el código, al levantar el contenedor lo alojamos en el puerto 8080, además de evitar el TestData en el dominio establecido.
La versión de PrestaShop fue la ultima (más estable), y  como base de datos usamso el servicio de MySQL versión 5.7, con un usuario "root" y contraseña "admin"

Entendemos también en el código, el uso de la dependencia. "Depends_on", para emplear setear el orden de que servicio debe iniciar y parar.

No sólo ubicamos puertos para nuestro servicio de PrestaShop, sino también, para nuestra base de datos (siendo 3306)
## 2. Levantar el servicio

Al ejecutar el proceso es realizado con éxito:

![Ejecucion del contenedor](images%2Fimg3.png)

Posteriormente, al entrar en este puerto vemos que está la página levantada sin problema.

![Pagina en ejecucion](images%2Fimg4.png)

## Conexión a la base de datos con PHPStore

Para esto. una vez nos ubicamos en nuestro "docker-compose" y entramos al apartado de servicios, escogiendo un Data Source, especificamente MySQL:

![Data Source](images%2Fimg5.png)

Comprobamos la conexión, empleando el puerto 3306, siendoel predeterminado de MySQL, y usamos "root" y "admin", como usuario y contraseña de la base de datos establecida en nuestro documento. Al comprobar la conexion, vemos que es establecida correctamente

![Test Connection](images%2Fimg2.png)

Y de esta manera tenemos nuestra base de datos ya conectada a nuestro servicio:

![servicio de base de datos 1](images%2Fimg6.png)
![servicio de base de datos 2](images%2Fimg.png)
---
## No pierda la costumbre de dar un 10 a esta entrega! :smile: