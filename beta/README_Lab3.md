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
- Расстояние, проходимое драконом. Эту переменную можно увеличивать на каждом уровне. Если дракон будет проходить больше расстояния и с большей скоростью, то, по итогу, успевать за ним станет гораздо сложнее, что повзолит увеличить сложность уровня (также увеличение данной переменной позволит избежать ситуации, когда дракон будет находиться чисто на однм месте, если бы мы, например, уменьшали данное значение).
 
## Вариант изменения найденных переменных для 10 уровней, полученный с помощью скрипта на языке Python (будет указан в 3 задании):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/fdd525a2-a29a-432f-9ee5-39cd8344b7ec)


## Задание 2
### Создать 10 сцен на Unity с изменяющимся уровнем сложности.
Ход работы:
- Создать ещё 9 аналогичных сцен, написать код, который будет считывать данные из таблицы и менять знаечния у нужных переменным в зависимости от уровня.
- Я продублировал исходную сцену 9 раз и для каждой сцены установил значения переменных с помощью скрипта на языке C#, которые отвечают за поведение дракона: "Speed", "TimeBetweenEgg", "LeftRightDistance".
- ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/90427a74-5d6c-4ab9-af99-cb789877e097)

## Для того, чтобы переменные приняли нужные значения из нашей Google-Таблицы, я я использовал следующий код на языке C# из предыдущей лабораторной работы, который немнго переделал для данной работы:

```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;
using System.Globalization;

public class GoogleSheetsParser : MonoBehaviour
{
    private List<float[]> data = new List<float[]>();

    private readonly string url = "https://sheets.googleapis.com/v4/spreadsheets/1H8NQd7gLpebbL2FP3OfZFlVCu7aluN6y4JKxrS4H7tE/values/A1%3AZ100?key=AIzaSyA8cQwYwFO0Zl0RYh3XIhfdmc4xNKHd7a4";

    public List<float[]> Data => data;

    public IEnumerator ParseGoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get(url);

        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);

        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            var count = 0;
            var arr = new float[3];

            foreach (var row in selectRow)
            {
                if (float.TryParse(row, NumberStyles.Number, CultureInfo.CreateSpecificCulture("en-US"), out _) & count <= 3)
                {
                    arr[count] = float.Parse(row, NumberStyles.Number, CultureInfo.CreateSpecificCulture("en-US"));
                    count++;
                }

                if (count == 3) data.Add(arr);
            }
        }
    }
}

```
- А также немного изменил код EnemyDragon.cs, чтобы знаечния, полученные из Google-Таблицы, присваивались нужным переменным в зависимости от номера уровня (1-10):

```C#

using UnityEngine;

public class EnemyDragon : MonoBehaviour
{
    [SerializeField] private GoogleSheetsParser parser;
    [SerializeField] private int levelIndex;

    public GameObject dragonEggPrefab;
    public float speed;
    public float leftRightDistance;
    public float timeBetweenEggDrops;
    public float chanceDirection;

    private void Start()
    {
        Invoke(nameof(Initialize), 1f);
        Invoke("DropEgg", 2f);
    }

    private void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance) speed = Mathf.Abs(speed);
        else if (pos.x > leftRightDistance) speed = -Mathf.Abs(speed);
    }

    private void DropEgg()
    {
        Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
        GameObject egg = Instantiate(dragonEggPrefab);
        egg.transform.position = transform.position + myVector;
        Invoke("DropEgg", timeBetweenEggDrops);
    }

    private void Initialize()
    {
        speed = parser.Data[levelIndex][0];
        leftRightDistance = parser.Data[levelIndex][1];
        timeBetweenEggDrops = parser.Data[levelIndex][2];
        chanceDirection = 0.01f;
    }

    private void FixedUpdate() 
    {
        if (Random.value < chanceDirection) speed *= -1;
    }
}

```


## Задание 3
### Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.

Ход работы:
- Написать код заполняющий Google таблицу данными из скрипта. Результатом является скрин с первого задания. В Unity также данные изменяются. Результат изменения данных в Unity - скриншоты со 2 задания.

```py

from matplotlib import pyplot as plt
import gspread
import random


speed = [4]
leftRightWidth = [10]
timeBetweenEggDrops = [2]
level = [1, 2, 3, 4, 5, 6 ,7, 8, 9, 10]

gc = gspread.service_account(filename='enduring-button-401711-f2400eac09f9.json')
sh = gc.open("Workshop3-Баланс в играх")


def set_values(column, start_min, start_max, delta):
    count = 2
    limit_min = start_min
    limit_max = start_max
    array = []
    while count < 11:
        count += 1
        array.append(random.uniform(limit_min, limit_max))
        sh.sheet1.update((column + str(count)), str(random.uniform(limit_min, limit_max)))
        limit_min, limit_max = limit_min + delta, limit_max + delta
    return array


speed += (set_values(column='B', start_min=5, start_max=5.5, delta=1.50))
leftRightWidth += (set_values(column='C', start_min=11, start_max=12, delta=0.75))
timeBetweenEggDrops += (set_values(column='D', start_min=1.8, start_max=1.9, delta=-0.15))

plt.plot(speed, level)
plt.title("Скорость дракона")
plt.ylabel("Номер уровня")
plt.xlabel("Сгенерированное значение")
plt.show()

plt.plot(leftRightWidth, level)
plt.title("Расстояние, на которое перемещается дракон")
plt.ylabel("Номер уровня")
plt.xlabel("Сгенерированное значение")
plt.show()

plt.plot(timeBetweenEggDrops, level)
plt.title("Время между бросками яиц")
plt.ylabel("Номер уровня")
plt.xlabel("Сгенерированное значение")
plt.show()

```

## Результат визуализации данных в Python:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/7aba4993-6d97-4411-9cdc-28de9590cf6b)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2f67a58f-bbd9-40a2-beb8-ccd2683b3687)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/d67e46ab-926c-4715-9cbe-0e173be61c42)



## Выводы
- В ходе данной лабораторной работы я попробовал настроить баланс в игре (на примере игры Dragon Picker), смоделировал 10 различных сцен в Unity на основе данных, полученных из Google-Таблицы, с помощью кода на языке C#, визуализировал изменение переменных с помощью библиотеки matplotlib, а также повторил способ отображения данных в Google-Таблице, полученных с помощью скрипта на языке Python.


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

