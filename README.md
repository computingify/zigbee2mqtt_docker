# Zigbee2mqtt running in a docker container
## To launch docker
'''sudo systemctl start docker.service'''

## To start zigbee2mqtt container
'''cd /opt/zigbee2mqtt_maison2/docker'''
'''sudo docker-compose up -d'''

## See docker logs
'''sudo docker logs zigbee2mqtt'''

## Identify docker images running
'''sudo docker ps'''

## Connect to docker running image
'''sudo docker exec -it zigbee2mqtt /bin/sh'''

## See the docker config
'''sudo docker inspect zigbee2mqtt'''

## Stop docker
'''sudo docker-compose down'''

## To made it work (issue facing at the first launch
Do never call Topic with the same name as another zigbee concentrator
Exchange frontend zigbee2mqtt config by:
'''sudo vim /otp/zigbee2mqtt/data/configuration.yaml'''
inside this file change:
'''frontend: true'''
by
'''frontend:
  enable: true
  port: 8081'''
reload docker container by:
'''sudo docker-compose down'''
'''sudo docker-compose up'''

## Docker contain start at boot
Create systemd service:
'''sudo vim /etc/systemd/system/docker-compose-zigbee2mqtt_maison2.service'''
Add the following content:
'''
[Unit]
Description=Docker Compose Zigbee2MQTT for Maison2 concentrator Service
Requires=docker.service
After=docker.service

[Service]
WorkingDirectory=/opt/zigbee2mqtt_maison2/docker
ExecStart=docker-compose up
ExecStop=docker-compose down
TimeoutStartSec=0
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
'''

Reload systemd and enable the service:
'''
sudo systemctl daemon-reload
sudo systemctl enable docker-compose-zigbee2mqtt_maison2.service
'''

Start the service:
'''sudo systemctl start docker-compose-zigbee2mqtt_maison2.service'''

# Debug Zigbee2mqtt
## See mqtt message:
'''mosquitto_sub -t zigbee2mqtt/#'''
'''mosquitto_sub -t zigbee2mqtt_maison2/#'''

# Raspberry pi Zigbee2mqtt instence
## sometime it won't start because of logs access:
check in logs:
'''journalctl -afu zigbee2mqtt.service'''
'''sudo rm -rf /opt/zigbee2mqtt/data/log/'''

## Check if the zigbee2mqtt is running
'''journalctl -afu zigbee2mqtt.service'''
