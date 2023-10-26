# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
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
- В проекте реализовать персептрон, который умеет производить вычисления: OR, AND, NAND, XOR, и дать комментарии о кореектности работы.
- Задание 2.
- С помощью скрипта на языке Python заполнитm google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре. Средствами google-sheets визуализируйте данные в google-таблице для наглядного представления выбранной игровой величины.
- Задание 3.
- Настроить на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
- Выводы.

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### В проекте реализовать перцсптрон, который умеет производить вычисления: OR, AND, NAND, XOR, и дать комментарии о корректности работы.

Ход работы:
- Для начала я создал пустой проект на Unity. Добавил на сцену пустой объект и прикрепил к нему следующий код на языке C#:
```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}

```
## Рассмотрим реализацию логических операций:
### 1) OR (Логическое сложение(ИЛИ)):
- Таблица:
 
| A | B | A OR B |
|-- |-- | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

- Установим следующие значние для работы персептрона:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/abd46c17-e1d6-468a-b7e5-3d7dad83fb28)

- С начаал зададим 1 обучающую эпоху и посмотрим на результаты тестов:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/1dda91db-4365-43bb-b8a0-eba3d6687f82)

Небольшое пояснение: в нашем случае значение Total Error отвечает за обучение персептрона: если оно отлично от нуля, то модель не обучилась, если же ноль - тогда модель успешно обучилась. При первой эпохе обучения значение Total Error = 2, и из результатов тестов видно, что значения отличны от истины: в первом случае, ответ 1, хотя должен быть 0.

- Зададим большее количество эпох, например, 4:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/bb4f834d-9ae8-4ca9-b8fe-5c910d2c95bc)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/55f2ba26-a4c6-444c-a8fc-9a12f9b93b07)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/c4ec003f-e29b-4ad7-9d9b-c0ff1b9ba388)

- В данном случае уже при четвертой эпохе обучения Totl Error равняется нулю, и мы видим из результатов тестов, что модель успешно обучилась. Повторим запуск программы с 4 эпохами обучения:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/4655bba0-62b7-4794-bac3-52681a4f2178)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/23c2bfba-8342-43e2-a5ae-e5ef688955fc)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/58781137-59a5-48c9-badf-7d16b139be0f)

По этим результатам можно сделать вывод, что не всегда 4 эпохи обучения достаточно, бывает, что и при 4 итерации Total Error отлично от нуля и программа работает некорректно.
В ходе подбора количества эпох обучения, в моем случае, на пятой эпохе значение Total Error равно нулю. (Подробные значения на каждой эпохе я буду разбирать во втором задании с построением графиком на основе количества эпох обучения.)

### 2) AND (Логическое умножение(И)):
- Таблица:
 
| A | B | A AND B |
|-- |-- | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

- Установим следующие знаечния дя работы персептрона:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/277429b8-36b2-49f6-b791-de0d3017dada)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты наших тестов:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/d094eb1e-9192-48b4-8d59-88d255cc4d71)

Собственно, как и в случае с OR, 1 эпохи обучения недостаточно для корректных ответов.

- Зададим 4 эпохи обучения:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/ff90a28e-db11-4716-b44b-da093d6aa1bc)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/67d1eb3f-6ae5-4044-98cc-d7e3e204dcac)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/d8925398-7f97-49d3-a36a-30e889044a71)

- При данной операции 4 эпох для обучения недостаточно. Попробуем снова:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/873d2013-21c1-4e64-bf40-7bdce60c2a67)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f1905def-74d8-4927-8f03-4a8f929d35af)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/cdad90d4-cf00-4b49-8bd9-08964e464f69)

В этот раз 4 эпохи обучения было достаточно. О подробных значениях при каждой эпохе будет описано подробнее во втором задании.

### 3) NAND (Инвертированное Логическое умножение(НЕ И)):
- Таблица:
 
| A | B | A NAND B |
|-- |-- | ------ |
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- Установим следующие значения для нашего скрипта:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/c9d96d08-3938-4c94-8a30-9517bb866001)

- Как и в прошлых операция, для начала зададим 1 эпоху обучения и посмотрим на результаты наших тестов:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/791cf2d0-7f1f-4ed9-b7f3-96119226e4b4)

Можно сделать вывод, что при одной эпохи для обучения мало (это подтверждается уже на трех логических операциях).

- Установим 4 эпохи обучения:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/79fa7490-0ff8-4300-8a9a-9bf7f393fad6)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2494cc55-5180-40ee-8f43-c267dbef874a)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/4f699905-fc2c-4ba6-8226-2b3ad2e942b6)

Четырех эпох обучения мало (проверил на нескольких запусках)

- Установим 8 эпох обучения:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/1f197b17-62b8-47c9-954c-70e441645028)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/9bc51402-2d80-48e2-8416-4ed0235cb1d3)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/db362c36-d9fe-4d46-b0d2-0c203d3092c3)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/66fb5f5e-0322-4923-8425-380b6ca5a462)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/0a2c41f6-3c2f-47ea-abed-201c91438c0f)

