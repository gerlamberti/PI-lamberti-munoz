## Filebeat

### Instalación

A continuación se describe el procedimiento para instalar el agente Filebeat.

Descargue y agregue la clave de firma pública de Elasticsearch para garantizar la autenticidad de los paquetes:

`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`

Instale los paquetes necesarios para permitir el uso del protocolo HTTPS con APT:

`sudo apt-get install apt-transport-https`

Guarde la definición del repositorio Filebeat:

`echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/ apt/sources.list.d/elastic-7.x.list`

Actualice los paquetes e instale el agente:

`sudo apt-get update && sudo apt-get install filebeat`

### Configuración del servicio

Para que Filebeat se ejecute automáticamente en cada arranque del sistema, configure el servicio de la siguiente manera:

`sudo systemctl daemon-reload` <br>
`sudo systemctl enable filebeat.service`