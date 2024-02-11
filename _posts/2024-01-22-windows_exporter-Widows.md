---
layout: post
title: Установка windows_exporter на Windows с веб-сайта разработчика
date: 2024-01-22
categories:
  - monitoring
---

<!-- # Установка **windows_exporter** на **Windows** с веб-сайта разработчика -->

> **Примечание.** *'windows_exporter' разрабатывается сообществом.*

## Шаг 1: Загрузка программы

Откройте в браузере веб-сайт разработчика [windows_exporter](https://github.com/prometheus-community/windows_exporter/releases){:target="_blank" :rel="noopener noreferrer"} и скачайте последнюю версию в формате **amd64.msi** - *windows_exporter-${VERSION}-amd64.msi*.

## Шаг 2: Установка и настройка программы

Выполните установку *windows_exporter-${VERSION}-amd64.msi*.

На данном этапе **windows_exporter** уже должен работать.

С возможностями тонкой настройки можно ознакомиться по ссылке [windows_exporter](https://github.com/prometheus-community/windows_exporter){:target="_blank" :rel="noopener noreferrer"}.

## Шаг 3: Настройка доступа к веб-интерфейсу программы

По умолчанию **windows_exporter** работает на порту 9182, если вы используете брандмауэр, необходимо разрешить этот порт.

## Шаг 4: Контрольная проверка

Откройте в браузере веб-интерфейс **windows_exporter**, пройдя по адресу *'http://FQDN_or_IP_Your_Server:9182/metrics'*.

Установка завершена!
