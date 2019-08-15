## Тестовое задание на должность Junior Frontend разработчика в AdGo

### Описание задачи
Наша компания занимается рекламой. Мы собираем огромное множество статистических данных о показах, кликах и стоимости рекламных объявлений. Одним из основных инструментов нашей системы является статистика. Статистика это интерфейс для просмотра и анализа подбного рода данных. Мы предлагаем реализовать Вам упрощенную версию данного инструмента.

Статистика - набор событий о показах рекламы. Показ происходит в определенной среде: устройстве, операционной системе, браузере... Также пользователь, увидевший рекламу, может кликнуть по рекламному объявлению и посмотреть рекламу. За просмотр рекламодатель оплачивает определенную сумму денег.

Если посмотреть на показы в таблице, то это будет выглядить следующим образом:

| ID      | Platform | Operating system | Browser | Click | Money | Datetime            |
| ------- | -------- | ---------------- | ------- | ----- | ----- | ------------------- |
| 1       | mobile   | android          | chrome  | true  | 0.004 | 2019-08-13 07:43:41 |
| 2       | mobile   | ios              | safari  | false | 0     | 2019-08-13 13:27:57 |
| 3       | desktop  | windows          | chrome  | false | 0     | 2019-08-14 03:51:13 |
| 4       | mobile   | android          | opera   | false | 0     | 2019-08-14 17:33:21 |
| 5       | desktop  | macos            | firefox | true  | 0.08  | 2019-08-15 09:00:05 |

