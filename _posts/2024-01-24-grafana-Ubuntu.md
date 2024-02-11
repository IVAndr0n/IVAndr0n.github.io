---
layout: post
title: Установка Grafana OSS на Ubuntu
date: 2024-01-24
categories:
  - monitoring
---

<!-- # Установка **Grafana OSS** на **Ubuntu** -->

> **Примечание.**
> *В связи с санкционными ограничениями сайт 'https://grafana.com/' отвечает ошибкой 'HTTP ERROR 451'.*
> *При попытке импортировать дашборд по URL или ID, даже при использовании VPN, получим сообщение 'Unavailable For Legal Reasons'.*

:bangbang: На время установки подключаем VPN.

> **Примечание.**
> *Вариант автономной установки:*
> *-скачиваем дистрибутив 'grafana_${VERSION}_amd64.deb';*
> *-выполняем автономную установку 'sudo apt install -qq -y ./grafana_${VERSION}_amd64.deb';*
> *-переходим к шагу 3.*

Установку с помощью менеджера пакетов выполняем согласно официальной инструкции <a target="_blank" rel="noopener noreferrer" href="https://web.archive.org/web/20240127092921/https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/#install-from-apt-repository">Grafana</a>.

## Шаг 1: Подготовка системы

Обновите список пакетов и установите необходимые зависимости:

```sh
sudo apt update
sudo apt install -y apt-transport-https software-properties-common wget
```

## Шаг 2: Загрузка и установка программы

Установку программы выполняем из официального репозитория с помощью менеджера пакетов.

Импортируем GPG ключ:

```sh
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

Добавляем в систему указатель на официальный репозиторий стабильных релизов:

```sh
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

Обновляем список доступных пакетов:

```sh
sudo apt update
```

Устанавливаем последний релиз **Grafana OSS**:

```sh
sudo apt install -y grafana
```

## Шаг 3: Настройка программы

Запустите службу и проверьте её статус:

```sh
sudo systemctl start grafana-server
sudo systemctl status grafana-server
```

Если статус службы **active (running)** добавьте ее в автозагрузку:

```sh
sudo systemctl enable grafana-server
```

## Шаг 4: Настройка доступа к веб-интерфейсу программы

По умолчанию **Grafana** работает на порту 3000, если вы используете брандмауэр, необходимо разрешить этот порт.

```sh
sudo ufw allow 3000/tcp
sudo ufw reload
```

## Шаг 5: Контрольная проверка

Откройте в браузере веб-интерфейс **Grafana**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:3000'*.

По умолчанию, значение логина и пароля **'admin'**. При первом входе в браузере, потребуется изменить пароль, кроме того это можно сделать из консоли.

```sh
sudo grafana-cli admin reset-admin-password <new_password>
```

> **Примечание.** *В <a target="_blank" rel="noopener noreferrer" href="https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-grafana-on-ubuntu-22-04">статье</a> описаны некоторые настройки безопасности **Grafana**.*

Установка завершена!
