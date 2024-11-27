---
## Front matter
title: "Моделирование сетей передачи данных"
subtitle: "Лабораторная работа № 5. Эмуляция и измерение потерь пакетов в глобальных сетях"
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

Основной целью работы является получение навыков проведения интерактивных экспериментов в среде Mininet по исследованию параметров сети, связанных с потерей, дублированием, изменением порядка и повреждением пакетов при передаче данных.

**Задачи**

1. Задайте простейшую топологию, состоящую из двух хостов и коммутатора с назначенной по умолчанию mininet сетью 10.0.0.0/8.
2. Проведите интерактивные эксперименты по по исследованию параметров сети, связанных с потерей, дублированием, изменением порядка и повреждением пакетов при передаче данных.
3. Реализуйте воспроизводимый эксперимент по добавлению правила отбрасывания пакетов в эмулируемой глобальной сети. На экран выведите сводную информацию о потерянных пакетах.
4. Самостоятельно реализуйте воспроизводимые эксперименты по исследованию параметров сети

# Теоретическое введение

Mininet[@mininet] — это эмулятор компьютерной сети. Под компьютерной сетью подразумеваются простые компьютеры — хосты, коммутаторы, а так же OpenFlow-контроллеры. С помощью простейшего синтаксиса в примитивном интерпретаторе команд можно разворачивать сети из произвольного количества хостов, коммутаторов в различных топологиях и все это в рамках одной виртуальной машины(ВМ). На всех хостах можно изменять сетевую конфигурацию, пользоваться стандартными утилитами(ipconfig, ping) и даже получать доступ к терминалу. На коммутаторы можно добавлять различные правила и маршрутизировать трафик.

# Выполнение лабораторной работы

## Запуск лабораторной топологии

Зададим простейшую топологию, состоящую из двух хостов и коммутатора с назначенной по умолчанию mininet сетью 10.0.0.0/8. На хостах h1 и h2 введем команду ifconfig, чтобы отобразить информацию, относящуюся к их сетевым интерфейсам и назначенным им IP-адресам. Проверим подключение между хостами сети(рис. @fig:001)

