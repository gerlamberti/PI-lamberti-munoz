## Kibana

### Instalación

Importe la clave de firma pública de Elastic para garantizar la autenticidad de los paquetes instalados. Ejecute el siguiente comando:

`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o / usr/share/keyrings/elasticsearch-keyring.gpg`

Instale el paquete `apt-transport-https`

`sudo apt-get install apt-transport-https`

Agregue la definición del repositorio oficial de Elastic:

`echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts. elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic -9.x.list`

Actualice los repositorios e instale kibana:

`sudo apt-get update && sudo apt-get install kibana`

### Habilitación y ejecución del servicio Kibana

Configure el servicio Kibana para que se inicie automáticamente al arrancar el sistema, ejecute los siguientes comandos:

`sudo /bin/systemctl daemon-reload` <br>
`sudo /bin/systemctl enable kibana.service`

El servicio puede iniciarse y detenerse al ejecutar:

`sudo systemctl start kibana.service` <br>
`sudo systemctl stop kibana.service`