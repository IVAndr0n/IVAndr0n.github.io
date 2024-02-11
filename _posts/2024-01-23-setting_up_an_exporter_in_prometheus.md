---
layout: post
title: Настройка Prometheus для мониторинга клиентских узлов
date: 2024-01-23
categories:
  - monitoring
---

<!-- # Настройка **Prometheus** для мониторинга клиентских узлов -->

## Шаг 1: Настройка конфигурационного файла программы

Внесите изменения в файл конфигурации:

```sh
sudo vim /etc/prometheus/prometheus.yml
```

Добавьте в конце файла блок данных узла клиента (формат отступов важен!):

```sh
  - job_name: "Name_Your_Server"
    static_configs:
      - targets: ["FQDN_or_IP_Your_Server:9100"]
```

## Шаг 2: Применение изменений

Перезапустите службу **Prometheus**:

```sh
sudo systemctl restart prometheus.service
```

## Шаг 3: Контрольная проверка подключения программы к клиентам

Откройте в браузере веб-интерфейс **Prometheus**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:9090/graph'*.

В строке поиска наберите *up* и нажмите **Execute**, должна появиться строка *up{instance="localhost:9100", job="Name_Your_Server"}* со значением 1.
Помимо этого, подключение экземпляра отображается в разделе 'Status\Targets'.

Установка завершена!
