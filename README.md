# Despliegue Nodejs
despliegueNodejs en servidores con MongoDB , ssl 


Despliegue aplicación web de Nodejs con Mondo DB desde Github a servidor Linux
Ya con el archivo funcional desde el código hacer:
1.	Script para el package.json

"scripts": {
    "server": "babel-node src",
    "dev": "nodemon --exec npm run server",
    "build": "babel src --out-dir dist",
    "start": "node dist/index.js"
  },

2.	Definir archivos .gintignore
Crear .girtignore y 


 .env , node_modules, dist
 
GIT 
- git --version
3.	Crear repositorio GIT 
-	 git init -y
-	git status
-	git add .
-	git status
-	git commit -m “despliegue de la aplicación V1 subida”
-	git remote add origin https://github.com/MercaConMercado/resapi-tareas.git (ruta donde está el repositorio de git hub)
-	git push origin master

 OTROS git
- git pull (trae todos los cambios hechos)
- git clone (trae todo el repositorio)
- git config --global user.email "decodermc@gmail.com"
- git config --global user.name "MercaConMercado"
- git commit
- i , agregar comentario , :wq!
- git log
- git checkout -- style.css (devolver cambios al original)
- git diff style.css (ver diferencias de lo que se agrega al archivo)
- git branch (ubicacion de codigo siempre en master)
- git branch version2 (crea otra version alternativa)
- git checkout version2 (acceso a la version alternativa)


CONFIGURACION SERVIDOR
- hostnamectl
4.	Agregar repositorio del proyecto al servidor
-	sudo apt update , sudo apt upgrade
-	nuevo usuario: 
    -	adduser soporte
    -	usermod –aG sudo soporte (agregar privilegios de usuario sudo a el nuevo creado)
-	pwd
-	git clone https://github.com/MercaConMercado/resapi-tareas.git (clona repositorio git, mirar que tenga git instalado)
-	ingresar al repositorio del proyecto en el servidor

NODE
5.	Instalar Node
-	Usar NVM para instalar 
-	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
-	source ~/.bashrc
-	nvm
-	nvm install node –lts
-	node --version, npm –versión
-	npm install (dentro del repositorio del proyecto ya instala node_module y los .json)
-	mongo (accede a la base de mongodb para ver db,colleciones y documentos)

MONGO DB
- prefenrencia: WINDOWS agregar path con C:\Program Files\nodejs\
6.	Instalar MongoDB Community ( dependiendo de la versión de Linux)
-	wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add –
-	echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list  (para versión Linux Ubuntu 16.04 (Xenial) )
-	sudo apt-get update
-	sudo apt-get install -y mongodb-org
-	mongod --version
-	sudo systemctl status mongod
-	sudo systemctl start mongod
-	sudo systemctl status mongod
-	mongodb crea y define la base de datos ya definida en el código en el archivo .env

INICIAR PROYECTO
7.	Proceso de configuración e inicialización	
-	npm run dev ( ya con esto se puede acceder a la aplicación 192.168.0.113:5000 )
-	npm install pm2 -g (paquete para mantener ejecutando el proyecto)
-	npm run build (crea el repositorio dis)
-	node dist/index.js (comprobamos el servicio con el node que inicializa)
-	pm2 start dist/index.js (este mantiene ejecutando siempre)

CONFIGURAR URL DEL PROYECTO
Conseguir Dominios en: www.namecheap.com
hostname -i (saber el hostname)
ngonx ( SERVIDOR QUE REDIRECCIONA, PROXY SERVER)
-   sudp apt i nginx
-   cd /etc/nginx/sites-available (entrar archivo de configuracion)
-   nano resapi-tareas
    agregar :
    
 // aqui el direccionamiento que queremos que redireccione cunado abran las url www.mercaconmercado.com api.mercaconmercado.com ... etc
<pre><code>  
server {
        listen 80; 
        server_name 192.168.0.113; 

        location / {
                proxy_pass http://localhost:5000; 
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}
</pre></code>  

-   sudo nginx -t (mirar si quedo bien la sintaxis de arriba)
-   sudo ln -s /etc/nginx/sites-available/resapi-tareas /etc/nginx/sites-enabled/resapi-tareas (copear archivo de configuracion a el ya habilitado)
-   sudo systemctl restart nginx
-   sudo systemctl status nginx 

AÑADIR SSL AL PROYECTO
cerbot (añade a un dominio SSL gratuito)
https://certbot.eff.org/

-   sudo snap install --classic certbot
-   sudo certbot --nginx -d mercaconmercado.com -d api.mercaconmercado.com (agregar el dominio donde esta ubicado y se debe hacer con cada dominio y subdominio)


MODULOS NODE JS instalacion:<br>

Dependencies
- npm i jsdoc // documentador de codigo JS
-

devDependencies
- npm i nodemon -D // en desarrollo para que quede ejecutando arrancador sin reiniciar


COSITAS PARA JS.
- Revale JS // https://revealjs.com/ // presentation framework
- Electron // https://www.electronjs.org/ // crear app de escritorio con js
- MOTORES DE VIDEOJUEGOS: 
    - GDEVELOP // https://gdevelop-app.com/ 
    - MELONJS // https://melonjs.org/
    - IMPACT // https://impactjs.com/
    - babylon // https://impactjs.com/
    - pixijs // https://www.pixijs.com/
    - pasher // https://phaser.io/
    - PLAY CANVAS // https://playcanvas.com/
- 3D api WebGL
    - threejs // https://threejs.org/
- Api aduio 
    - tone js // https://tonejs.github.io/
    - howlerjs // https://howlerjs.com/
- Animaciones 
    - greensock // https://greensock.com/
- HARDWARE
    - johnny-five. // http://johnny-five.io/
    - node-red // https://nodered.org/  
    - iot js // https://iotjs.net/
- APP MOVIL
    - ionic // https://ionicframework.com/
    - reactnative // https://reactnative.dev/

