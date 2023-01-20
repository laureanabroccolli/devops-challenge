# DIAGRAMA DE RED

Primeramente volque todo dentro de la nube de AWS, con una puerta de enlace de internet hacia VPC (Virtual Private Cloud) que es nuestro servicio principal donde armaremos el diagrama de la aplicacion.

Para hacer el diagrama utilice recursos de EC2 que son maquinas virtuales en la nube elásticas y escalables de computo general y alto rendimiento.

Nos pide cargas variables y alta disponibilidad. Para eso usamos primeramente un balenceador de carga (load balancer: se utuliza para recibi las solicitudes de los usuarios que quieran acceder al frontend y distribuirlas entre las maquinas EC2 creadas)  y para alta disponibilidad es indispensable disponer de mas de una zona.
Con el balanceador de cargas accedemos a cada zona de disponibilidad.

En estas zonas de disponibiilidad utilizamos Auto Scaling para crear planes de escalado que automatizan la manera en la que diferentes recursos responden ante los cambios que se producen en la demanda.

Luego coloque dos redes publicas (ya que las maquinas EC2 necesitan estar en una red para que se les asigne una IP y para que el frontend pueda ser accesible para los usuarios) una para cada zona de disponibilidad ya que se encuentran en lugares fisicos diferentes.

Estas soportan JS, que es lo pedido inicialmente.

Con respecto al backend solicitaba una base de dato relacional y una no relacional, entonces utilice:

**DynamoDB:** Base de datos no relacional (no-sql), super-rápida y auto-escalable.

**Aurora:** Base de datos relacional, hasta cinco veces más rápida que las bases de datos de MySQL estándar y tres veces más rápida que las bases de datos de PostgreSQL estándar. 

Por ultimo, nos piden para el backend dos microservicios externos, es decir fuera de la nube. Conectandose a ellos mediante internet Gateway, accediendo a una red de internet. 
