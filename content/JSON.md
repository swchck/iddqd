# JSON

## Что такое JSON

JSON (JavaScript Object Notation) - простой формат обмена данными, удобный для чтения и написания как человеком, так и компьютером. Он основан  на JavaScript. JSON - текстовый формат, полностью независимый от языка реализации, но он использует соглашения, знакомые программистам C-подобных языков, таких как C, C++, C#, Java, JavaScript, Perl, Python и многих других. Эти свойства делают JSON идеальным языком обмена данными.

## Что такое JSON Schema

JSON Schema - это стандарт описания структур данных в формате JSON. Использует синтаксис JSON. Схемы, описанные этим стандартом, имеют MIME "application/schema+json". Стандарт удобен для использования при валидации и документировании структур данных, состоящих из чисел, строк, массивов и структур типа ключ-значение.

## Что такое JSON объект

JSON объект - неупорядоченный набор пар ключ/значение. Объект начинается с { (открывающей фигурной скобки) и заканчивается } (закрывающей фигурной скобкой). Каждое имя сопровождается : (двоеточием), пары ключ/значение разделяются , (запятой).

## Какие есть правила синтаксиса JSON объекта (массива)

Есть несколько основных правил для создания строки JSON:

- Строка JSON содержит либо массив значений, либо объект (ассоциативный массив пар имя/значение).
- Массив заключается в квадратные скобки ([ и ]) и содержит разделенный запятой список значений.
- Объект заключается в фигурные скобки ({ и }) и содержит разделенный запятой список пар имя/значение.
- Пара имя/значение состоит из имени поля, заключенного в двойные кавычки, за которым следует двоеточие (:) и значение поля.

Чтобы включить двойные кавычки в строку, нужно использовать обратную косую черту: \". Так же, как и во многих языках программирования, можно помещать управляющие символы и шестнадцатеричные коды в строку, предваряя их обратной косой чертой.

Ниже приводится пример оформления заказа в формате JSON:

```JSON
{"menu": {
  "id": "file",
  "value": "File",
  "popup": {
    "menuitem": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}}
```

## Какие типы данных, поддерживаются в JSON

Значение в массиве или объекте может быть:

- Числом (целым или с плавающей точкой)
- Строкой (в двойных кавычках)
- Логическим значением (true или false)
- Другим массивом (заключенным в квадратные скобки)
- Другой объект (заключенный в фигурные скобки)
- Значение null

## Каковы недостатки JSON

Недостатками JSON являются:

- Трудно читается и анализируется пользователем, нет визуальности
- Нет синтаксиса для задания типа объекта
- Много синтаксического мусора

## Что такое JSONP

JSONP или "JSON with padding" является расширением JSON, позволяющим выполнять в единообразном стиле асинхронные запросы к сервисам, расположенным на другом домене - операцию, запрещённую в типичных веб-браузерах из-за политики ограничения домена.

## Какой MIME-тип в JSON

`application/json`

## Для чего используется JSON

Наиболее частое распространенное использование JSON - пересылка данных между сервером и браузером, а так же для хранения данных. Обычно данные JSON доставляются с помощью AJAX, который позволяет обмениваться данными браузеру и серверу без необходимости перезагружать страницу.

## Какие преимущества использования JSON

Сравнительные преимущества JSON:

- В разы меньший объем данных (экономия трафика, плюс к скорости работы сайта)
- Меньшая загрузка процессора и клиента, и сервера
- Почти неограниченные возможности расширения (т.к. можно выполнить ф-цию)
- Его предложения легко читаются и составляются как человеком, так и компьютером.
- Его легко преобразовать в структуру данных для большинства языков программирования (числа, строки, логические переменные, массивы и так далее)
- Многие языки программирования имеют функции и библиотеки для чтения и создания структур JSON.

## Какая функция используется для преобразования текста JSON в объект

Чтобы преобразовать текст JSON в объект используется функция `eval()`.

## Что такое JSON Parser

Вызов `JSON.parse(str)` превратит строку с данными в формате JSON в JavaScript-объект/массив/значение, возможно с преобразованием получаемого в процессе разбора значения.

## Что такое JSON-RPC

JSON-RPC(сокр. от англ. JavaScript Object Notation Remote Procedure Call - JSON-вызов удалённых процедур) - протокол удалённого вызова процедур, использующий JSON для кодирования сообщений. Это очень простой протокол (очень похожий на XML-RPC), определяющий только несколько типов данных и команд. JSON-RPC поддерживает уведомления (информация, отправляемая на сервер, не требует ответа) и множественные вызовы.

## Что такое JSON-RPC-Java

Реализация протокола JSON-RPC на Java

## Какова роль `JSON.stringify()`

Метод `JSON.stringify()` преобразует объекты JavaScript в строку в формате JSON, возможно с заменой значений, если указана функция замены, или с включением только определённых свойств, если указан массив замены. Используется, когда нужно из JavaScript передать данные по сети.

## Как парсить JSON в JavaScript

```JavaScript
var json = '{"name": "Andrew", "sn": "Cohen", "age": "24"}';
var obj = JSON.parse(json);
```

## Как создать JSON объект из JavaScript

```JavaScript
var obj = {};
    obj['name'] = 'Andrew';
    obj['sn'] = 'Cohen';
    obj['age'] = 24;
```

## Валидациия JSON в JavaScript

```JavaScript
function isValidJSON(jsonData) {
    try {
        JSON.parse(jsonData)
        return true;
    }
    catch (e) {
        return false;
    }
}

var json = '{"name": "Andrew", "sn": "Cohen", "age": "24"}';
isValidJSON(json);
```