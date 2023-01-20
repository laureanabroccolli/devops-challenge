# DESPLIEGUE LOCAL

Inicie creando dos archivos Dockerfile, tanto para backend como para frontend (ambos en su carpeta correspondiente), los cuales incluyen una serie de instrucciones que se necesitan ejecutar de manera consecutiva para cumplir con los procesos necesarios para la creación de una nueva imagen.

Luego use Docker-Compose que es una herramienta para definir y ejecutar aplicaciones de Docker de varios contenedores. Cree un archivo YAML para configurar los servicios de la aplicacion (en este caso backend y frontend).

Para ejecutar la app use Docker-Compose up, donde me encontre con algunas complicaciones, como por ejemplo un error en el archivo requierements.txt, en estos tres requerimientos: psycopg2, psycopg2-binary y pygraphviz. Los primeros dos los pude solucionar colocandolos en el dockerfile correspondiente (el del backend) y el ultimo no pude soluciar el error, pero cuando lo quite de la lista, corrio correctamente.
![image](https://user-images.githubusercontent.com/100167774/213609296-5fa2b982-a7d7-4c85-8831-26f06e6d2919.png)
Y el ultimo error que tuve, que mas que un error fue un aviso, fue con respecto a crear una SECRET_KEY, en este caso coloque el valor en el archivo, pero en realidad no deberia estar expuesta. Deberia estar guardada en la maquina para que docker-compose vaya a buscarla y en ENV la tengo que dejar vacia.
![image](https://user-images.githubusercontent.com/100167774/213609543-b6abc2b9-5fff-4599-9cbd-7420cc14616f.png)

Y por ultimo, luego de agregar el los archivos Dockerfile, el archivo Docker-Compose.yml y de solucionar los errores presentados, con el comando Docker-Compose up pude ejecutar la app. 
Ingresando al LocalHost desde mi navegador la pude visualizar.

# DESPLIEGUE AWS (plus, a través de un botón)

### 1. Desarrollar un boilerplate de la aplicación Django y React.js. (Ya hecho)

### 2. Configurar docker-compose. (Ya hecho)

### 3. Crear una instancia de EC2.

Para ello ingresamos a AWS y en EC2 se pueden crear instancias, creamos una, elegimos como imagen base Amazon Linux 2 AMI (HVM), SSD Volume Type y seleccionamos el tipo de instancia, dejamos los detalles de instancias predeterminados y el almacenamiento predeterminado, las etiquetas son opcionales, luego se configura el grupo de seguridad de la instancia y lanzamos la instancia.

Creamos una nueva llave de seguridad, esta es la que permite el acceso a la instancia. Y ya tenemos nuestra instancia corriendo en instancias de EC2.

(Para utilizar la llave de seguridad MyLinuxKP.pem se deben configurar su propietario con:
chown 400 MyLinuxKP.pem)

### 4. Copiar los archivos de proyecto a la instancia de EC2 y configurar instancia para docker-compose

Luego se copian los archivos del boilerplate del app del disco local a la instancia de EC2, usando su dirección IP pública. Despues de copiar los archivos a la instancia, se accede a ella a través de ssh con el siguiente comando:
ssh ec2-user@54.89.125.233 -i MyLinuxKP.pem

Ahora dentro de la instancia de EC2 se corren los siguientes 5 comandos:

 • sudo yum update

 • sudo yum install docker

 • sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

 • sudo chmod +x /usr/local/bin/docker-compose

 • sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

 Los comandos anteriores actualizan dependencias en la instancia, instalan docker y docker-compose.

 Para iniciar el servicio de docker en la instancia se usa:

 • sudo service docker start

 • Para desplegar los contenedores se usa:

 • sudo docker-compose up --build

Se puede verificar el funcionamiento correcto del boilerplate al acceder a la instancia a través de los puertos de desarrollo

### 5. Crear una imagen (AMI) de la instancia para replicarla.

Aqui se crea una imagen de máquina de amazon (AMI) con base en la instancia anterior para automatizar su replicación. Para ello se accede al dashboard de instancias de EC2 y se crea una imagen a partir de la instancia en cuestión:

Instancia > Acciones > Imagen > Crear Imagen

Ahora se tiene una imagen de la instancia que se creó como boilerplate. Con base en esta imagen se pueden generar instancias identicas a la original en el momento de captura de la imagen.

Para lanzar una instancia a partir de la imagen desde la consola de AWS se selecciona la imagen como imagen base de la instancia.

Esto es, la creación de la instancia boilerplate a partir de la imágen es lo que se busca automatizar con un botón.

### 6. Desarrollar interfaz (botón) para automatizar despliegue de instancia basada en imagen.

Para desarrollar un botón en JavaScript para automatizar el despliegue de una instancia basada en una imagen de boilerplate, el primer paso es instalar el SDK de AWS para JavaScript:
npm install -save aws-sdk

Segundo, se tiene un script provisto por Amazon (https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/ec2-example-creating-an-instance.html) para crear instancias de EC2 usando el SDK de AWS para JavaScript.

En ese script se cambian las constantes: 'AMI_ID', 'KEY_PAIR_NAME' y 'SDK Sample'.

El id del ami se obtiene en la consola de AWS navegando EC2 > IMAGES (AMIs) > Seleccionar AMI deseada > AMI ID

El 'KEY_PAIR_NAME' es el nombre de las credenciales que se usaran para acceder a la instancia, en este caso se tiene MyLinuxKP.pem

El 'SDK Sample' se cambia por el nombre que se le quiera poner a la instancia.

Finalmente, se añade la función al onClick de un botón en React.




Y asi es como automatizamos con un botón el despliegue de instancias de EC2 con el código base y la configuración (boilerplate) para desarrollar aplicaciones y ejecutarlas usando docker-compose.

