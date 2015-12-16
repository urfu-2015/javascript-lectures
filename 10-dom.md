# dom

## Введение

Когда-то давным давно, когда js был совсем молод и еще ходили Netscape-динозавры, то все думали,
что мощь-js заканчивается следующими сценариями:

1. создание мигающих ссылок;
2. скрытие/расскрытие элементов;
3. ...

Метеориты падали и падали, многие динозавры вымерли, и JS начал изменяться под потребности работадателей и
других 'зверей':

1. Исполнения кода не только в браузере;
2. Запуск js на сервере;
3. Firefox Os;
4. Phonegap;
5. ...

Но все 'мутации', которым подвергся js, не изменили его главную цель, а именно, создание динамических страниц
в браузере. Конечно со времен динозавров много воды утекло и потребности изменились:

1. Календари;
2. Частичное заполнение форм;
3. Саггесты(подсказки при вводе);
4. Динамическая отрисовка страниц;
5. И многое другое...

В этой лекции мы поговорим о том, что делает фронтенд разработчика - фронтенд разработчиком.

P.S. Все примеры, который написаны в данной главе работают в относительно современных браузерах.
Так как лекции готовятся с расчетом на смерть ie10 <=

## Запуск кода в браузере

Запустить js в браузере можно множеством способов:

1. Через консоль разработки
2. Через сервисы-песочницы
3. Через html документ

Давай-те рассмотрим каждый из вариантов:

### Запуск кода через консоль

1. Запускаем Яндекс.Браузер
2. Идем в Settings
3. Идем в Advanced
4. Идем в More Tools
5. Идем в Developer Tools
6. Затем идем в таб Console
7. Пишем в строке код 2+2
8. Жмем enter // 4

P.S. Для запуска Developer Tools есть хоткей

### Запуск кода через песочницу

1. идем на сайт jsfiddle.net
2. во вкладке js пишем: console.log(2+2)
3. жмем кнопку run
4. идем в Developer tools

### Запуск кода через html документ

1. Создадим простую html-страницу

```html
    <html>
    <head></head>
    <body>
     <h1>Hello world!</h1>
    </boby>
    </html>
```

2. Вставляем тэг-script

```html
    <html>
    <head></head>
    <body>
     <h1>Hello world!</h1>
     <script>
        console.log(2+2);
     </script>
    </boby>
    </html>
```

3. Открываем ваш документ в браузере
4. открываем Developer Tools

## DOM

### Что такое DOM

### Что такое DOM для разработчика

DOM - это API, которое предоставляет браузер js-интерпретатору для взаимодействия с html-документов:

1. чтение данных из документа;
2. изменение документа;
3. реакция на действия пользователя;

Очень важно понимать, что DOM - это API, которое удоволетворяет стандарту, то есть
каждый браузер старается реализовать стандарт, но бывает возникают некоторые 'проблемы'.

Стандарт можно почитать здесь: http://www.w3.org/DOM/

### Что такое DOM для интерпретатора

```html
    <html>
      <body>
        <form>
          <input type='text' placeholder='Имя'/>
          <input type='text' placeholder='Фамилия'/>
          <button type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

DOM - это Document Object Model, то есть браузер предоставляет интерпретатору
не html страницу ввиде строки, а некоторый object, который связан с документом,
отображаемым в браузере. То есть при изменении модели меняется документ, а при
изменении документа меняется модель.

Грубо говоря модель - это граф, который состоит из узлов(Node).

### Что такое узел?

Node - это, объект со следующими полями:

1. тип узла;
2. набор атрибутов;
3. ссылка на левого/правого соседа;
4. ссылка на родительский узел;
5. ссылка на массив 'детей';

Каждый узел имеет тип. Типов очень много, но не все из них одинакого полезные,
поэтому укажу только важные:

1. ELEMENT\_NODE=1 - элемент;
2. TEXT\_NODE=3 - текст;
3. COMMENT\_NODE=8 - комментарий
4. DOCUMENT\_NODE=9 - документ(ссылка на тэг-html)
5. DOCUMENT\_TYPE\_NODE=10 - DOCTYPE
6. DOCUMENT\_FRAGMENT\_NODE=11 - фрагмент

Из списка типов элементов можно понять следующее:

1. DOM сохраняет информацию о тексте;
2. DOM сохраняет информацию о комментарии;
3. DOM содержит особый элемент DOCUMENT\_NODE, то есть на корень графа;

Пока оставим за сценой, что такое DOCUMENT\_FRAGMENT\_NODE, но в конце лекции мы вернемся к нему.

DOCUMENT\_NODE - в документе ровно один. Для получение ссылки
на DOCUMENT\_NODE нужно обратиться к переменную document.

```js
    document.nodeType === document.DOCUMENT_NODE;
```

Давай-те вернемся к нашему основному примеру:
```html
    <html>
      <body>
        <form>
          <input type='text' placeholder='Имя'/>
          <input type='text' placeholder='Фамилия'/>
          <button type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Сколько узлов будет в DOM?

1. 6 - элементарных узлов;
2. 1 - элемент текста;

Но не все так просто мы забыли посчитать пробельные символы между элементами:

1. 6 - элементарных узлов;
2. 9 - текстовых узлов;

То есть соседний элемент у 'Фамилия' - не 'Имя', а некоторый текстовый элемент.
Поэтому многие разработчики 'минифицируют' свой html перед отправкой его клиенту,
удаляя все лишние пробелы и переводы строк.

### Как искать что-то по DOM

```html
    <html>
      <body>
        <form>
          <input type='text' placeholder='Имя'/>
          <input type='text' placeholder='Фамилия'/>
          <button type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Прежде чем перейти к чему-то сложному, нужно научиться находить какие-то узлы.

Поставим для себя следующие задачи:

1. Найти форму
2. Найти текстовые поля

#### Поиск по id

Давай-те разметим наш документ id-ми:

```html
    <html>
      <body>
        <form id='form'>
          <input id='name' type='text' placeholder='Имя'/>
          <input id='surname' type='text' placeholder='Фамилия'/>
          <button id='submit' type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Для поиска по id нужно воспользоваться методом getElementById, который есть только у
document.

```js
    var form = document.getElementById('form');
    var name = document.getElementById('name');
```



1. Подходит для поиска уникальных элементов;
2. Очень быстрый;
3. Поведение не гарантированно при наличии дупликатов;

#### Поиск по тэгам

Возьмем наш html.

```html
    <html>
      <body>
        <form>
          <input type='text' placeholder='Имя'/>
          <input type='text' placeholder='Фамилия'/>
          <button type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Для поиска по тэгу необходимо воспользоваться методом getElementsByTagName.

```js
    var forms = document.getElementsByTagName('form');
    var input = document.getElementByTagName('input');
```

Метод возвращает не массив элементов, а некоторый array-like объект NodeList.
NodeList - это живая-коллекция, то есть она меняется при изменении DOM

Из-за своей особенности(живучести), NodeList очень сильно не любят =\>
от него очень часто избавляются по средством превращения array-like объекта в массив:

```js
    var inputs = document.getElementByTagName('input');
    inputs = [].slice.call(inputs);
```

Но если вы готовы:

1. Помнить, что коллекция может неожиданно измениться
2. Кэш коллекции сбрасывается при изменении DOM
3. Вы не сможете пользоваться методами массива

Метод getElementsByTagName есть не только у document, но и у всех других элементов,
если вы вызываете этот метод не у document, то метод ищет в контексте элемента.



1. вы можете найти все тэги за один запрос;
2. можно искать в контексте определенного элемента;
3. редко нужно искать по тэгам, так как такой код сложно поддерживать

#### Поиск по классу

Возьмем наш html и добавим все элементам классы.

```html
    <html>
      <body>
        <form class='b-form'>
          <input class='b-form__name' type='text' placeholder='Имя'/>
          <input class='b-form__surname' type='text' placeholder='Фамилия'/>
          <button class='b-form__submmit' type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Для поиска элементов по классам можно воспользоваться методом
getElementsByClassName, который есть так же у всех узлов.

```js
    var form = document.getElementsByClassName('b-form')[0];
    var name = form.getElementsByClassName('form__name')[0];
```


1. Можно искать сразу много элементов удоволетворяющих одному css селектору;
2. Можно гибко размечать нужные элементы;
3. Метод не такой быстрый, как getElementById, но не критично.

#### Поиск элемента по css-селектору:

Для поиска по DOM интерпретатор предоставляет два очень удобных метода:

1. querySelector - поиск первого элемента по css-селектору
2. querySelectorAll - поиск всех элементов удоволетворяющих css-селектору

```js
    var form = document.querySelector('.b-form');
    var items = form.querySelectorAll('input');
```

1. метод очень удобный
2. метод помедленне всех остальных, но не критично

Мы рекомендуем вам пользоваться им.

### Как искать что-то рядом

#### Поиск родительского элемента

Для поиска родительского элемента нужно воспользоваться свойствами:

1. parentNode
2. parentElement

parentNode - ищет родительский Node узел произвольного типа, а parentElement
ищет родительский узел типа отличного от комментарий, текст или DOCTYPE.

Лучше использовать всегда parentElement, или обфуцировать свой html и использовать
parentNode.

```js
    var formsName = document.querySelector('.b-form__name');
    var form = formsName.parentElement;
```

#### Поиск определенного родительского элемента

Метод closest, который есть у всех элементов, позволяет найти ближайшего родителя
или самого себя, удоволетворяющего css-селектору.

Если нет отца/самого\_себя удоволетворяющего условию, то вернется null

```html
    <div class='b-page'>
       <div class='controller'></div>
    </div>
    <div class='b-page'>
       <div class='another-controller'></div>
    </div>
```

```js
    var controller = document.querySelector('.controller');
    var page = controller.closest('.b-page');
```

#### Поиск соседей

Для поиска соседей существует 4ре свойства:

1. nextSibling
2. nextElementSibling
3. previousSibling
4. previousElementSibling

next - ищет правого соседа, а previous - левого.

```js
    var formsName = document.querySelector('.b-form__name');
    var formsSurName = formsName.nextSibling;

    formsName === formsSurName.previousSibling;
```

### Поиск детей

Для поиска детей есть следующие свойства:

1. firstChild - возвращает ссылку на первого ребенка
2. firstElementChild
3. lastChild - возвращает ссылку на последнего ребенка
4. lastElementChild
5. childNodes - возвращает live-array со списком детей
6. children

```js
    var form = document.querySelector('.b-form__name');
    var name = form.firsElementChild;
    var submit = form.lastElemenChild;
```

### Matches

Метод, который есть у всех элементов, проверяет что dom-element подходит под
css-селектор.

```html
    <div class='b-page'>
       <div class='controller current'></div>
    </div>
    <div class='b-page'>
       <div class='another-controller'></div>
    </div>
```

```js
    var controller = document.querySelector('.controller');
    controller.matches('.controller.current')
```

## Работа с атрибутами

атрибуты нужны для двух вещей:

1. для настройки нативной работы некоторых html элементов(таких как ссылки)
2. для передачи данных на клиент и работы с ними

### Чтение из атрибутов

```html
    <html>
      <body>
        <form class='b-form'>
          <input class='b-form__name' type='text' placeholder='Имя'/>
          <input class='b-form__surname' type='text' placeholder='Фамилия'/>
          <button class='b-form__submmit' type='submit'>Отправить</button>
        </form>
      </body>
    </html>
```

Для чтения из атрибутов нужно воспользоваться методом getAttribute()

```js
    var name = document.querySelector('.b-form__name');
    name.getAttribute('placeholder');
```

### Изменение атрибутов

Для чтения из атрибутов нужно воспользоваться методом setAttribute()

```js
    var name = document.querySelector('.b-form__name');
    name.setAttribute('placeholder', 'Имя вашего соседа');
```

### Для удаления атрибутов

Для чтения из атрибутов нужно воспользоваться методом removeAttribute()

```js
    var name = document.querySelector('.b-form__name');
    name.removeAttribute('placeholder');
```

### свойства vs атрибуты

