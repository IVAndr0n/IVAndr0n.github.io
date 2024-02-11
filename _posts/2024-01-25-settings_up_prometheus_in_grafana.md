---
layout: post
title: Настройка Grafana для мониторинга Prometheus
date: 2024-01-25
categories:
  - monitoring
---

<!-- # Настройка **Grafana** для мониторинга **Prometheus** -->

:one: Откройте в браузере веб-интерфейс **Grafana**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:3000'*.

:two: Авторизуйтесь.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/01.png" width="370">

:three: Наведите мышь на знак *три полосы* слева сверху, затем нажмите *Open menu > Connections > Data sources*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/02.png" width="300">

Справа сверху нажмите *+ Add new data source*, выберите **Prometheus**.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/03.png" width="630">

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/04.png" width="520">

Заполните поля:

```sh
Name: <Your_Name>
Prometheus server URL: <http://FQDN_or_IP_Your_Server:9090>
```

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/05.png" width="750">

Прокрутите до конца страницы и нажмите *Save & Test*, если все верно, то будет сообщение *Successfully queried the Prometheus API*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/06.png" width="480">

:four: Скачайте дашборд.

На время закачки подключаем VPN и скачиваем с официального сайта <a target="_blank" rel="noopener noreferrer" href="https://grafana.com/grafana/dashboards/">Grafana</a> необходимый дашборд в формате JSON файла.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/07.png" width="450">

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/08.png" width="300">

:five: Импортируйте дашборд.

Наведите мышь на знак *три полосы* слева сверху, затем нажмите *Open menu > Dashboards*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/09.png" width="280">

Справа сверху нажмите *New > Import > Upload dashboard JSON file*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/10.png" width="820">

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/11.png" width="730">

Задайте уникальное имя и в разделе **Prometheus** выберите ваш *Prometheus data source*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/12.png" width="730">

:six: Войдите в импортированный дашборд.

Наведите мышь на знак *три полосы* слева сверху, затем нажмите *Open menu > Dashboards > Node Exporter Full*.

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/13.png" width="800">

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/IVAndr0n.github.io/main/_posts/images/2024-01-25-settings_up_prometheus_in_grafana/14.png" width="430">

:seven: Выполните дополнительные настройки безопасности.

Отключите возможность регистрации пользователей и вход для анонимных пользователей.

```sh
sudo vim /etc/grafana/grafana.ini
```

В разделе *users* найдите параметр *allow_sign_up*, установите значение *false*, раскомментируйте данный параметр, удалив символ *';'*.

В разделе *auth.anonymous* найдите параметр *enabled*, установите значение *false*, раскомментируйте данный параметр, удалив символ *';'*.

Сохраните изменения и перезапустите службу **Grafana**.

Установка завершена! :100:
