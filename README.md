# despliegueNodejs
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
Crear .girtignore y agregar 
 .env , node_modules, dist
 
GIT 
- git --version
3.	Crear repositorio GIT 
-	 git init
-	git status
-	git add .
-	git status
-	git commit –m “despliegue de la aplicación V1 subida”
-	git remote add origin https://github.com/MercaConMercado/resapi-tareas.git (ruta donde está el repositorio de git hub)
-	git push origin master

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
    
server {
        listen 80; // puerto
        server_name 192.168.0.113; // aqui el direccionamiento que queremos que redireccione cunado abran las url www.mercaconmercado.com api.mercaconmercado.com ... etc

        location / {
                proxy_pass http://localhost:5000; // 
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}

-   sudo nginx -t (mirar si quedo bien la sintaxis de arriba)
-   sudo ln -s /etc/nginx/sites-available/resapi-tareas /etc/nginx/sites-enabled/resapi-tareas (copear archivo de configuracion a el ya habilitado)
-   sudo systemctl restart nginx
-   sudo systemctl status nginx 

AÑADIR SSL AL PROYECTO
cerbot (añade a un dominio SSL gratuito)
https://certbot.eff.org/

-   sudo snap install --classic certbot
-   sudo certbot --nginx -d mercaconmercado.com -d api.mercaconmercado.com (agregar el dominio donde esta ubicado y se debe hacer con cada dominio y subdominio)

