##Imagen a utilizar como sistema operativo de fondo para mis contenedores
FROM node:15.13-alpine

##Carpeta donde voy a colocar mi app 
WORKDIR /deployment

##Hacer una copia de todas las carpetas
COPY . . 

##Ejecutar NPM
RUN npm install 

##Puerto para que corra la app
EXPOSE 3000

##Start-server
CMD ["npm","start"]
