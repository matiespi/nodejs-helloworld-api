# Pasos para poder ejecutar pipeline:

Darles permisos a jenkins para ejecutar sudo sin passwd:

## Instalar NGROK y exponer el puerto 8080

1. curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
	| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
	&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
	| sudo tee /etc/apt/sources.list.d/ngrok.list \
	&& sudo apt update \
	&& sudo apt install ngrok
2. ngrok config add-authtoken 2eqDy4Fo9uJbyOg3s7rAXk49J16_3kWPLSpTWS87MVQyGpwwR

3. ngrok http http://localhost:8080
   Si no funciona, tambien se puede ejecutar: ngrok tcp 8080
   
## Crear webhook en github para repositorio:
Ya estando en el repositorio, ir a "Settings" - Seccion "WebHooks"
- "Add WebHooks"
  En "Payload URL": Agregar url provista por ngrok, ej: http://2.tcp.ngrok.io:19110/github-webhook/
     "Content type": apllication/json
Seleccionar "Let me select individual events" - Tildar: Pull requests y Pushes

(paso adicional)
En "Settings" - Seccion "Deploy keys" agregamos una key para que el usuario de jenkins pueda comunicarse con github.
En la maquina donde esta instalado jenkins, entrar con "sudo su -s /bin/bash jenkins", ejecutar ssh-keygen 
Cuando pida contraseña darle enter dos veces.
Cuando se cree, ejecutar: cat /var/lib/jenkins/.ssh/id_rsa.pub para ver la key generada, copiarla y pegarla en Seccion "Deploy keys" y darle en "Allow write access"

## Configuracion de job en Jenkins

Cuando se este creando el job, en "Build Triggers", tildar: "GitHub hook trigger for GITScm polling" y seguir la configuracion normal del job.