![Простейшая сеть](image/1.png){#fig:001 width=70%}

## Добавление потери пакетов на интерфейс, подключённый к эмулируемой глобальной сети

На хосте h1 добавим 10% потерь пакетов к интерфейсу h1-eth0 и проверим(рис. @fig:002).

![Потери 10% на хосте h1](image/2.png){#fig:002 width=70%}

Потеряно 14% пакетов. 

Дполнительно на хосте h2 добавим 10% потерь пакетов к интерфейсу h1-eth0 и проверим(рис. @fig:003).

![Потери 10% на хосте h1 и h2](image/3.png){#fig:003 width=70%}

Потеряно 27% пакетов.

## Добавление значения корреляции для потери пакетов в эмулируемой глобальной сети

Добавьте на интерфейсе узла h1 коэффициент потери пакетов 50% (такой высокий уровень потери пакетов маловероятен), и каждая последующая вероятность зависит на 50% от последней(рис. @fig:004).

![Добавление значения корреляции для потери пакетов на h1](image/4.png){#fig:004 width=70%}

Потеряны пакеты под номерами 23-25, 30-37, 39-41, 43-46, 48, всего 42% пакетов утеряно.

## Добавление повреждения пакетов в эмулируемой глобальной сети

Добавьте на интерфейсе узла h1 0,01% повреждения пакетов. Проверим конфигурацию с помощью инструмента iPerf3 для проверки повторных передач(рис. @fig:005)

![Добавление повреждения пакетов и проверка](image/5.png){#fig:005 width=70%}

Значения повторно переданных пакетов можно увидеть в колонке `Retr`. Общее число 121.

## Добавление дублирования пакетов в интерфейс подключения к эмулируемой глобальной сети

Для интерфейса узла h1 зададим правило c дублированием 50% пакетов (т.е. 50% пакетов должны быть получены дважды). Проверим, что на соединении от хоста h1 к хосту h2 имеются дублированные пакеты, используя команду ping с параметром -c 20 с хоста h1. Дубликаты пакетов и помечаются как DUP!(рис. @fig:006).

![Добавление дублирования пакетов](image/6.png){#fig:006 width=70%}

## Воспроизведение экспериментов

В виртуальной среде mininet в своём рабочем каталоге с проектами создадим каталог simple-drop и перейдем в него. Создадим скрипт для эксперимента lab_netem_ii.py:

```
#!/usr/bin/env python
"""
Simple experiment.
Output: ping.dat
"""

from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI

from mininet.log import setLogLevel, info
import time

def emptyNet():

  "Create an empty network and add nodes to it."
  net = Mininet( controller=Controller, waitConnected=True )

  info( '*** Adding controller\n' )
  net.addController( 'c0' )

  info( '*** Adding hosts\n' )
  h1 = net.addHost( 'h1', ip='10.0.0.1' )
  h2 = net.addHost( 'h2', ip='10.0.0.2' )

  info( '*** Adding switch\n' )
  s1 = net.addSwitch( 's1' )

  info( '*** Creating links\n' )
  net.addLink( h1, s1 )
  net.addLink( h2, s1 )

  info( '*** Starting network\n')
  net.start()

  info( '*** Set delay\n')
  h1.cmdPrint( 'tc qdisc add dev h1-eth0 root netem loss 10%' )
  h2.cmdPrint( 'tc qdisc add dev h2-eth0 root netem loss 10%' )

  time.sleep(10) # Wait 10 seconds

  info( '*** Ping\n')
  h1.cmdPrint( 'ping -c 100', h2.IP(), '| grep "time=" | awk \'{print $5, $7}\' | sed -e \'s/time=//g\' -e \'s/icmp_seq=//g\' > ping.dat'' )

  info( '*** Stopping network' )
  net.stop()

if __name__ == '__main__':
  setLogLevel( 'info' )
  emptyNet()
```

В этом скрипте создается простейщая топология сети, затем с помощью комнанд, использованных нами ранее задается потеря в 10% для обоих хостов, после чего пингуется второй хост(100 сообщений отправляется), при этом из сообщений при пинге вытаскиваются номер сообщения и значение времени, которые записываются в файл с данными.

Скорректируем скрипт так, чтобы на экран или в отдельный файл выводи-
лась информация о потерях пакетов.

```
#!/usr/bin/env python
"""
Simple experiment.
Output: ping.dat
"""

from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI

from mininet.log import setLogLevel, info
import time

def emptyNet():

  "Create an empty network and add nodes to it."
  net = Mininet( controller=Controller, waitConnected=True )

  info( '*** Adding controller\n' )
  net.addController( 'c0' )

  info( '*** Adding hosts\n' )
  h1 = net.addHost( 'h1', ip='10.0.0.1' )
  h2 = net.addHost( 'h2', ip='10.0.0.2' )

  info( '*** Adding switch\n' )
  s1 = net.addSwitch( 's1' )

  info( '*** Creating links\n' )
  net.addLink( h1, s1 )
  net.addLink( h2, s1 )

  info( '*** Starting network\n')
  net.start()

  info( '*** Set delay\n')
  h1.cmdPrint( 'tc qdisc add dev h1-eth0 root netem loss 10%' )
  h2.cmdPrint( 'tc qdisc add dev h2-eth0 root netem loss 10%' )

  time.sleep(10) # Wait 10 seconds

  info( '*** Ping\n')
  h1.cmdPrint('ping -c 100', h2.IP(), '| grep "packet loss" | awk \'{print $6, $7, $8}\')

  info( '*** Stopping network' )
  net.stop()

if __name__ == '__main__':
  setLogLevel( 'info' )
  emptyNet()
```

Создадим Makefile для управления процессом проведения эксперимента

```
all: ping.dat
ping.dat:
  sudo python lab_netem_ii.py
  sudo chown mininet:mininet ping.dat
clean:
  -rm -f *.dat
```

Запустим эксперимент и увидим, что выводится потеря 22%(рис. @fig:007).

![Проведение эксперимента](image/7.png){#fig:007 width=70%}


# Выводы

В результате выполнения работы получили навыки проведения интерактивных экспериментов в среде Mininet по исследованию параметров сети, связанных с потерей, дублированием, изменением порядка и повреждением пакетов при передаче данных. Эти параметры влияют на производительность протоколов и сетей.

# Список литературы{.unnumbered}

::: {#refs}
:::