Многие атрибуты отражены в свойствах Node, например:

```js
    var name = document.querySelector('.b-form__name');

    name.placeholder;
    name.className;
    name.id;
```

НО:

1. Не у всех атрибутов совпадает имя со свойством. Например, class и className
2. значение, находящиеся в атрибуте, может не совпадать со свойством
3. при изменении атрибута/свойства синхронизация не всегда гарантирована

### важные cвойства и атрибуты

1. id -
Идентификатор объекта, который используется для поиска уникальных элементов.
2. className -
В js 'class' зарезервированное имя, поэтому разработчики стандарта
решили использовать имя className
3.
style -
Это свойство используется для задания стилей конкретным элементам.
Обычно используется для динамического изменения: width, height, left, right...
Если у вас есть какой-то конечный(разумный) набор состояний у html элемента, то лучше
менять класс, а style использовать только в крайнем случаи.
4.
и другие

### Пользовательские атрибуты

Веб-стандарты выделелили под бизнес-лапшу namespace атрибутов.
Все атрибуты с префиксом 'data-' считаются пользовательскими =\> веб-стандарты
никогда не выпустят атрибут, который начинается с 'data-' =\> их можно безопасно использовать
для своих нужд.

```html
    <div class='page' data-server-time='01/09/2007'>
    </div>
```

```js
    var page = document.querySearch('page');
    page.getAttribute('data-search-time');
```

Более того, веб-стандарты создали для нас специальное свойство
dataset, которое содержит все пользовательские атрибуты.

Если бы пользовательские атрибуты в словаре dataset именовались так же
как в html-разметке, то тогда ими было бы не удобно пользоваться:

```js
    var domElement = document.querySelector('.element');
    domElement.dataset['data-track-id'];
```

Поэтому разработчики стандарта решили нам упростить жизнь, и создали
отображение html-атрибутов в js-свойства и обратно.

Отображение html-атрибута в js-свойство:

1. Берем имя атрибута
2. Откусываем префикс 'data-'
3. Преобразовываем к верхнему регистру символы следующие за '-'
4. Удаляем символы '-'

Отображение js-свойства в html

1. Берем имя свойства
2. Ищем все символы в верхнем регистре (\*с 1ой позиции)
3. Вставляем перед ним '-'
4. Опускаем строку в нижний регистр и прибавляем префикс 'data-

```html
    <div class="track-list">
        <div class="track-list__item" data-duration="2m30s">...</div>
    </div>
```

```js
    var trackList = document.querySelector('.track-list');
    var tracks = [].slice.apply(trackList.querySelectorAll('.trackList__item'));

    tracks.forEach(function (track) {
       console.log(track.dataset.duration);
    });
```

Между data-атрибутами и dataset существует 'связь', то есть при
изменении атрибута меняется свойство, а при изменении свойства изменяется
атрибут.

```js
    var item = document.querySelector('.track-list__item');

    item.dataset.url = 'https://music.yandex.ru/album/2754/track/21168'
    item.getAttribute('data-url') === item.dataset.url;

    item.setAttribute('data-artist') = 'Marilyn Manson'
    item.getAttribute('data-artist') === item.dataset.artist;
```

### Изменение class

При разработке интерфейсов многие компоненты имеют визуальные состояния:

1. Модальное окно открыто/закрыто
2. Элемент когда-то нажимали или нет
3. Любимый трэк пользователя или нет

Для изменение визуального состояния страницы можно поступать следующими способами:

1. Отправить на другую страницу с измененным отображением(старая школа)
2. Удалить часть элементов из html
3. Изменить style у нужных элементов
4. Изменить class

Способы 1-3 хороши, но давай-те поговорим о 4\.

#### Изменение атрибута

class - это тоже атрибут, для его изменения вы можете изменить этот атрибут:

```js
    var track = document.querySelector('.track');

    track.setAttribute('class', 'track track_lovely_yes');
```

Тут нужно обратить внимание, что при изменении class через атрибут вы работаете
со строкой =\> вам все операции по работе с классами нужно писать самим:

1. удаление класса
2. замена класса
3. добавление класса

