# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Есипенко Игорь Ярославович
- РИ220936
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Предложить вариант изменения найденных переменных для 10 уровней в игре. Визуализировать изменение уровня сложности в таблице. 
- Задание 2.
- Создать 10 сцен на Unity с изменяющимся уровнем сложности.
- Задание 3.
- Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.
- Выводы.

## Цель работы
Разработать оптимальный баланс для десяти уровней игры Dragon Picker.

## Задание 1
### Предложить вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.

Ход работы:
- Скачать репозиторий игры Dragon Picker, изучить скрипты игровых объектов, выделить переменные, которые можно изменять в 10 уровнях, предложить вариант по изменению найденных переменных, визуализировать эти переменные в Google таблице.

## Переменные, которые подвергнутся изменению в течение 10 уровней:

### timeBetweenEggDrops
- Время между сбрасыванием яиц. В первом уровне сбрасывание яйца происходит каждые 2 секунды. Для усложнения прохождения игры стоит уменьшить временной интервал сбрасывания яиц, сделать так, чтобы дракон сбрасывал их чаще, что усложнит их поимку на более высоких уровнях.

### speed
- Скорость перемещения дракона. Для усложнения прохождения необходимо сделать поведение дракона более непредсказуемым. Один из вариантов усложнения: увеличение скорости дракона на каждом следующем уровне.

### leftRightDistance
- Расстояние, проходимое драконом. Эту переменнуюможно увеличивать на каждом уровне. Если дракон будет проходить больше расстояния и с большей скоростью, то, по итогу, успевать за ним станет гораздо сложнее, что повзолит увеличить сложность уровня.

## Вариант изменения найденных переменных для 10 уровней, полученный с помощью скрипта на языке Python (будет указан в 3 задаче):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/8a49b42b-bd71-4db3-abbe-2cec6154be9e)



## Задание 2
### С помощью скрипта на языке Python заполнитm google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре. Средствами google-sheets визуализируйте данные в google-таблице для наглядного представления выбранной игровой величины.

Ход работы:
- Для начала нужно реализовать код на языке Python и настроить свзяь с Google-таблицей.
Для этого нужно создать новый проект в GoogleCloud:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/bb71f8c1-bebb-4743-a032-4571f40add22)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/c722f3d6-e773-4101-91f8-0cdaef331ee6)

- Установить дополнительные продукты для дальнейшей работы:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/4aa5e99b-0ced-47fd-b5ba-eb78d0c86e55)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/fa61e1a6-ced5-4b6c-940b-faebdd715dfe)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/3c7296c4-ee74-4b21-842a-a4d52cc85bb5)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/0165d723-fc17-4bfe-98ef-e13ba38e4276)

- Создать API-ключ и настроить доступ к нашей таблице по ссылке:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/88833fa7-402a-47c3-ae1b-9a0ac00ad6f8)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2f6a2ded-960e-4f40-a283-512cc1c8f3be)
![2023-10-11_18-51-21](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/de6ef14c-37b6-4303-889a-b83526807560)

- Чтобы убедиться что мы сделали все правильно, запустим следу.щий код, чтобы проверить, что полученные данные генерируются в нашей таблице:
```py

import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400014-da50a8398902.json')
sh = gc.open("UnityWorkshop2")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)

```
- По результату можно понять, что полученные данные выводятся в нашей Google-таблице (сравнение результатов из консоли и из таблицы):
![2023-10-11_18-49-00](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/4d735f1f-a27d-4f34-b26a-2cb4e1ce4da8)
![2023-10-11_18-48-41](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/96b5bd1c-a6f2-40c2-ad6b-016d1bc29708)
- Следующим шагом нужно немного переделать исходный код.
  Так как наш ресур может изменяться от 0 до 160, то и генерировать числа мы будем в этом диапазоне.
  В первом столбце таблицы будет показываться номер итерации; Во втором-сгенерированное число; А в третьем-процентное соотношение разницы полученного и предыдущего числа в сравнении с минимальным значением, необходимым для получения награды (минимальное значение-40)
  Сверим полученные значения:
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/6b054f70-2260-45e1-b27f-f61580c54e69)
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/720c5f36-b868-45f9-b951-f1ec732046e1)

- Визуализаци данных с помощью средств Google-Sheets (на оси X представлены полченные значения; а на оси Y-их процентное соотношение, высчитанное по формуле из кода на языке Python):
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/65fd02af-03d3-4610-9e7f-238d802ca3bd)


## Задание 3
### Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.

Ход работы:
- Написать код заполняющий Google таблицу данными из скрипта. Результатом является скрин с первого задания. В Unity также данные изменяются. Результат изменения данных в Unity - скриншоты со 2 задания.

```py

import gspread
import random

gc = gspread.service_account(filename='enduring-button-401711-f2400eac09f9.json')
sh = gc.open("Workshop3-Баланс в играх")


def set_values(column, start_min, start_max, delta):
    count = 2
    limit_min = start_min
    limit_max = start_max
    while count < 11:
        count += 1
        sh.sheet1.update((column + str(count)), str(random.uniform(limit_min, limit_max)))
        limit_min, limit_max = limit_min + delta, limit_max + delta


set_values(column='B', start_min=5, start_max=5.5, delta=1.50)
set_values(column='C', start_min=11, start_max=12, delta=0.75)
set_values(column='D', start_min=1.8, start_max=1.9, delta=-0.15)

```

## Выводы
- В ходе данной лабораторной работы я попробовал настроить баланс в игре (на примере игры Dragon Picker), смоделировал 10 различных сцен в Unity на основе данных, полученных из Google-Таблицы, с помощью кода на языке C#, а также повторил способ отображения данных в Google-Таблице, полученных с помощью скрипта на языке Python.


| Plugin | README |
| ------ | ------ |
| GitHub | [plugins/github/README.md][PlGh] |
| Visual Studio| [plugins/visualstudio/README.md][PlGh] |
| DropBox | [plugins/dropbox/README.md][PlGh] |
| Unity | [plugins/unity/README.md][PlMe] |
| Google Drive | [plugins/googledrive/README.md][PlGh] |
| Pycharm | [plugins/pycharm/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