>В [репозитории находится api-сервер](server), который умеет общаться по `HTTP`. Данное api предоставит вам данные которые необходимы для выполнения данного задания. [Подробное описание api-сервера](#api) в конце readme.

### Задание
Вам нужно реализовать интерфейс с выводом статистических данных в таблицу. Добавить возможность сгруппировать данные, применить фильтры, а также просмотр в режиме постраничной навигации.

![scheme](docs/scheme.png "Scheme")
<p align="center"><i>Схематичное представление задания.</i></p>

По пунктам:

- Таблица с данных;
- Постраничная навигация, например по 10 или 25 строк;
- Фильтр периода за который происходит выбор статистики;
- Фильтр по платформе, браузерам и операционным системам, о которых будет отображена информация;
- Любые ваши дополнения привествуются.

### FAQ

<strong>Куда и как отправить решение?</strong>

Сделайте `fork` данного репозитория. Реализуйте свое решение в директории [app](app). Когда будете готовы, создайте `pull request` в `master` ветку нашего репозитория. Заодно мы оценим ваше умение работать с `GIT`.

<strong>Какие библиотеки и инструменты можно использовать?</strong>

Решение обязательно должно быть написано с использование библиотеки [React](https://reactjs.org). Мы не хотим накладывать на вас ограничения, вы можете использовать любые другие библиотеки, но нам важно посмотреть **ваш код** и **ваши решения**.

<strong>Можно ли использовать бибилотеку компонентов?</strong>

Вы можете оформить проект как угодно, даже если это будет чистый **html** с минимальным количеством **css**, нам хватит. Если вы не можете не делать супер красивые и удобные интерфейсы, любите какой-то набор компонентов, можете воспользоваться им, например **Bootstrap**.

### <a name="api"></a> API
>Для запуска веб сервера выполните команду `npm start`

`GET /api/v1/platforms` - список платформ

Пример ответа:

```json
[  
    {  
        "label": "Desktop",
        "value": 1
    },
    {  
        "label": "Mobile",
        "value": 2
    }
]
```

`GET /api/v1/browsers` - список браузеров

Пример ответа:

```json
[  
    {  
        "label": "Chrome",
        "value": 1,
        "platform": 1
    },
    {  
        "label": "Firefox",
        "value": 2,
        "platform": 1
    },
    {
        "..."
    }
]
```

`GET /api/v1/operating-systems` - список операционных систем

Пример ответа:

```json
[  
     {  
        "label": "Windows",
        "value": 1,
        "platform": 1
    },
    {  
        "label": "Mac OS",
        "value": 2,
        "platform": 1
    },
    {
        "..."
    }
]
```

`GET /api/v1/groups` - список группировок

Пример ответа:

```json
[  
    {  
        "label": "Day",
        "value": "day"
    },
    {  
        "label": "Platform",
        "value": "platform"
    },
    {
        "..."
    }
]
```

`GET /api/v1/statistics?searchParams` - та самая статистика

Ответ выглядит вот так:

```json
{
    "count": "количество всех записей на сервере по вашему запросу",
    "rows": "данные, с учетом пагинации",
    "total": "просуммированные показатели, вычисляются на основе всех записей, а не только тех что в поле rows"
}
```

Параметры `searchParams`

| Поле               | Тип                          | Обязательное | Значение по умолчанию | Описание                                                                                     |   |   |
|--------------------|------------------------------|--------------|-----------------------|----------------------------------------------------------------------------------------------|---|---|
| groupBy            | Строка                       | Да           | -                     | поле `value` из списка группировок                                                           |   |   |
| from               | Дата, в формате `YYYY-MM-DD` | Да           | -                     | дата с которой извлекаются события                                                           |   |   |
| to                 | Дата, в формате `YYYY-MM-DD` | Да           | -                     | дата по которую, включительно, извлекаются события                                           |   |   |
| limit              | Число                        | Нет          | 25                    | количество записей                                                                           |   |   |
| offset             | Число                        | Нет          | 0                     | сдвигает выборку на offset * limit записей, `offset=0&limit=25` возвращают первые 25 записей |   |   |
| platform           | Строка                       | Нет          | -                     | поле `value` из списка платформ                                                              |   |   |
| browsers[]         | Строка                       | Нет          | -                     | поле `value` из списка брузеров,,можно передать несколько                                    |   |   |
| operatingSystems[] | Строка                       | Нет          | -                     | поле `value` из списка операционных систем, можно передать несколько                         |   |   |

Несколько примеров

Получаем статистику за первые 7 дней июля, сгруппированную по дням

`/api/v1/statistics?groupBy=day&from=2019-07-01&to=2019-07-07`

Ответ:

```json
{  
    "count": 7,
    "rows":[  
        {  
            "day":"2019-07-01",
            "impressions":23,
            "clicks":4,
            "money":0.15445
        },
        {  
            "day":"2019-07-02",
            "impressions":11,
            "clicks":1,
            "money":0.06262
        },
        {  
            "day":"2019-07-03",
            "impressions":17,
            "clicks":1,
            "money":0.01448
        },
        {  
            "day":"2019-07-04",
            "impressions":13,
            "clicks":2,
            "money":0.17215
        },
        {  
            "day":"2019-07-05",
            "impressions":24,
            "clicks":8,
            "money":0.41402999999999995
        },
        {  
            "day":"2019-07-06",
            "impressions":21,
            "clicks":4,
            "money":0.33422999999999997
        },
        {  
            "day":"2019-07-07",
            "impressions":23,
            "clicks":4,
            "money":0.04693
        }
    ],
    "total":{  
        "impressions":132,
        "clicks":24,
        "money":1.1988899999999998
    }
}
```

Вот так можем посмотреть что у нас происходило в июне и на разных платформах

`/api/v1/statistics?groupBy=platform&from=2019-06-01&to=2019-06-30`

Ответ:

```json
{  
    "count": 2,
    "rows":[  
        {  
            "platform":"Mobile",
            "impressions":473,
            "clicks":97,
            "money":4.42389
        },
        {  
            "platform":"Desktop",
            "impressions":133,
            "clicks":17,
            "money":0.97717
        }
    ],
    "total":{  
        "impressions":606,
        "clicks":114,
        "money":5.40106
    }
}
```

### The end

Баг фиксы и правки приветствуются. Все вопросы можно задать в [телеграмм](https://t.me/sse1595).

Пожалуйста, приложите инструкцию как нам запустить ваше решение.

Удачи!
