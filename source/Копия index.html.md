---
title: TIM Connect SDK

language_tabs:
  - native


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

<h1>Введение</h1>

Комплект средств разработки TIM Connect является способом взаимодействия с сервисом на основе следующих возможностей:

- регистрация и авторизация устройства в сервисе;
- получение и обработка PUSH-уведомлений, проходящих через СМСЦ сервиса;
- доступ к архиву сообщений пользователя, проходящих через СМСЦ сервиса;
- визуализация информации, предоставленной в сообщениях;
- доступ к статистическим данным, формируемым пользователями сервиса;
- формирование данных о геолокации и персональных маркеров сообщений (хэштеги).

<img src=https://www.tim-connect.com/media/splash.png>


<h1>Краткая сводка работы с SDK</h1>

В рамках использования SDK можно выделить общую структуру данных, которая будет обрабатываться одинаково на всех мобильных платформах устройств. Сервис предоставляет информацию, которая не является закрытой информацией банка с точки зрения дублирования SMS. Однако, предполагается, что данные из PUSH-уведомлений будут хранится во внутренней памяти телефона в зашифрованном виде на уровне приложения, а код обфусцирован (в случае с ОС Android).

<h2>Минимальные требования для запуска</h2>
<aside class="warning">
SDK TIM Connect требует выполнение следующих шагов интеграции:
</aside>
<p>1)  Прием PUSH-уведомлений с сервера и отображение в центре уведомлений ОС;<p>
<p>2)  Сохранение и вывод сообщений в ленте уведомлений внутри приложения;<p>
<p>3)  Двусторонний звонок в приложение.<p>
<p>
Корректная работа сервисов возможно только при выполнении всех трех шагов интеграции.
<p>



```native
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

