---
## Front matter
title: "Моделирование сетей передачи данных"
subtitle: "Лабораторная работа № 1. Введение в Mininet"
author: "Демидова Екатерина Алексеевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

**Цель работы**

Основной целью работы является развёртывание в системе виртуализации (например, в VirtualBox) mininet, знакомство с основными командами для работы с Mininet через командную строку и через графический интерфейс.

**Задачи**

1. Настроить стенд виртуальной машины Mininet
2. Освоить основы работы в Mininet

# Теоретическое введение

Mininet[@mininet] — это эмулятор компьютерной сети. Под компьютерной сетью подразумеваются простые компьютеры — хосты, коммутаторы, а так же OpenFlow-контроллеры. С помощью простейшего синтаксиса в примитивном интерпретаторе команд можно разворачивать сети из произвольного количества хостов, коммутаторов в различных топологиях и все это в рамках одной виртуальной машины(ВМ). На всех хостах можно изменять сетевую конфигурацию, пользоваться стандартными утилитами(ipconfig, ping) и даже получать доступ к терминалу. На коммутаторы можно добавлять различные правила и маршрутизировать трафик.

Mininet создает реалистичную виртуальную сеть, выполняя реальный код ядра, коммутатора и приложения на одной машине (VM, облачной или собственной) за считанные секунды с помощью одной команды `sudo mn`. 

# Выполнение лабораторной работы

## Настройка стенда виртуальной машины Mininet

Импортируем образ Mininet(рис. @fig:001)

![Импорт конфигураций](image/1.png){#fig:001 width=70%}

Исправим ошибки в конфигурации виртуальной машины, а затем создадим дополнительно к NAT соединение сиртуального адаптера хоста на втором адаптере(рис. @fig:002, @fig:003).

![Настройка виртуальной машины](image/2.png){#fig:002 width=70%}

![Настройка виртуальной машины](image/3.png){#fig:003 width=70%}

Зайдем в виртуальную машину в пользователя по умолчанию и проверим IP-адрес(рис. @fig:004).

![Начало работы с Mininet](image/4.png){#fig:004 width=70%}

Настроим SSH-соединение без пароля с виртуальной машиной по ее IP-адресу(рис. @fig:005)

![SSH-соединение с Mininet](image/5.png){#fig:005 width=70%}

Добавим сетевое соединение eth1(рис. @fig:006).

![Добавление сетевого соединения eth1](image/6.png){#fig:006 width=70%}

Скачаем новую версию Mininet, обновим исполняемые файлы и проверим обновление системы(рис. @fig:007).

![Обновление версии Mininet](image/7.png){#fig:007 width=70%}

Для увеличения размера шрифта и применения векторных шрифтов вместо растровых необходимо внести изменения в файл /etcX11/app-defaults/XTerm(рис. @fig:008).

![Настройка параметров XTerm](image/8.png){#fig:008 width=70%}

Настроим соединения X11 для суперпользователя, чтобы была возможность под ним открывать графические приложения(рис. @fig:009).

![Настройка соединения X11 для суперпользователя](image/9.png){#fig:009 width=70%}

## Основы работы в Mininet

Запустим минимальную топологию, затем посмотри созданные узлы и соединения в ней, а также интерфейсы узлов. Затем пропингуем с одного узла другой(рис. @fig:010).

![Работа с Mininet с помощью командной строки](image/10.png){#fig:010 width=70%}

## Построение и эмуляция сети в Mininet с использованием графического интерфейса

Построим базовую топологию сети и зададим IP-адреса(рис. @fig:011)

![Построение сети Mininet](image/11.png){#fig:011 width=70%}

Запустим эмуляцию и проверим корректность(рис. @fig:012).

![Запуск сети](image/12.png){#fig:012 width=70%}

Затем настроим диапазон адресов и посмотрим, какие адреса из этого диапазона были выданы автоматически(рис. @fig:013).

![Автоматическое выделение адресов](image/13.png){#fig:013 width=70%}

В конце сохраним топологию(рис. @fig:014).

![Сохранение топологии](image/14.png){#fig:014 width=70%}

# Выводы

В результате выполнения работы был развёрнут в системе виртуализации  VirtualBox mininet, а также ознакомились с основными командами для работы с Mininet через командную строку и через графический интерфейс.

# Список литературы{.unnumbered}

::: {#refs}
:::


