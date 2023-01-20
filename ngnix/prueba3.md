# INTEGRACION Y DESPLIEGUE CONTINUO (CI/CD)

En primer lugar utilice, GitHub Actions (plataforma de integración y despliegue continuos (IC/DC) que permite automatizar el mapa de compilación, pruebas y despliegue). Donde cree el flujo de trabajo (proceso automatizado configurable formado por uno o más trabajos) y cree un archivo YAML para definir la configuracion del mismo.

El flujo se almacena en el repositorio de codigo, en un directorio llamado .github/workflows.

Para crearlo segui los siguientes pasos: 

*1.Cree un directorio llamado workflow-templates.*

*2.Cree el nuevo archivo de flujo de trabajo dentro del directorio workflow-templates.*

*3.Cree un archivo .yml dentro del directorio workflow-templates.*

## Luego para ejecutar:

docker-compose build

docker-compose up

docker run -p 8080:80 {{ secrets.DOCKERHUB_USERNAME }}/devops-nginx:tagname

y por ultimo abrir localhost:8080.
