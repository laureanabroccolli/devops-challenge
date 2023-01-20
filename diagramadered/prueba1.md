# DIAGRAMA DE RED

Nos entrcontramos dentro de la nube de AWS, con una puerta de enlace de internet hacia VPC (Virtual Private Cloud) que es nuestro servicio principal donde armaremos el diagrama de la aplicacion.

Nos pide alta disponibilidad, para eso es indispensable tener mas de una zona de disponibilidad y utilizamos un balanceador de carga (load balancer: se utuliza para recibi las solicitudes de los usuarios que quieran acceder al frontend y distribuirlas entre las maquinas EC2 creadas) para acceder a ellos.

En estas zonas de disponibiilidad utilizamos Auto Scaling para crear planes de escalado que automatizan la manera en la que diferentes recursos responden ante los cambios que se producen en la demanda.

Luego nos encontramos con dos redes publicas (ya que las maquinas EC2 necesitan estar en una red para que se les asigne una IP y para que el frontend pueda ser accesible para los usuarios) una para cada zona de disponibilidad ya que se encuentran en lugares fisicos diferentes.

Con respecto al backend solicitaba una base de dato relacional y una no relacional, entonces:

**DynamoDB:** Base de datos no relacional (no-sql), super-rápida y auto-escalable.

**Aurora:** Base de datos relacional, hasta cinco veces más rápida que las bases de datos de MySQL estándar y tres veces más rápida que las bases de datos de PostgreSQL estándar. 

Por ultimo, nos piden para el backend dos microservicios externos, es decir fuera de la nube. Conectandose a ellos mediante internet Gateway, accediendo a una red de internet. 
