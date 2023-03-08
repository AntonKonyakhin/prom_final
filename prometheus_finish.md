### PROM FINAL: Финальное задание

 - node1:

docker, docker-compose установлены, запущен wordpress с помощью docker-compose.yaml - файла

![wordpress](/images/wordpress.jpg "")

cadvisor запустил, как контейнер:

![cadvisor](/images/cadvisor.jpg "")

доступен по порту 8080
![cadvisor](/images/cadvisor_2.jpg "")


локально установлены mysqld_exporter, node_expoter, запусщены как сервисы через systemd 

![services](/images/services.jpg "")

используются порты по умоляанию для mysqld_exporter, node_expoter 9104 и 9100 соответственно

опубликован порт 3306 из контейнера с mysqld, чтобы exporter смог до него добраться с локальной машины.

- prometheus:

prometheus установлен локально и запущен как сервис 

![services](/images/prometheus.jpg "")


grafana установлена локально и запущена как сервис 

![services](/images/grafana.jpg "")



![services](/images/prometheus-grafana.jpg "")



конфиг prometheus:
```
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093
		  
# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: 'cadvisor'
    scrape_interval: 10s
    # metrics_path: '/metrics'
    static_configs:
      - targets: ['206.189.6.14:8080']

  - job_name: 'node'
    scrape_interval: 10s
    static_configs:
      - targets: ['206.189.6.14:9100']

  - job_name: 'mysql'
    scrape_interval: 10s
    # metrics_path: '/metrics'
    static_configs:
      - targets: ['206.189.6.14:9104']
```


добавлены dashboard в grafana:

![dashboards](/images/dashboards.jpg "")


node_exporter:
![dashboards](/images/node_exporter_dashboard.jpg "")

mysql_dashboard:
![dashboards](/images/mysql_dashboard.jpg "")

cadvisor_dashboard:
![dashboards](/images/cadvisor_dashboard.jpg "")





