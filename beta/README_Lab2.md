# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
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
- Написать программу Hello World на Python с запуском в Jupiter Notebook.
- Задание 2.
- Написать программу Hello World на C# с запуском на Unity.
- Задание 3.
- Оформить отчет в виде документации на github (markdown-разметка).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выберать одну из компьютерных игр, привести скриншот её геймплея и краткое описание концепта игры. Выберать одну из игровых переменных в игре, описать её роль в игре, условия изменения / появления и диапазон допустимых значений. Построить схему экономической модели в игре и указать место выбранного ресурса в ней.

Ход работы:
- Для начала нужно было выбрать конкретную игру (я остановился на игре Genshin Impact). Genshin Impact - многопользовательская компьютерная онлайн игра, в которую можно играть как одному, так и с другими людьми.
- Концепт игры: Приключенческая игра с открытым миром. Это означает, что на той стороне реки или горы вас ожидает новый, восхитительный пейзаж, нужно лишь разумно расходовать силы. Если вам повстречается блуждающая фея или странный механизм, дерзните и попробуйте разгадать их тайну. Но что они сулят — беду или приятный сюрприз — сможете узнать только вы! В игре также есть:
- Взаимодействие стихий и разнообразные боевые тактики; - Визуальный стиль и прекрасная музыка; - Отправляйтесь в путешествие с компаньонами.(взято с официального сайта HoYolab)
- Скриншот геймплея игры:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/6d750150-073a-4d02-8d79-616d6881512d)
- Для описания ресурса я выбрал первородную смолу.
- Описание: данный ресурс появляется у игрока каждый день; в течение 8 минут восстанавливается в количестве одной единицы; нужен для получения наград после прохождения босов или испытаний; можно восстановить за счет слабой смолы (отдельный игровой ресурс).
- Ниже предсавлена примерная экономическая модель данного ресурса:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/785ff3fb-f7e7-4b1b-ba86-d9c9b8d6f0c1)

## Задание 2
### С помощью скрипта на языке Python заполнитm google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре. Средствами google-sheets визуализирjdfnm данные в google-таблице для наглядного представления выбранной игровой величины.

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

- Для того, чтобы звуки воспроизводились в Unity, нужен следующий код на языке C#:
```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet.Count == 0) return;

        if (dataSet["Mon_" + i.ToString()] > 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        //https://docs.google.com/spreadsheets/d/1H8NQd7gLpebbL2FP3OfZFlVCu7aluN6y4JKxrS4H7tE/edit?usp=sharing
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1H8NQd7gLpebbL2FP3OfZFlVCu7aluN6y4JKxrS4H7tE/values/Лист1?key=AIzaSyA8cQwYwFO0Zl0RYh3XIhfdmc4xNKHd7a4");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}


```





## Задание 3
### Настроить на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
- Добавить в сцену пустой объект GameObject:
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/07138ce4-90a1-409b-bdba-c1eb106db1bc)
- После этого нужно настроить инспектор свойства объекта GameObject следующим образом (подключить скрипт и звуковые дорожки):
- ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/1559db52-edaf-4bbb-9bd9-aaa008fe0075)
- Запустить выполнение и наслаждаться проигрыванием звуковых дорожек, которые мы указали

## Выводы
- В ходе данной лабораторной работы я познакомился с визуализацией даных из таблицы GoogleSheets в Unity, с их визуализацие и работой со звуковыми эффектами в Unity с помощью скриптов на языках Python и C#. Смог вывести изменить условия воспроизведения звуков и прослушал эти звуки, в зависимости от данных из Google-таблицы, которые были получены с помощью скрипта на языке Python.


| Plugin | README |
| ------ | ------ |
| GitHub | [plugins/github/README.md][PlGh] |
| Visual Studio| [plugins/visualstudio/README.md][PlGh] |
| Unity | [plugins/unity/README.md][PlMe] |
| Pucharm | [plugins/pycharm/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