В качестве домашнего упражнения можете написать мальнькие функции, которые это делают.

#### Изменение свойства

Так же менять класс можно через свойство className

```js
    var track = document.querySelector('.track');

    track.className = 'track track_lovely_yes';
```

Тут такая же ситуация, как и с setAttribute.

#### Работа со свойством classList

После такой работы с классами кажется, что веб очень-очень странный, но
функции для изменения класса пишутся довольно легко.

Шли годы и разработчики стандарта решили уделить время созданию более удобному
интерфейсу для работы с классами, так как пользовательский сценарий по
изменению класса довольно частый.

classList - это свойство, которое есть у всех dom-element.

Для добавления нового класса у элемента нужно воспользоваться методом add:

```js
    var track = document.querySelector('.track');

    track.classList.add('track_lovely_yes');

    track.className;
```

Для удаления класса из элемента нужно воспользоваться методом remove:

```js
    var track = document.querySelector('.track');

    track.classList.remove('track_lovely_yes');

    track.className;
```

Для замены класса из элемента нужно воспользоваться методом toggle:

```js
    var track = document.querySelector('.track');

    track.classList.toggle('hidden', 'shown');

    track.className;
```

Для проверки на наличия класса нужно воспользоваться методом contain:

```js
    var track = document.querySelector('.track');

    track.classList.contain('track');
```

### Изменение DOM

Когда-то давным давно чтобы изменить страницу нужно было перейти по url
с измененной страницей.

Но темные времена прошли и веб стал очень динамичным. При работе с сайтом dom частенько меняется:

1. открытие модальных окон
2. создание таблиц
3. бесконечные ленты
4. обновление страниц без перезагрузки данных

Поэтому очень важно уметь менять dom.

#### Создание новых dom-элементов.

У объекта страницы есть метод createElement, который принимает название tag нового элемента и создает новый element с вашим тэгом.

```js
    var track = document.createElement('div');

    track.className = 'track'
```

Когда вы создаете новый элемент, то он создается в "вакууме" и нигде не отображается.
Чтобы он стал виден на странице его нужно добавить к одному из элементу, который уже отображен на странице.

### Создание текста

Для создание текста нужно воспользоваться методом createTextNode

```js
    var text = document.createTextNode('Ваша любимая песня');
```

### Добавление элемента

Мало создать элемент, но нужно его еще добавить к существующему dom, иначе
от него мало толку.

Для этого существует несколько способоов.

#### Способ первый

Находим какой-то элемент и добавляем справа к его детям новый элемент.

```js
    var track = document.createElement('div');
    var label = document.createElement('div');

    track.appendChild(label);
```

#### Способ второй

Находим элемент и вставляем перед ним.

```js
    var track = document.createElement('div');
    var label = document.createElement('div')
    var duration = document.createElement('div')

    track.appendChild(duration);
    track.insertBefore(label, duration);
```

### Способ третий - заменить существующий элемент

Находим старый элемент, заменяем на новый

```js
    var track = document.createElement('div');
    var label = document.createElement('div');

    track.appendChild(track);

    // какой-то код

    track.replaceChild(document.createTextNode('Ваша любимая песня'), label);
```

### Способ пять

Все способы до этого кажутся очень кропотливыми и если нам предстоит задача
по json нарисовать страничку на клиенте, то такая задача кажется непосильной.
Поэтому есть свойство innerHTML, которое позволяет создавать новые элементы проще.

Это свойство позволяет получить html детей в виде строки.

```js
    var html = document.innerHTML;
```

Так же в свойство innerHTML можно присваивать новый html:

```js
    var track = document.createElement('div');

    track.innerHTML = "<div class='label'>Marilyn Manson</div><div class='star'>5</div>"
```

Но innerHTML опасное свойство, так как это свойство не экранирует html, что очень даже логично,
но это является уязвимостью. Например, XSS.

### Способ шесть

DOM элементы содержат в себе некоторый аналог innerHTML, а именно, innerText.

innerText - это свойство, которое позволяет заменить все содержимое элемента на заэкранированный текст,
то есть если вы вставить некий html-код, то вы на странице увидете код, а не html-элементы.

