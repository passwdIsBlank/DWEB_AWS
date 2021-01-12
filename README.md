# ¿Que es EC2?
Elastic Compute Cloud se refiere a proveer un servicio de computadora virtual adaptable a un usuario de forma que este se adapte a las necesidades del usuario.

## Instancia de Ubuntu 20.04
![Resumen de la instancia creada](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/instance.PNG)

# Apache y PHP

1. ``sudo apt install apache2 php libapache2-mod-php php7.4-fpm``
2. ``sudo vim /etc/php/7.4/apache2/php.ini`` y editamos lo siguiente:
    - ``cgi.fix_pathinfo=1`` esta linea activa el autocompletado de la ruta de forma que intente ejecutar el archivo más cercano en caso de no encontrar el archivo PHP solicitado.
    - ``sudo systemctl restart apache2`` Reiniciamos el servicio de apache.
3. Estado de los servicios:
    - Apache
    ![Estado de servicio de Apache](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/status_APACHE.PNG)
    - PHP
    ![Estado de servicio de PHP](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/status_PHP.PNG)
4. Podemos pasar a comprobar que el servidor funciona correctamente, para ello en AWS debemos añadir una regla para habilitar el tráfico al puerto 80 para nuestra instancia.
    - En el navegador vamos a AWS y clickamos en la instancia de Ubuntu 20.04
    - En la segunda sección vamos a la pestaña de 'Seguridad'
    - Una vez en la pestaña cickamos en el grupo de seguridad en cuestión y vamos a 'Editar reglas de entrada'
    ![Habilitar trafico HTTP en la instancia paso 1](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/HTTP_enable_1.PNG)
    - Añadimos una nueva regla para el protocolo HTTP y desde cualquier IP 0.0.0.0/0, una vez hecho guardamos los cambios
    ![Habilitar trafico HTTP en la instancia paso 2](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/HTTP_enable_2.PNG)
    Comprobamos mediante el navegador (De momento usamos la IP pública del server:
    ![Comprobación por medio del navegador](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/HTTP_test.PNG)
# MYSQL

1. ``sudo apt install mysql-server``
2. ``sudo mysql_secure_installation``
    - En mi caso solo desactive la 1ra opción de comprobación de la seguridad de la clave, el resto yes
3. Estado del servicio:
![Estado de servicio de MySQL](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/status_MYSQL.PNG)
# FTP

1. ``sudo apt install vsftpd``
2. ``sudo vim /etc/vsftpd.conf`` y editamos (por el momento anonymous_enable estará activado tambien)
    - ``anonymous_enable=YES`` esta opción permite logearse con usuarios anónimos
    - ``local_enable=YES`` esta opción permite logearse con usuarios locales
3. ``sudo systemctl restart vsftpd`` y reiniciamos el servicio
4. Estado del servicio:
![Estado de servicio de FTP](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/status_FTP.PNG)
4. Igual que con Apache debemos habilitar el tráfico al puerto 21 para que podamos acceder por medio de FTP, seguir paso 3 de Apache cambiando los datos de la regla
   ![Habilitar trafico FTP en la instancia](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/FTP_enable_1.PNG)
   Comprobamos mediante CMD (De momento usamos la IP pública del server):
   ![Comprobación mediante CMD](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/FTP_test.PNG)
   
# IP Elástica

## ¿Qué es y para qué sirve una IP Elástica?
En esencia es lo mismo que una IP estática solo que optimizada para la informática en la nube dinámica.

## Configuración de una IP Elástica
1. En el navegador vamos a AWS y clickamos en la pestaña de 'Direcciones IP elásticas' en 'Red y Seguridad', aquí clickaremos sobre 'Asignar la dirección IP elástica'
![Crear IP Elástica 1](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/Elastic_IP_1.PNG)
2. En este paso no modificamos nada
![Crear IP Elástica 2](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/Elastic_IP_2.PNG)
3. Clickamos sobre 'Asociar esta dirección IP elástica'
![Asignar IP Elástica a instancia 1](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/Elastic_IP_3.PNG)
4. Seleccionamos la instancia y a que IP privada estará asociada nuestra IP elástica
![Asignar IP Elástica a instancia 2](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/Elastic_IP_4.PNG)
5. Comprobamos mediante el navegador (Usamos la IP elástica en vez de la IP pública del server):
![Comprobación mediante navegador](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/Elastic_IP_test.PNG)

# DNS

## ¿Que es y para que sirve el DNS?
DNS significa Domain Name System, es un protocolo creado para asociar diferentes dispositivos en redes IP a nombres, hay bastantes tipos de registros utilizados y consta de una zona directa e inversa cuyas funciones generales respectivamente son resolución Nombre => IP e IP => Nombre.

##Tipos de registros
Estos son algunos de los registros disponibles:
-  A: Este hace referencia a la dirección IPv4 de un servidor web y es el más típico de encontrarnos en los servidores DNS.
- AAAA: Este hace referencia a la dirección IPv6 de un host. Es igual que el registro “A”, pero refiriéndose a una dirección IPv6 y no IPv4.
- CNAME: Este hace referencia a un alias de otro dominio. Es decir, su función es hacer que un dominio sea un alias de otro dominio. Normalmente este tipo de registros se utilizan para asociar nuevos subdominios con dominios ya existentes del registro A.
- MX: Este hace referencia a una lista de servidor de intercambio de correo que se debe utilizar para el dominio.
- PTR: Este hace referencia a un punto de terminación de red. Es decir, que la sintaxis de DNS es la responsable del mapeo de una dirección IPv4 para el CNAME en el alojamiento.
- NS: Este hace referencia a que servidor de nombres es el autorizado para el dominio.
- SOA: Este hace referencia al comienzo de autoridad. Este registro es uno de los registros DNS más importantes porque guarda información esencial como la fecha de la última actualización del dominio, otros cambios y actividades.
- SRV: Este hace referencia a un servicio. Es decir, se utiliza para la definición de un servicio TCP en el que opere en el dominio.
- TXT: Este hace referencia a un texto. Es decir, permite que los administradores inserten texto en el registro DNS. Esto se utiliza para dejar notas sobre información del dominio.

## DNS del CPanel de Guebs
![Primeros tres registros del CPanel de Guebs](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/cpanel_DNS_records.PNG)
- El primer registro es tipo A y asocia el nombre grupo1.zerbitzaria.net a la IP 54.76.1.76
- El segundo registro es tipo MX y nos dice donde está el servidor de mail del nombre grupo1.zerbitzaria.net
- El tercer registro es tipo TXT y es una cadena de carácteres

## ¿Cuantos servidores DNS existen?
Hay 13 servidores raíz y los que haya creado la gente.

## ¿Cuántas redirecciones DNS son posibles?
Hasta que se pregunte al root server o el root server responda que no sabe donde está.

## ¿Qué son los servidores DNS Raíz?
Son los que están más arriba en la jerarquía y contienen la información sobre quien puede poseer la información solicitada

## ¿Para qué montar un servidor si simplemente escribiendo en un fichero la relación IP/Nombre el sistema ya funcionaría?
Bueno, pues para no tener que repetir el mismo proceso en cada ordenador que haya en la red, esto automatizaría el proceso al preguntarle al servidor DNS.

## Según lo expuesto, y si en tu configuración de red del sistema operativo solamente posees un servidor DNS, entonces: ¿cuál sería el proceso para encontrar la IP de la dirección web: http://www.debian.org/distrib/netinst?
El servidor al que tu ordenador mandará la petición reenviará dicha petición a el siguiente servidor DNS y así sucesivamente hasta que se encuentre la dirección o hasta que un root server responda que no posee la información.

## ¿Es posible si dispones de una conexión a Internet con IP dinámica ofrecer servicios en Internet? Es decir, si quieres ofrecer los servicios DNS, no dispones de IP estática, esto es, cada vez que te conectas a Internet tu IP, aunque a veces sea la misma, no siempre es la misma.
Es posible con un DDNS, Dynamic Domain Name System los cuales son efectivos a la hora de reflejar los cambios a tu IP dinámica.

## ¿Qué es ICANN?
ICANN es una organización que opera a nivel multinacional/internacional y es la responsable de asignar las direcciones del protocolo IP, de los identificadores de protocolo, de las funciones de gestión del sistema de dominio y de la administración del sistema de servidores raíz.

# CRON

# ¿Qué es Cron?
Es un administrador de procesos en segundo plano que se ejecuta cuando arranca el sistema.

# ¿Qué es Crontab?
Es un archivo de texto que contiene la lista de comandos que se deben ejecutar. Crontab verificará la fecha y hora en que se debe ejecutar el script o comando, los permisos de ejecución y lo ejecutará en segundo plano.

# Ejemplos

![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_1.PNG)
Cada y media se ejecuta
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_2.PNG)
A las 20.30 h se ejecuta
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_3.PNG)
A las 20.30 de Lunes a Viernes se ejecuta
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_4.PNG)
A las 20.30 los Martes y Jueves
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_5.PNG)
A las 20.30 h los días 10 y 20 de cada mes
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_6.PNG)
Cada 15 min se ejecuta
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_7.PNG)
Se ejecuta diariamente
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_8.PNG)
Se ejecuta mensualmente * @monthly
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_9.PNG)
A las 20.30 h de Lunes a Viernes
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/6_10.PNG)
A las 00.01 los días del 1 al 7 de cada mes


# Ejercicio 7

1. Ejecutamos ``crontab -e`` y seleccionamos nuestro editor preferido
2. Al final del archivo añadimos una línea especificando cuando queremos que se ejecute él o los comandos
![](https://raw.githubusercontent.com/passwdIsBlank/DWEB_AWS/main/images/7_1.PNG)