---
layout: post
title: Автоматизация создания образов Windows с использованием Packer
date: 2023-10-21
categories:
  - hashicorp
---

<!-- # Автоматизация создания образов **Windows** с использованием **Packer** -->

## Введение

**Packer** - это инструмент от компании HashiCorp, который позволяет автоматизировать процесс создания однородных образов виртуальных машин. В этом руководстве мы рассмотрим шаги по созданию образов Windows с использованием Packer.

### Предварительные требования

Прежде чем начать, убедитесь, что у вас установлен [Packer](https://www.packer.io) и есть доступ к репозиторию на [GitHub](https://github.com/IVAndr0n/packer-vsphere-windows). Также убедитесь, что у вас есть ISO-образ Windows, который будет использован для создания образа виртуальной машины.

### Структура репозитория на GitHub

Проект содержит каталог файлов для автоматического создания образов виртуальных машин на базе операционных систем Windows с помощью Packer, включая файлы конфигурации, скрипты настройки и скрипты сборки образа.

Структура репозитория:

<img allign="left" alt="img" src="https://raw.githubusercontent.com/IVAndr0n/packer-vsphere-windows/main/images/01.png" width="545">

<!-- ```sh
$ tree --dirsfirst -F
.
├── add/
│   └── win6.1/  # required updates for windows 7 and Windows Server 2008R2
│       ├── VMware-tools-11.0.6-15940789-i386.exe
│       ├── VMware-tools-11.0.6-15940789-x86_64.exe
│       ├── windows6.1-kb3138612-x64.msu
│       ├── windows6.1-kb3138612-x86.msu
│       ├── windows6.1-kb4474419-v3-x64.msu
│       ├── windows6.1-kb4474419-v3-x86.msu
│       ├── windows6.1-kb4490628-x64.msu
│       ├── windows6.1-kb4490628-x86.msu
│       ├── windows6.1-kb4555449-x64.msu
│       └── windows6.1-kb4555449-x86.msu
├── boot_config/
│   ├── Autounattend-BIOS-x64-7-2008R2.pkrtpl.hcl
│   ├── Autounattend-BIOS-x86-7.pkrtpl.hcl
│   ├── Autounattend-UEFI-x64-10-11-2012R2-2016-2019-2022.pkrtpl.hcl
│   └── Autounattend-UEFI-x86-10.pkrtpl.hcl
├── manifests/
├── scripts/
│   ├── add-win6.1.cmd
│   ├── config.ps1
│   ├── initialize.ps1
│   ├── install-vmtools.ps1
│   ├── remove-apps.ps1
│   └── specialize.ps1
├── build_all_windows_with_duration_record_parallel.ps1
├── build_all_windows_with_duration_record_sequential.ps1
├── build_all_windows_without_duration_record_parallel.cmd
├── README.md
├── template-windows_vsphere.pkr.hcl
├── variables_vsphere.pkr.hcl
├── windows-10-pro-x64_vsphere.pkrvar.hcl
├── windows-10-pro-x86_vsphere.pkrvar.hcl
├── windows-11-pro-x64_vsphere.pkrvar.hcl
├── windows-7-pro-x64_vsphere.pkrvar.hcl
├── windows-7-pro-x86_vsphere.pkrvar.hcl
├── windows-server-2008r2-std-x64_vsphere.pkrvar.hcl
├── windows-server-2012r2-std-x64_vsphere.pkrvar.hcl
├── windows-server-2016-std-x64_vsphere.pkrvar.hcl
├── windows-server-2019-std-x64_vsphere.pkrvar.hcl
└── windows-server-2022-std-x64_vsphere.pkrvar.hcl

5 directories, 36 files
``` -->

add/: Содержит необходимые обновления для Windows.
boot_config/: Включает файлы конфигурации для каждой версии Windows.
scripts/: Содержит скрипты PowerShell для настройки и установки дополнительного ПО.
build_all_windows_with_duration_record_parallel.ps1: Обеспечивает параллельное создание образов с фиксацией длительности процесса.
build_all_windows_with_duration_record_sequential.ps1: Обеспечивает последовательное создание образов с фиксацией длительности процесса.
build_all_windows_without_duration_record_parallel.cmd: Обеспечивает параллельное создание образов без фиксации длительности процесса.
README.md: Документация с подробным описанием процесса.
variables_vsphere.pkr.hcl: определены необходимые переменные, такие как адрес vCenter Server и параметры виртуальной машины
'<OS-name>_vsphere.pkrvar.hcl': Для каждой версии Windows созданы файлы переменных, содержащие уникальные параметры, такие как путь к ISO-образу и размер диска.
*'template-windows_vsphere.pkr.hcl'*: служит в качестве шаблона, содержащего конфигурацию Packer, локальные переменные и другие настройки.

### Создание образа с использованием Packer

Определите необходимые переменные в файле variables_vsphere.pkr.hcl.
Для каждой версии Windows создайте файлы переменных <OS-name>_vsphere.pkrvar.hcl, указав уникальные параметры.
Используйте файл template-windows_vsphere.pkr.hcl в качестве шаблона для конфигурации Packer.
Запустите Packer с помощью команды packer build, указав файл шаблона и соответствующие файлы переменных.
Packer загрузит ISO-образ операционной системы, создаст виртуальную машину в vSphere, применит скрипты и обновления, а затем создаст образ виртуальной машины.

### Запуск процесса создания образа

#### Создание одиночного образа

1. Откройте терминал и перейдите в директорию с файлами Packer.
2. Запустите команду *'packer build -force -var-file=<OS-name>_vsphere.pkrvar.hcl'* для начала процесса создания образа. Обратите внимание, что вместо *<OS-name>_vsphere.pkrvar.hcl* вы должны указать имя файла шаблона определенной операционной системы.

#### Создание множества образов

**Скрипты предназначены для работы в операционной системе Windows:**

*build_all_windows_with_duration_record_parallel.ps1* обеспечивает параллельное создание образов с фиксацией длительности процесса, снижает общее время создания образов за счет распределения нагрузки на сервер;

*build_all_windows_with_duration_record_sequential.ps1* обеспечивает последовательное создание образов с фиксацией длительности процесса, снижает нагрузку на сервер при создании образов за счет увеличения общего времени создания образов;

*build_all_windows_without_duration_record_parallel.cmd* обеспечивает параллельное создание образов, но не фиксирует длительность процесса.

1. Откройте терминал PowerShell и перейдите в директорию с файлами Packer.
2. Запустите необходимый скрипт.
3. Скрипт автоматически найдет все файлы конфигурации Packer (*'.pkrvar.hcl'*) в текущей директории и запустит процесс создания образов для каждого из них.
4. В процессе создания каждого образа будет записан лог и файл с длительностью процесса (исключение - скрипт *.cmd).
5. После завершения работы скрипта можно проверить логи и файлы длительности для анализа производительности и успешности создания образов.

### Поддерживаемые версии Windows

- Windows 7 x86/x64
- Windows 10 x86/x64
- Windows 11 x64
- Windows 2008 R2 x64
- Windows 2012 R2 x64
- Windows 2016 x64
- Windows 2019 x64
- Windows 2022 x64

### Заключение

Использование Packer в DevOps практиках позволяет улучшить процессы развертывания, обеспечивая быстрое создание надежных окружений. Преимущества включают ускорение развертывания, безопасность и консистентность среды.
