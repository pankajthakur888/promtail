# promtail
monitoring
sudo –s 

mkdir /opt/shipment 

chown –R ubuntu:ubuntu /opt/shipment 

exit 

vi ship-logs-config.yaml 

############################################################################ 
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://10.0.3.137:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - mailer-server
    labels:
      job: mailer-logs
      __path__: /var/log/mail.log
############################################################################ 

 

curl -O -L https://github.com/grafana/loki/releases/download/v2.4.1/promtail-linux-amd64.zip 

 

unzip "promtail-linux-amd64.zip" 

sudo chmod a+x "promtail-linux-amd64" 

###To RUN and check connection 

/opt/shipment/promtail-linux-amd64 -config.file /opt/shipment/ship-logs-config.yaml  

###To RUN automatically  

vi /etc/systemd/system/promtail.service 

############################################################################ 
[Unit] 
Description=Promtail service 
After=network.target 

  

[Service] 
Type=simple 
ExecStart=/opt/shipment/promtail-linux-amd64 -config.file /opt/shipment/ship-logs-config.yaml 
User=root 
Group=root 
Restart=always 

[Install] 
WantedBy=multi-user.target 
############################################################################ 


sudo systemctl enable promtail.service
sudo systemctl start promtail.service
sudo systemctl status promtail.service