```js
    var track = document.createElement('div');

    track.innerText = 'Пользовательский ввод';
```

#### Способ семь

Все предыдущие способы по генерации нового html содержимого кажутся 'неприятными'.
И я с вами полностью согласен. Поэтому умные js разработчики написали кучу шаблонизаторов,
которые позволяют создавать html ввиде строки значительно приятнее, а затем
вы можете вставить этот html 5 способом.

https://garann.github.io/template-chooser/

Маленькое домашнее задание придумать свой шаблонизатор! Больше шаблонизаторов!

### Удаление элементов

Для удаления html элементов надо воспользоваться функцией removeChild

```js
    var track = document.querySelector('.track');
    var label = track.querySelector('.label');

    track.removeChild(label);
```

### Добавление нескольких элементов

Бывает, нам нужно добавить не одного ребенка, а сразу N

#### Способ 1

Берем метод appendChild и цикл for

```js
    var track = document.querySelector('.track');

    for(var i = 0; i < 10; i++) {
        var label = document.createElement('div');

        track.appendChild(label)
    }
```

Но важно помнить, что каждый раз когда вы изменяете dom у вас запускаются
внутренние механихмы браузера по обновлению страницы. Следовательно, есть
правило хорошего тона: 'Трогать дом можно, но чуть-чуть';

#### Второй способ

Создаем какой-то dom-элемент, вставляем в него новые элементы и, вставляем в dom.

```js
    var track = document.querySelector('.track');
    var container = document.createElement('div');

    for(var i = 0; i < 10; i++) {
        var label = document.createElement('div');
        container.appendChild(label);
    }

    track.appendChild(container);
```

Этот способ хорош с точки зрения вставки пачки элементов в DOM, но
у вас создается лишний элемент, а мусор в доме держать плохо.

#### Третий способ

Для решение этой проблемы есть специальный тип Node - DOCUMENT\_FRAGMENT.

DocumentFragment - растворяется в DOM при присоединении его к отображаемым элементамю.

Для создание DocumentFragment нам нужно воспользоваться методом createDocumentFragment у document

```js
    var track = document.querySelector('.track');
    var container = document.createDocumentElement();

    for(var i = 0; i < 10; i++) {
        var label = document.createElement('div');
        container.appendChild(label);
    }

    track.appendChild(container);
```

## События

Когда-то давным давно люди думали, что html-страницы нужны только для
публикации научных статей, но время шло и людям захотелось не только
смотреть на текст + картинки, но и как-то взаимодействовать со страницей.
Для этого браузер оснастили механизмом событий, а именно, способностью
сообщать клиентскому коду, что на странице что-то произошло. Например:

1. пользователь тыкнул на кнопку
2. страница загрузилась
3. пользователь начал скролить и так далее.

Сам механизм события довольно сложный, поэтому мы о нем поговорим позже.

### Как подписаться на событие

Как обычно в js существует несколько способов

##### Способ первый

Подписать на событие можно с помощью html-атрибута. Для всех основных
событий имя атрибута: on + имя событие(onclick, onblur, ...) .Обработчик события
записываем в виде строки в разметку или записываем имя глобальной функции

```html
    <button onclick='onClickByButton()'>First</button>
    <button onclick='(function(){alert(2);})()'>Second</button>
```

```js
    function onClickByButton() {
       alert(1);
    }
```



1. Можно добавить только один обработчик;
2. Смешиваем отображение с реализацией;
3. Глобальные функции;
4. Самый простой вариант;

#### Способ второй

В js у всех dom-element есть свойства on + имя событие. Чтобы подписаться на
событие через js нужно присвоить функцию обработчик в атрибут.

Чтобы отписаться от события нужно присвоить в атрибут null.

```html
    <button >First</button>
```

```js
    var button = document.querySelector('button');

    button.onclick = function () {
       console.log('Click!!!');
    }
```



1. Можно добавить только один обработчик;
2. Самый простой вариант;

### способ третий

Разработчики веб стандарта предлагают пользоваться методом addEventListener, который есть у всех dom-element.
Метод принимает три аргумента:

