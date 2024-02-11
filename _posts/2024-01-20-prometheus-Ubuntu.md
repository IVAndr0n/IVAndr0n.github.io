---
layout: post
title: Установка Prometheus на Ubuntu с веб-сайта разработчика
date: 2024-01-20
categories:
  - monitoring
---

<!-- # Установка **Prometheus** на **Ubuntu** с веб-сайта разработчика -->

> **Примечание.** *Установку можно выполнить посредством команды 'sudo apt install -y prometheus', но в этом случае устанавливается устаревшая версия программы.*

## Шаг 1: Подготовка системы

Обновите список пакетов и установите необходимые зависимости:

```sh
sudo apt update
sudo apt install -y wget tar vim
```

Создайте системную учетную запись группы и пользователя:

```sh
sudo groupadd --system prometheus
sudo useradd --system --shell /usr/sbin/nologin --gid prometheus prometheus
```

## Шаг 2: Загрузка программы

Установку программы из архива выполняем в каталоге для временных файлов.

```sh
mkdir /tmp/prometheus && cd /tmp/prometheus
```

> **Примечание.** *Каталог '/tmp/' очищается автоматически при перезагрузке системы, кроме того, созданный в '/tmp/' каталог можно удалить вручную после установки программы.*

Архив программы, можно скачать любым методом:

:one: Продвинутый, используя только консоль

```sh
curl -s https://api.github.com/repos/prometheus/prometheus/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
```

:two: Обычный, используя браузер и консоль

Откройте в браузере веб-сайт разработчика <a target="_blank" rel="noopener noreferrer" href="https://prometheus.io/download/#prometheus">Prometheus</a> и скопируйте ссылку на последний релиз для linux в формате tar.gz, добавьте ссылку к wget. Пример:

```sh
wget https://github.com/prometheus/prometheus/releases/download/v${VERSION}/prometheus-${VERSION}.linux-amd64.tar.gz
```

Распакуйте архив:

```sh
tar -vxzf prometheus*.tar.gz
```

Перейдите в каталог с распакованными файлами:

```sh
cd prometheus*/
```

## Шаг 3: Установка программы

Создайте каталоги для данных и конфигурации:

```sh
sudo mkdir /var/lib/prometheus
sudo mkdir /etc/prometheus
```

Скопируйте файлы программы и конфигурационный файл, настройте права безопасности:

```sh
sudo cp prometheus /usr/local/bin
sudo cp promtool /usr/local/bin
sudo cp -r console* /etc/prometheus
sudo cp prometheus.yml /etc/prometheus

sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo chown prometheus:prometheus /var/lib/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus
```

## Шаг 4: Настройка автоматического запуска программы

Создайте unit-файл для systemd:

```sh
sudo tee /etc/systemd/system/prometheus.service<<EOF
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP \$MAINPID
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090 \
    --web.external-url=

SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

> **Примечание.** *Параметр 'web.listen-address=0.0.0.0:9090' можно не использовать, но он пригодится, если необходимо изменить порт, заданный по умолчанию.*

Запустите службу и проверьте её статус:

```sh
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus
```

Если статус службы **active (running)** добавьте ее в автозагрузку:

```sh
sudo systemctl enable prometheus
```

## Шаг 5: Настройка доступа к веб-интерфейсу программы

По умолчанию **Prometheus** работает на порту 9090, если вы используете брандмауэр, необходимо разрешить этот порт.

```sh
sudo ufw allow 9090/tcp
sudo ufw reload
```

## Шаг 6: Контрольная проверка

Откройте в браузере веб-интерфейс **Prometheus**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:9090'*.

Дополнительные способы проверить запущен ли **Prometheus**:

```sh
sudo ss -pnltu | grep 9090
curl http://FQDN_or_IP_Your_Server:9090 | grep "Found"
```

Установка завершена!
