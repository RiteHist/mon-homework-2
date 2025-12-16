# mon-homework-2

Для выполнения домашнего задания создано два compose файла: [compose.yaml](https://github.com/RiteHist/mon-homework-2/blob/main/prometheus-grafana/compose.yaml), который создает связку prometheus+grafana, и [node_exporter.yaml](https://github.com/RiteHist/mon-homework-2/blob/main/prometheus-grafana/node_exporter.yaml), который запускает node exporter на удаленной машине.

Конфигурационные файлы для Prometheus лежат по пути [prometheus-grafana/prom-conf](https://github.com/RiteHist/mon-homework-2/tree/main/prometheus-grafana/prom-conf), хосты для мониторинга добавляются через service discovery на основе файла node_targets.yml. [Пример файла](https://github.com/RiteHist/mon-homework-2/blob/main/prometheus-grafana/prom-conf/node_targets.yml.example). На node exporter также настраивается базовая аутенфикация через файл web-conf.yml. [Пример файла](https://github.com/RiteHist/mon-homework-2/blob/main/prometheus-grafana/web-conf/web-conf.yml.example). Для корректной загрузки метрик с node exporter, в директорию prometheus-grafana/prom-conf должны быть добавлены файлы username и password, содержащие в себе имя пользователя и пароль, заданные в файле web-conf.yml.

## Задание 1

Окно Data sources в Grafana:

![alt text](https://github.com/ritehist/mon-homework-2/blob/main/media/1.PNG?raw=true)

## Задание 2

PromQL запросы:

- Утилизация CPU:
   `clamp_min(100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100), 0)`

- CPULA:
  `(sum by (instance) (node_load1) / count by (instance) (node_cpu_seconds_total{mode="idle"})) * 100`
  `(sum by (instance) (node_load5) / count by (instance) (node_cpu_seconds_total{mode="idle"})) * 100`
  `(sum by (instance) (node_load15) / count by (instance) (node_cpu_seconds_total{mode="idle"})) * 100`

- Свободная память:
  `node_memory_MemAvailable_bytes / (1024^3)`

- Количество места на файловой системе:
  `node_filesystem_avail_bytes{mountpoint="/"} / (1024^3)`

Получившийся дашборд:

![alt text](https://github.com/ritehist/mon-homework-2/blob/main/media/2.PNG?raw=true)

## Задание 3

Созданные правила alerts для панелей:

![alt text](https://github.com/ritehist/mon-homework-2/blob/main/media/3.PNG?raw=true)

Полученные тестовые alerts при настроенном Telegram в качестве канала связи:

![alt text](https://github.com/ritehist/mon-homework-2/blob/main/media/4.PNG?raw=true)

Внешний вид дашборда после добавления alerts:

![alt text](https://github.com/ritehist/mon-homework-2/blob/main/media/5.PNG?raw=true)

## Задание 4

[JSON файл дашборда](https://github.com/RiteHist/mon-homework-2/blob/main/dashboard.json)