1. eventName имя событие
2. listener - ваш обработчик
3. useCapture - стадия активации обработчика, о которой мы поговорим позже.

```html
    <button >First</button>
```

```js
    var button = document.querySelector('button');
    var callback = function (event) {
     alert('Click')
    }

    button.addEventListener('click', callback, false);
```

### Объект event

Обработчик событий принимает объект event, который в себе содержит всю информацию о произошедшем событии.

1. target - на каком элементе сработало событие
2. currentTarget - на каком элементе в данный момент обрабатывается событие
3. type - название события
4. и другие свойства, которые специфичны для различных событий

### Отписка от событий

У метода addEventListener есть напарник removeEventListener(), который имеет такую же сигнатуру.

```html
    <button >First</button>
```

```js
    var button = document.querySelector('button');
    var callback = function () {
       alert('Click')
    };

    button.addEventListener('click', callback, false);
    button.removeEventListener('click', callback, false);
```

Важно обратить внимание, что при отписке от события нужно передать туже функцию.

Следующий код не будет работать:

```js
    var button = document.querySelector('button');
    button.addEventListener('click', function () {
                                        alert('Click')
                                     }, false);
    button.removeEventListener('click', function () {
                                           alert('Click')
                                        }, false);
```

Зачем может понадобиться отписываться:

1. при изменении состояния приложения(например, контрол заблокирован)

### Механизм работы обработки событий

Вот и настал момент, когда мы примерно представляем интерфейс и мы можем поговорить, о том как устроен механизм обработки событий.

Когда событие возникает на dom елементе, то события вовзникает не только
на нем но и на всех родителях причем. Но обо все по порядку.

Механизм:

1. Кто-то создал событие (например, тыкнул мышкой по кнопке)
2. Берем спискок родителей
3. Начинаем стадию захвата
4. Бежим в цикле по родителям начиная с самого дальнего
5.   * Вызываем обработчики, которые подписаны на стадию захвата
6. Начинаем стадию цели
7. Вызываем все обработчики, которые подписаны не важно на какую стадию
8. Начинаем стадию всплытия
9. Бежим в цикле по родителям начиная с самого ближнего
10.   * Вызываем обработчики, которые подписаны на стадию всплытия

Что мы узнали из механизма:

1. addEventListener - последним параметром принимает стадию захвата/всплытие
2. Список элементов на которых будут вызваны обработчики фиксируется при возникновении события
3. Событие на стадии захвата происходят чуть быстрее(но по историческим причинам все используют стадию всплытия)
4. На цели обработчики вызываются в произвольном порядке.

10 - пунктов в алгоритме очень тяжело поэтому давайте рассмотрим пример

#### Пример с квадратами:

http://jsfiddle.net/851kwgnv/1/

Так давай-те тыкнем по 'красному квадрату':

1. target=red
2. parents = \[html, body, container\]
3. Вызываем обработчики захвата на html -\> body -\> container
4. Вызываем обработчики цели на red
5. Вызываем обработчики стадии всплытия container -\> body -\> html

Затем давай-те тыкнем по 'синему квадрату':

1. target=blue
2. parents = \[html, body, container, red\]
3. Вызываем обработчики захвата на html -\> body -\> container-\>red
4. Вызываем все обработчики на blue
5. Вызываем обработчики стадии всплытия red -\> container -\> body -\> html

Затем давай-те тыкнем по 'зеленому квадрату':

1. target=green
2. parents = \[html, body, container, red, blue\]
3. Вызываем обработчики захвата на html -\> body -\> container-\>red-\> blue
4. Вызываем все обработчики на blue
5. Вызываем обработчики стадии всплытия blue -\> red -\> container -\> body -\> html

### Делегирование событий

Механизм событий позволяет делегировать обработку событий родителю. Для этого
нам нужно сделать следующее:

1. Найти родителя;
2. подписаться на нужное событие в родителе на стадию всплытия;
3. в обработчике отсеять не нужные элементы;

Как будем отсеивать. Все обработчики события вызывают с параметром event.
Параметр event - это объект в котором есть куча полезных вещей:

