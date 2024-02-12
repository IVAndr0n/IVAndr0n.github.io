---
layout: post
title: Установка node_exporter на Ubuntu с веб-сайта разработчика
date: 2024-01-21
categories:
  - monitoring
---

<!-- # Установка **node_exporter** на **Ubuntu** с веб-сайта разработчика -->

## Шаг 1: Подготовка системы

Обновите список пакетов и установите необходимые зависимости:

```sh
sudo apt update
sudo apt install -y wget tar
```

Создайте системную учетную запись группы и пользователя:

```sh
sudo groupadd --system node_exporter
sudo useradd --system --shell /usr/sbin/nologin --gid node_exporter node_exporter
```

## Шаг 2: Загрузка программы

Установку программы из архива выполняем в каталоге для временных файлов.

```sh
mkdir /tmp/node_exporter && cd /tmp/node_exporter
```

> **Примечание.** *Каталог '/tmp/' очищается автоматически при перезагрузке системы, кроме того, созданный в '/tmp/' каталог можно удалить вручную после установки программы.*

Архив программы, можно скачать любым методом:

:one: Продвинутый, используя только консоль

```sh
curl -s https://api.github.com/repos/prometheus/node_exporter/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
```

:two: Обычный, используя браузер и консоль

Откройте в браузере веб-сайт разработчика [node_exporter](https://prometheus.io/download/#node_exporter) и скопируйте ссылку на последний релиз для linux в формате tar.gz, добавьте ссылку к wget. Пример:

```sh
wget https://github.com/prometheus/node_exporter/releases/download/v${VERSION}/node_exporter-${VERSION}.linux-amd64.tar.gz
```

Распакуйте архив:

```sh
tar -vxzf node_exporter*.tar.gz
```

Перейдите в каталог с распакованными файлами:

```sh
cd node_exporter*/
```

## Шаг 3: Установка программы

Скопируйте исполняемый файл программы и настройте права безопасности:

```sh
sudo cp node_exporter /usr/local/bin

sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

## Шаг 4: Настройка автоматического запуска программы

Создайте unit-файл для systemd:

```sh
sudo tee /etc/systemd/system/node_exporter.service<<EOF
[Unit]
Description=Prometheus Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=node_exporter
Group=node_exporter
ExecReload=/bin/kill -HUP \$MAINPID
ExecStart=/usr/local/bin/node_exporter \
    --web.listen-address=0.0.0.0:9100

SyslogIdentifier=node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

> **Примечание.** *Параметр 'web.listen-address=0.0.0.0:9100' можно не использовать, но он пригодится, если необходимо изменить порт, заданный по умолчанию.*

Запустите службу и проверьте её статус:

```sh
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

Если статус службы **active (running)** добавьте ее в автозагрузку:

```sh
sudo systemctl enable node_exporter
```

## Шаг 5: Настройка доступа к веб-интерфейсу программы

По умолчанию **node_exporter** работает на порту 9100, если вы используете брандмауэр, необходимо разрешить этот порт.

```sh
sudo ufw allow 9100/tcp
sudo ufw reload
```

## Шаг 6: Контрольная проверка

Откройте в браузере веб-интерфейс **node_exporter**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:9100/metrics'*.

Дополнительные способы проверить запущен ли **node_exporter**:

```sh
sudo ss -pnltu | grep 9100
curl http://FQDN_or_IP_Your_Server:9100 | grep "Node Exporter"
```

Установка завершена!