В данной попытке уже при шестой эпохе обучения значение Total Error было равно нулю. Подробные значения рассмотрим во втором задании.

### 4) XOR (Исключающая Логическая сумма((ИЛИ) и (НЕ И))):
- Таблица:
 
| A | B | A XOR B |
|-- |-- | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- Установим следующие значения для нашего скрипта:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/94e42a5a-7626-4e7e-8255-b241a5349fed)

И сразу назначим 4 эпохи обучения:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/54fa8aa8-38bf-432f-82ac-0ecccea9ac9e)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/e7aba88a-644a-4805-ad34-751eca3dd64e)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/7b24a335-d606-47ea-9c5f-d8d665edb092)

При нескольких попытках четырех эпох было мало для обучения. В ходе работы с разными значениями эпох обучения я выяснил, что начиная с третьей-четвертой эпохи значение Total Error всегда было равно 4. Следовательно - однослойный перцептрон не может обучиться этой операции.

Поискав информацию в интернете и почитав больше данных о персептроне, я выяснил, что однослойный персептрон в силах решать только линейные задачи, а операция XOR не является линейной.

Эту задачу с помощью персептрона можно решить следующим способом: применим формулу ``` x XOR y = (x Or y) AND (x NAND y) ```. Для этого я создал еще две треннировочные модели (tsOr и tsNAND). А в модели ts использовал логику обычного умножения. Когда первые две модели оттренировались, их значения я положил в функцию CalcOutput после тренировки модели умножения. В итоге у меня получился следующий код:

```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {
	public TrainingSet[] tsOR;
	public TrainingSet[] tsNAND;
	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i, TrainingSet[] set)
	{
		double dp = DotProductBias(weights,set[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j, TrainingSet[] set)
	{
		double error = set[j].output - CalcOutput(j, set);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error * set[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs, TrainingSet[] set)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < set.Length; t++)
			{
				UpdateWeights(t, set);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8, tsOR);
		double tsOr0 = CalcOutput(0,0); //0
		double tsOr1 = CalcOutput(0,1); //1
		double tsOr2 = CalcOutput(1,0); //1
		double tsOr3 = CalcOutput(1,1); //1
		
		Train(8, tsNAND);
		double tsNAND0 = CalcOutput(0,0); //1
		double tsNAND1 = CalcOutput(0,1); //1
		double tsNAND2 = CalcOutput(1,0); //1
		double tsNAND3 = CalcOutput(1,1); //0

		Train(8, ts);
		Debug.Log("Test 0 0: " + CalcOutput(tsOr0, tsNAND0));
		Debug.Log("Test 0 1: " + CalcOutput(tsOr1, tsNAND1));
		Debug.Log("Test 1 0: " + CalcOutput(tsOr2, tsNAND2));
		Debug.Log("Test 1 1: " + CalcOutput(tsOr3, tsNAND3));		
	}
	
	void Update () {
		
	}
}

```

- В итоге, после выполнения 8 эпох обучения у каждой модели мы получаем результат, который является верным для даннйо операции:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/6b0ec1b0-cee3-488d-a729-6041bb67710e)

Опять же, подробный отчет по эпохам тренировок будет указан во втором задании.

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
### Настроить на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
(Для дальнейшей работы возьмем данные, сгенерированные с помощью кода, описанного в предыдущем задании)
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
- Обговорим условие воспроизведения звуковых дорожек:
  Если процентное соотношение будет < 10, то воспроизводится звук "Плохо"; Если процентное соотношение > 10, но < 100-звук "Нормально". ведь это будет означать, что нам не хватит накопленных ресурсов; А если процентное соотношение > 100-звук "Отлично":
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/87189841-4a8a-467b-b669-7a83bac64ba5)

- Далее добавляем в сцену пустой объект GameObject:
- ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/07138ce4-90a1-409b-bdba-c1eb106db1bc)

- Добавляем в папку Assets файл со скриптом на языке C# и звуковые файлы
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/18ca88db-1eec-41f6-a60a-f45aa8381a53)
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f2380831-56e4-480e-83d3-cff933c65a32)


- После этого нужно настроить инспектор свойства объекта GameObject следующим образом (подключить скрипт и звуковые дорожки):
- ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/1559db52-edaf-4bbb-9bd9-aaa008fe0075)
  
- Запускаем выполнение и наслаждаемся проигрыванием звуковых дорожек, которые мы указали (так же можем видеть, что значения, выводимые в консоли Unity, полностью совпадают с значениями из таблицы)
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/31197978-08a4-4a04-b893-60d1f82db071)
  ![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/a3b6bcd1-d8ce-4bbe-bbd8-517fb8fabb9e)



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

