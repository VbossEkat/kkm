===
KKM
===

Contributors
------------
- Copyright (c) 2005, 2012
- Marat Khayrullin <xmm.dev@gmail.com>
- https://github.com/xmm/kkm/

Посмотрите на форк https://github.com/aborche/kkm

Introduction
------------
Библиотека, написанная на языке python, для работы с 
контрольно-кассовыми машинами (фискальными регистраторами).
Реализован обобщенный интерфейс и драйвер реализующий протокол Atol 
к ккм "Феликс-Р [ФК]" производства Курское ОАО "Счетмаш" и 
PayVKP-80K от Paykiosk.ru

Requirements
------------
Для работы необходим модуль serial.  
Под win32 последний раз проверялся в 2005,
под Linux работает сейчас - ноябрь 2012. 

Использованные документы
------------------------
 <1>: Атол технологии
       Руководство программиста: Протокол работы ККМ v2.4
 <2>: Атол технологии
       Руководство программиста: Общий драйвер ККМ v.5.1
       (версия док-ции: 1.7 от 15.05.2002)
 <3>: Курское ОАО "Счетмаш"
       Инструкция по программированию РЮИБ.466453.528 И15
       Машина электронная контрольно-кассовая Феликс-Р Ф
 <4>: Атол технологии
       Приложение к протоколу работы ККМ (2009)

License
-------
Начиная с версии 2.0.pre1 (опубликованной на https://github.com/xmm/kkm/)
эта библиотека распространяется под лицензией Apache-2.0. Подробности в файле LICENSE. 
Предыдущая версия, расположенная по адресу http://code.google.com/p/kkm/ 
распространялась под лицензией GPL-2.

Installation
------------
     python setup.py

Usage
-----
Инициализация фискальника::

     import kkm
     kkmDev = kkm.KkmMeta.autoCreate({'port': '/dev/ttyS0', 'baudrate': 115200})

или выбрать конкретный драйвер::

     kkm.Atol.AtolKKM({'port': '/dev/ttyS0', 'baudrate': 115200})

Проведение оплаты со скидкой на весь чек::

     kkmDev.setRegistrationMode(KKM_PASSWORD) 
     kkmDev.OpenCheck() 
     kkmDev.Sell(name.strip(), Decimal(price.strip()), Decimal(count.strip()), 0) 
     kkmDev.Discount(discount, kkm.kkm_Check_dis) 
     kkmDev.Payment(payment) 

Вывод сообщения на дисплей покупателя::

     kkmDev.PrintToDisplay('\x0C')
     kkmDev.PrintToDisplay('')
     max = kkmDev.getDisplayStringMax()
     kkmDev.PrintToDisplay('Спасибо'.center(max))
     kkmDev.PrintToDisplay('за покупку!'.center(max))