1. currentTarget - dom-elem на котором вызван обработчик (совпадает с this)
2. target - элемент, по которому реально тыкнули
3. и другие полезные параметры

Тогда делегирование событий может быть реализованно как-то так:

```html
    <form>
        <button data-number=1>Первый</button>
        <button data-number=2>Второй</button>
        <button data-number=3>Третий</button>
    </form>
```

```js
    var button = document.querySelector('form');

    button.addEventListener('click', function (event) {
        if (event.target.tagName !== 'BUTTON') {
          return;
        }
        console.log(event.target.dataset.number);
    }, false);
```

Но возникаем проблема, когда в элементе с которого мы хотим делигировать событие
есть вложенные элементы

```html
    <form>
        <button data-number=1><span>Первый</span></button>
        <button data-number=2><span>Второй</span></button>
        <button data-number=3><span>Третий</span></button>
    </form>
```

```js
    var button = document.querySelector('form');

    button.addEventListener('click', function (event) {
        if (event.target.tagName !== 'BUTTON') {
          return;
        }
        console.log(event.target.dataset.number);
    }, false);
```

Проблема в том, что target будет указывать на кликнутый элемент, то есть
на самый вложенные, в нашем случае span. Чтобы эту проблему решить нужно
маленько 'прокачать' наш обработчик:

```html
    <form>
        <button data-number=1><span>Первый</span></button>
        <button data-number=2><span>Второй</span></button>
        <button data-number=3><span>Третий</span></button>
    </form>
```

```js
    var button = document.querySelector('form');

    button.addEventListener('click', function (event) {
        var ourTarget = event.target.closest('button');

        if (!ourTarget) {
          return;
        }

        console.log(ourTarget.dataset.number);
    }, false);
```

Что мы сделали:

1. взяли элемент по которому тыкнули (event.target)
2. нашли от него ближайший родитель/самого себя, нашей реальной цели
3. если реальная цель не нашлась, то беда и тыкнули по чему-то другому
4. иначе мы нашли что надо.

Ясное дело, что постоянно писать такие обработчики накладно, поэтому
рекондую вам написать функцию delegate, которая принимает:

1. parentTarget - элемент на который делегируют ответственность за обработку событий
2. selector - селектор детей с которых делегируется ответственность
3. listener - обработчик



1. Не нужно подписывать на каждый элемент при добавлении.
2. Не нужно отписываться от событий при удалении элементов.
3. Один обработчик вместо N.
4. Обработчик становится чуть сложнее.
5. Обработчик будет срабатывать, когда мог бы не срабатывать.

Я рекомендую иметь на вооружении этот инструмент, так как он помогает избавиться
от кучи проблем.

Так же вам может показаться, что было бы не плохо повешать delegate на body,
чтобы иметь минимальное количество обработчиков или вообще один. Но
при таком подходе обработчик будет срабатывать очень часто и это может
повлиять на производитьльность. Но такой подход используют некоторые framework.

### stopPropagation

Бывает в одном из обработчиков мы понимаем, что мы все обработали и вызов дальнейших обработчиков не нужен.
Тогда нам на помощь приходитм метод stopPropagation, который есть у объекта event.

```html
    <div class='container'>
      <div class='block'></div>
    </div>
```

```js
    var container = document.querySelector('.container');
    var block = document.querySelector('.block');

    block.addEventListener('click', function (event) {
        console.log(1);
        event.stopPropagation();
    }, false);

    container.addEventListener('click', function (event) {
        console.log('этот текст никогда не распечатается');
    }, false);
```

!!!stopPropagation полность останавливает обработку события.

### preventDefault

Некоторые html-элементы имеют некоторое действие по умолчанию, при возникновении
определенных событий. Например, ссылка при клике осуществляет переход по ссылке.

Чтобы этого избежать у объекта event есть метод preventDefault.

```html
    <a class='link' href='yadnex.ru'>
      какая классная ссылка
    </a>
```

```js
    var link = document.querySelector('.link');

    link.addEventListener('click', function (event) {
        event.preventDefault();
    }, false);
```
