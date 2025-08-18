## Elasticseach

### Instalación

A continuación se describe el procedimiento para instalar Elasticseach.

Descargue y agregue la clave de firma pública de Elasticsearch para garantizar la autenticidad de los paquetes:

`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /
usr/share/keyrings/elasticsearch-keyring.gpg`

Instale los paquetes necesarios para permitir el uso del protocolo HTTPS con APT:

`sudo apt-get install apt-transport-https`

Agregue el repositorio oficial de Elastic para la versión 7.x:

`echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.
elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic
-7.x.list`

Actualice los paquetes e instale Elasticseach:

`sudo apt-get update && sudo apt-get install elasticsearch`

### Configuración del servicio

Para que Filebeat se ejecute automáticamente en cada arranque del sistema, configure el servicio de la siguiente manera:

`sudo systemctl daemon-reload` <br>
`sudo /bin/systemctl enable elasticsearch.service`