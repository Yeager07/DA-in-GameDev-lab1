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
- Построить графики зависимости количества эпох от ошибки обучения. Указать, от чего зависит необходимое количество эпох обучения.
- Задание 3.
- Визуализировать работу персептрона с помощью физуальной модели на сцене Unity.
- Выводы.

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### В проекте реализовать перцсптрон, который умеет производить вычисления: OR, AND, NAND, XOR, и дать комментарии о корректности работы (Дал комментарии в выводе к работе).

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

- В итоге, после выполнения 8 эпох обучения у каждой модели мы получаем результат, который является верным для данной операции:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/6b0ec1b0-cee3-488d-a729-6041bb67710e)

Опять же, подробный отчет по эпохам тренировок будет указан во втором задании.

Из полученных данных можем сделать вывод, что один слой перцептрона способен решать только линейные задачи, такие как OR, AND и NAND. Для решения нелинейной задачи XOR необходимо добавлять еще несколько слоев перцептронов.

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать, от чего зависит необходимое количество эпох обучения (Указал в выводе к работе).

Ход работы:
- Заполнить Google-Таблицу данными об эпохах и визуализировать их с помощью графиков (проделать для каждой логической операции, разобранной в первом задании).

В данной работе для каждой операции я делал 5 попыток с 8-ю эпохами обучения. На графиках указано среднее значение Total Error за 5 попыток.
### OR (Логическое сложение(ИЛИ)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/d6e3acad-a83b-4f35-9e18-b98aad124cdc)

### AND (Логическое умножение(И)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/030731a8-e15f-4f50-95f5-0f4bfd23cf3a)

### NAND (Инвертированное Логическое умножение(НЕ И)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2e95c306-8c02-4b46-9905-a218a52c3ad0)

### XOR (Исключающая Логическая сумма((ИЛИ) и (НЕ И))):
У данной операции для получения количества ошибок я складывал количество ошибок при каждой операции (сумма ошибок на первой эпохе обучения в каждой операции и так для всех эпох обучения)
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/412c9e69-f277-4148-ac37-43be86de5556)


Количество эпох обучения зависит от решаемой операции. Например при операции OR ошибок не встречается уже на пятой эпохе обучения, при AND и NAND - при седьмой, при XOR - при восьмой. Статистика не совсем точная, так как взято небольшое количество попыток, всего 5.

## Задание 3
### Визуализировать работу персептрона с помощью физуальной модели на сцене Unity.
Ход работы:
- Создать сцену в Unity, добавить несколько кубов, плоскость(сделал фиолетовую, т.к. мой любимый цвет, не судите строго) и визуализировать каждую логическую операцию.
- Для начала я создал сцену с тремя вариантами возможными вариантами, в которой черный куб симвализирует 0 (Ложь), а белый - 1 (Истина); возможно всего три варианта пар значений: (0 0), (0 1)/(1 0) и (1 1).
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f5d0c9a1-7973-4045-b6cd-913138e46b42)

При столкновении кубов, они оба должны окраситься в цвет результата той или иной операции (напомню, черный цвет означает 0 (Ложь), а белый - 1 (Истина)).

Для этого в коде я завел две новые переменные, которые будут отвечать за значение, которое представляет собой куб (0 или 1). И уже эти значения нужно будет подставлять в функцию CalcOutput для определения результата операции после обучения модели. Также у верхнего куба нужно назначить Rigidbody, а у нижнего - Is trigger. поэтому старый код немного поменялся, вот новый код:

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
	// public TrainingSet[] tsOR;
	// public TrainingSet[] tsNAND;
	public TrainingSet[] ts;

	public double firstCube;
	public double secondCube;
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
		// Train(8, tsOR);
		// double tsOr0 = CalcOutput(0,0); //0
		// double tsOr1 = CalcOutput(0,1); //1
		// double tsOr2 = CalcOutput(1,0); //1
		// double tsOr3 = CalcOutput(1,1); //1
		
		// Train(8, tsNAND);
		// double tsNAND0 = CalcOutput(0,0); //1
		// double tsNAND1 = CalcOutput(0,1); //1
		// double tsNAND2 = CalcOutput(1,0); //1
		// double tsNAND3 = CalcOutput(1,1); //0
		Train(8, ts);
	}
	
	void Update () {
		
	}
	private void OnTriggerEnter(Collider other) {
		if (CalcOutput(firstCube, secondCube) == 0) {
			other.gameObject.GetComponent<Renderer>().material.color = Color.black;
        	this.gameObject.GetComponent<Renderer>().material.color = Color.black;
		}
		else {
			other.gameObject.GetComponent<Renderer>().material.color = Color.white;
        this.gameObject.GetComponent<Renderer>().material.color = Color.white;
		}
    }
}

```
### OR (Логическое сложение(ИЛИ)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/6409610f-2729-41f5-8c3b-9d7dad0fcaaa)

- Результат:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2ebc2721-951b-4b4e-8618-0025b91a07e9)
- Процесс выполнения:
![OR](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/ac5e1d3f-6cbe-44e3-a0aa-22668930cc1f)




### AND (Логическое умножение(И)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f5d0c9a1-7973-4045-b6cd-913138e46b42)

- Результат:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/0fb4853e-3e4e-4f4a-892b-835ea6a8c2e7)
- Процесс выполнения:
![203351408-9844174f-4b46-4eed-9e82-bceece979c1a](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/0f123e5e-d264-4f4c-a8cf-bfaf1fb74056)




### NAND (Инвертированное Логическое умножение(НЕ И)):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f5d0c9a1-7973-4045-b6cd-913138e46b42)

- Результат:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/2642eaa8-fc3a-4105-8486-2b5c7d4347c7)
- Процесс выполнения:
![NAND](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f7e5be35-887b-4574-8f21-9067cefc3228)




### XOR (Исключающая Логическая сумма((ИЛИ) и (НЕ И))):
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/f5d0c9a1-7973-4045-b6cd-913138e46b42)

- Результат:
![image](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/5dc84337-963c-4254-ac9d-5e2d90f858a4)
- Процесс выполнения:
![XOR](https://github.com/Yeager07/DA-in-GameDev-lab1/assets/127008112/9640483d-b80c-4125-b7ce-ef780131cc2d)




## Выводы
- В ходе данной лабороторной работы я познакомился с моделью нейронной сети - персептроном. С помощью однослойного персептрона смог реализовать такие логические операции, как: OR, AND, NAND. Для операции XOR мне пришлось применять три персептрона, каждый из которых выполнял одну из операций упомянутых ранее. Из этого я сделал вывод, что однослойный персептрон способен решать только линейные задачи. Также построил графики на основе количества ошибок при каждой эпохе обучения, чтобы оценить обучаемость модели для той или иной операции: выяснил, что легче всего персептрону далась операция OR (видно из графика). Выяснил, что количество эпох обучения зависит от решаемой операции. Например при операции OR ошибок не встречается уже на пятой эпохе обучения, при AND и NAND - на седьмой, при XOR - на восьмой. Статистика не совсем точная, так как взято небольшое количество попыток, всего 5, а также, применив функционал Unity, построил наглядную модель работы персептрона: при столкновении кубов выполнялась та или иная логическая операция и они окрашивались в цвет её результата (более подробно описал в 3 задании).


| Plugin | README |
| ------ | ------ |
| GitHub | [plugins/github/README.md][PlGh] |
| Visual Studio| [plugins/visualstudio/README.md][PlGh] |
| Unity | [plugins/unity/README.md][PlMe] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

