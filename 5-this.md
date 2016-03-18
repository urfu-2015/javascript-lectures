# this

- [Введение](5-this.md#%D0%92%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-)
- [Исполняемый код и контекст исполнения](5-this.md#%D0%98%D1%81%D0%BF%D0%BE%D0%BB%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%B9-%D0%BA%D0%BE%D0%B4-%D0%B8-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BA%D1%81%D1%82-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F-)
- [This в глобальном коде](5-this.md#this-%D0%B2-%D0%B3%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%BC-%D0%BA%D0%BE%D0%B4%D0%B5-)
- [This в коде Node.js модуля](5-this.md#this-%D0%B2-%D0%BA%D0%BE%D0%B4%D0%B5-nodejs-%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D1%8F-)
- [Strict mode](5-this.md#strict-mode-)
- [This в коде функции](5-this.md#this-%D0%B2-%D0%BA%D0%BE%D0%B4%D0%B5-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-)
    - [Простой вызов функции](5-this.md#%D0%9F%D1%80%D0%BE%D1%81%D1%82%D0%BE%D0%B9-%D0%B2%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-)
    - [Вызов функции, как метод объекта  ](5-this.md#%D0%92%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%BA%D0%B0%D0%BA-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0-)
    - [Вызов функции, используя call](5-this.md#%D0%92%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-call-)
    - [Вызов функции, используя apply](5-this.md#%D0%92%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-apply-)
    - [Вызов функции, в качестве callback](5-this.md#%D0%92%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%B2-%D0%BA%D0%B0%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%B5-callback-)
        - [Cохранение this в лексическом окружении](5-this.md#%D0%A1%D0%BF%D0%BE%D1%81%D0%BE%D0%B1-%D0%BF%D0%B5%D1%80%D0%B2%D1%8B%D0%B9--%D1%81%D0%BE%D1%85%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-this-%D0%B2-%D0%BB%D0%B5%D0%BA%D1%81%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%BC-%D0%BE%D0%BA%D1%80%D1%83%D0%B6%D0%B5%D0%BD%D0%B8%D0%B8-)
        - [Передача this в качестве аргумента в метод массива](5-this.md#%D0%A1%D0%BF%D0%BE%D1%81%D0%BE%D0%B1-%D0%B2%D1%82%D0%BE%D1%80%D0%BE%D0%B9--%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0-this-%D0%B2-%D0%BA%D0%B0%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%B5-%D0%B0%D1%80%D0%B3%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D0%B0-%D0%B2-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B0-)
        - [Привязка this специальным методом `.bind()`](5-this.md#%D0%A1%D0%BF%D0%BE%D1%81%D0%BE%D0%B1-%D1%82%D1%80%D0%B5%D1%82%D0%B8%D0%B9--%D0%BF%D1%80%D0%B8%D0%B2%D1%8F%D0%B7%D0%BA%D0%B0-this-%D1%81%D0%BF%D0%B5%D1%86%D0%B8%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%BC-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%BE%D0%BC-bind-)
    - [Вызов функции, в качестве конструктора](5-this.md#%D0%92%D1%8B%D0%B7%D0%BE%D0%B2-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%B2-%D0%BA%D0%B0%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%B5-%D0%BA%D0%BE%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D0%BE%D1%80%D0%B0-)
- [This в eval коде](5-this.md#this-%D0%B2-eval-%D0%BA%D0%BE%D0%B4%D0%B5-)
- [ES2015 This в стрелочных функциях](5-this.md#es2015-this-%D0%B2-%D1%81%D1%82%D1%80%D0%B5%D0%BB%D0%BE%D1%87%D0%BD%D1%8B%D1%85-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F%D1%85--)
- [ES2016 Оператор `::`](5-this.md#es2016-%D0%9E%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80--)
- [Ссылки](5-this.md#%D0%A1%D1%81%D1%8B%D0%BB%D0%BA%D0%B8-)

## Введение [⇡](5-this.md#this)

С конструкцией *this* (или реже *self*) мы часто сталкиваемся во многих языках программирования.

Например в *C++*:

```js
class User {
public:
    int age=30;

    void showAge() {
        std::cout << this->age;
    };
};

int main() {
    User* sergey = new User;

    sergey->showAge();
}
```

Или в *java*:

```js
public class User {
    private int age = 30;

    public void showAge() {
        System.out.println(this.age);
    }
}

public static void main(String []args){
    User sergey = new User();

    sergey.showAge();
}
```

В примере мы объявляем класс `User`, создаём его экземпляр (или объект другими словами) `sergey`.
Затем мы выводим возраст пользователя, вызывая метод объекта `showAge`.
И чтобы получить доступ к полю объекта – `age`, в самом методе мы используем *this*.

В большинстве языков *this* обладает тремя ключевыми свойствами:
- является ключевым словом языка – его нельзя использовать в качестве индентификатора;
- указывает (ссылается) на текущий объект (экземпляр класса);
- его нельзя перезаписать, иными словами, присвоить ему другое значение.

Такая языковая конструкция, с теми же свойствами, есть и в *javascript*:

```js
function User () {
    return {
        age: 30,

        showAge: function () {
            console.log(this.age);
        }
    }
}

var sergey = new User();

sergey.showAge();
```

Похожим образом мы объявляем конструктор объектов `User`, создаём объект `sergey`,
и в методе `showAge` при помощи *this* получаем доступ к полю `age` созданного объекта.

Но, как вы уже наверное догадываетесь, в javascript есть ряд долнительных особенностей.  
Например, мы легко можем использовать *this* за пределами объектов.

Запустив в браузере следующий код, мы не получим ошибку, а получим ширину окна браузера:

```js
console.log(this.innerWidth);
```

А в Node.js мы можем получить её версию вот так:

```js
> this.process.version
'v0.12.7'
```

Чтобы определить на что ссылается *this* в каждый момент времени работы javascript программы,
в начале разберёмся, как интерпретатор читает вашу программу.

## Исполняемый код и контекст исполнения [⇡](5-this.md#this)

Итак, читая вашу программу, интерпретатор различает в ней участки кода четырёх типов:

- Код функции (*Function code*) - код тела функции, не включая вложенные функции;
- Eval код (*Eval code*) - код, передаваемый в виде строки в функцию eval;
- Код модуля (*Module code*) - код модуля, не включая вложенные функции;
- Глобальный код (*Global code*) - остальной код, не включающий в себя код модулей и функций.

Рассмотрим пример:

```js
// Глобальный код

function add(a, b) {
    // Код функции
    return a + b;
}

function evilAdd(a, b) {
    // Код функции

    return eval(
        // Eval код
        'a+b'
    );
}

var sum = add(4, 2);
```

В примере мы видим один участок глобального кода, два участка с кодом функции и ещё один с кодом eval.
Чтобы правильно прочитать и понять каждый участок, интепретатору необходимо серьёзно подготовиться.

Вначале, перед чтением участка кода, он создаёт контекст исполнения (*Execution Context*).
Его можно просто рассматривать как специальный объект, который хранит необходимые для интрепретации сведенья.
Затем интепретатор предварительно просматривает весь участок кода и заполняет этот объект.

Самый первый участок, который интерепретатор встречает – всегда глобальный код. И перед его интепретацией создаётся первый контекст – глобальный (*Global Execution Context*). В нашем случае глобальный контекст будет выглядеть условно так:

```js
globalContext = {
    lexicalEnvironment: {
        sum: undefined,
        add: <ссылка на функцию 'add'>,
        evilAdd: <ссылка на функцию 'evilAdd'>
    }
};
```

Так, в специальное свойство контекста *lexicalEnvironment* (лексическое окружение) складывается информация о переменных и функциях:

* для каждой объявленной в коде переменной создаётся свойство с именем переменной и значением `undefined`;
* для каждой декларации функции (*Function Declaration*) создаётся свойство с именем функции и значением – ссылкой на функцию.

При входе на следующий участок – вызов функции `add`, создаётся новый контекст.

```js
functionContext = {
    lexicalEnvironment: {
        x: 4,
        y: 2
    },
    parentLexicalEnvironment: <ссылка на globalContext.lexicalEnvironment>
};
```

В него добавляется ссылка на родительское лексическое окружение – таким образом интерпретатор находит переменные в замыкании. И для каждого параметра функции создаётся свойство с именем и значением этого параметра.

Все контексты складываются в  стек (*Execution Context Stack*).
Внизу – всегда глобальный, а наверху - всегда текущий (*Running Execution Context*)

```js
contextsStack = [
    globalContext,
    ...
    functionContext // Running Execution Context
]
```

В один момент времени исполняется только **один** участок код, и мы находимся только в **одном** контексте.
Как только участок кода заканчивается, контекст удаляется из стека и мы возвращаемся к предыдущему.

А где здесь *this*, спросите вы? A *this* – это всего лишь ещё одно специальное свойство каждого контекста исполнения. И в него, **перед интепретацией** каждого участка кода, записывается значение.

```js
globalContext = {
    lexicalEnvironment: {
        sum: undefined,
        add: <ссылка на функцию 'add'>,
        evilAdd: <ссылка на функцию 'evilAdd'>
    },
    this: <ссылка на объект>
};
```

А значение это напрямую **зависит от трёх факторов**:
- тип участка кода;
- как мы попали на этот участок;
- и в каком режиме работает интепретатор (строгом или нет).

В большинстве случаев в *this* записывается ссылка на объект, но не всегда.

Начнём с простого, с глобального кода.

## This в глобальном коде [⇡](5-this.md#this)

При входе в глобальный код, в *this* записывается ссылка на глобальный объект (*Global object*) –
это такой специальный объект, который создаётся перед интерпретацией программы
и хранит специфические свойства и методы окружения, где ваш код запускается.

Например, в браузере – это объект `window`. Как мы уже видели, в нём мы можем подсмотреть ширину окна:

```js
console.log(this.innerWidth);
```

Или вызвать метод `close` и закрыть окно:

```js
this.close();
```

Что равносильно вызову:

```js
window.close();
```

Объект `console` и его метод `log`, на самом деле, так же храниться в глобальном объекте:

```js
this.console.log('Hello, World!');
```

В Node.js так же есть глобальный объект, который, например, хранит информацию о версии Node.js:

```js
> this.process.version
'v0.12.7'
```

Теперь расмотрим код модулей. Полноценно мы пока можем использовать их только в Node.js.

## This в коде Node.js модуля [⇡](5-this.md#this)

В коде модулей Node.js, в *this* записывается ссылка на объект,
который мы собираемся экспортировать из модуля.

Напишем простой модуль, который вернёт нам информацию о 2015 годе.

```js
// ./year-2015.js

// В module.exports кладётся то, что будет торчать наружу
module.exports.weeks = 54;
module.exports.days = 366;

// Так как this ссылается на module.exports, мы можем писать так:
this.isLeap = true;  // Год високосный
```

```js
// ./client.js

// Подключаем модуль из файла ./year-2015.js
var year2015 = require('./year-2015.js');

console.log(year2015.isLeap);
// true
```

Пока нам хватало одного фактора, чтобы определить значение *this*: тип участка кода.

Но при входе в код функций, понадобятся и два остальных:
- как мы попали в код функций – как функция была вызвана в родительском участке кода;
- и в каком режиме работает интепретатор (строгом или нет).

У интепретатора есть всего два режима - обычный режим (по умолчанию) и строгий (*Strict mode*).

## Strict mode [⇡](5-this.md#this)

Появление стандарта EcmaScript 5 (ES5) помимо добавления в язык новых возможностей,
внесло в него и ряд исправлений, которые могут привести к тому, что старый код перестанет работать.

Чтобы этого не случилось, по умолчанию опасные изменения выключены.
А для того, чтобы обозначить, что код соответствует современному стандарту,
просто указываем специальную директиву **use strict** в начале файла:

```js
'use strict';

function add(a, b) {
    return a + b;
}
```

Так же его можно включить только для кода отдельной функции, указав директиву в начале.
Её действие распространиться и на все вложенные функции.

```js
function add(a, b) {
    'use strict';

    return a + b;
}
```

*Strict mode* вносит несколько дополнительных ограничений на ваш код.
Например, в нём зарезевировано больше ключевых слов:

```js
function func() {
    var eval = 42; // OK
}

function strictFunc() {
    'use strict';
    var eval = 42; // SyntaxError: Unexpected eval or arguments in strict mode
}
```

Или в строгом режиме мы не можем объявлять переменные без указания `var`, `let` или `const`:

```js
randomNumber = 4; // OK, сохранится в глобальный объект!
```

```js
'use strict';

randomNumber = 4; // ReferenceError: randomNumber is not defined
```

Далее мы увидим, что строгий режим накладывает ограничения и на *this*. Вернёмся теперь к функциям.

## This в коде функции [⇡](5-this.md#this)

Итак, на этапе установки контекста в *this* записывается значение
в зависимости от того **как функция была вызвана** в родительском участке кода и
от того в **каком режиме работает интепретатор** (строгом или нет).

Начнём с простого вызова функции.

#### Простой вызов функции [⇡](5-this.md#this)

```js
function func() {
    return this;
}

func(); // window
```

В **обычном режиме** *this* будет указывать на глобальный объект (в браузере – это window).

```js
function strictFunc() {
    'use strict';

    return this;
}

strictFunc(); // undefined
```

В **в строгом** *this* будет не определён, то есть фактически равен *undefined*.

> Это можно записать как общее правило, если не удаётся определить чему будет равен
> *this*, то в **в строгом режиме** будет *undefined*, а **в обычном режиме** –
> глобальный объект.

Теперь рассмотрим вызов функции, как вызов метод объекта.

#### Вызов функции, как метод объекта [⇡](5-this.md#this)

Определим простой объект:

```js
var sergey = {
    // Свойство объекта
    age: 30,

    // И его метод
    getAge: function () {
        'use strict';

        return this.age;
    }
}
```

При вызове функции, как метода объекта (как бы от лица объекта),
перед интерпретацией кода в *this* записывается ссылка на этот объект.
И код `return this.age` будет интепретироваться как `return sergey.age`:

```js
sergey.getAge(); // 30
```

Давайте мы не надолго позаимствуем метод (*borrow method*) у объекта и вызовем его отдельно:

```js
var sergey = {
    // Свойство объекта
    age: 30,

    // И его метод
    getAge: function () {
        'use strict';

        return this.age;
    }
}

var getAge = sergey.getAge;

getAge();
```

Что мы увидим в ответ?

Мы увидим ошибку:

```js
getAge(); // TypeError: Cannot read property 'age' of undefined
```

Мы помним, что важно не где эта функция опрделена, а **как функция была вызвана**. Позаимствовав метод мы воспользовались уже простым вызовом функции, а не вызовом как метод объекта. А значит при вызове, учитывая строгий режим, *this* будет не определён – равен *undefined*.

Ещё один пример. Что мы получим если выполним этот код в браузере?

```js
var block = {
    innerHeight: 300,

    getHeight: function () {
        return this.innerHeight;
    }
}

var getHeight = block.getHeight;

getHeight();
```

Вернётся высота окна, а не высота блока (300), так как в не строгом режиме,
при простом вызове функции *this* будет указывать на глобальный объект,
то есть window.

Но заимствовать методы, опасная, но не такая уж и плохая идея.

#### Вызов функции, используя call [⇡](5-this.md#this)

Мы все помним замечательный объект `sergey` с методом `getAge`

```js
var sergey = {
    age: 30,

    getAge: function () {
        'use strict';

        return this.age;
    }
}
```

У вот у объекта `oleg` всё не так хорошо :( У него нет своего метода `getAge`:

```js
var oleg = {
    age: 25
}
```

Мы знаем, что можем позаимствовать удобный метод у Сергея,
но простой вызов заимстованного метода приводит к ошибкам.
Что делать? Как его вызвать от лица Олега?  

К счастью, в javascript у каждой функции есть специальный метод:  
`func.call(<thisArg>, <arg1>, <arg2>, ...)`;

Он означает буквально следующее:  
«Мы вызываем `func` с параметрами `arg1`, `arg2`, ..., в которой *this* равен `thisArg`»

```js
// Заимствуем метод
var getAge = sergey.getAge;

// Безопасно вызываем от лица Олега
getAge.call(oleg); // 25
```

При входе в код функции `getAge`, в *this* запишется ссылка на объект `oleg`.
Поэтому, код `return this.age` будет интепретирован как `return oleg.age`;

В лекции о функциях вы уже встречались с более практическим применением этой формы вызова:

```js
function func() {
  var args = Array.prototype.slice.call(arguments);
}
```

Мы заимствуем у объекта `Array` метод `slice` и вызываем его от лица `arguments`, у которого этого метода нет.
Вызов метода `.slice()` без параметров возвращает копию `arguments`, но уже как полноценный массив с плюшками.

А что если нам не удобно передавать параметры через запятую в метод call?
Мы не знаем их количество и хотим передать массив?

#### Вызов функции, используя apply [⇡](5-this.md#this)

И на это случай в javascript есть специальный метод:    
`func.apply(<thisArg>, [ <arg1>, <arg2>, ...])`;

Он означает буквально следующее:  
«Мы вызываем `func` с **массивом** параметров `arg1`, `arg2`, ..., в которой *this* равен `thisArg`»

Основное применение `.apply()` – возможность передать параметры массивом,
если сам метод такого не предусматривает. Например метод `.push()` у массивов:

```js
var fruits = [];
var citrus = ['Orange', 'Lemon', 'Mandarin'];

// Добавляем по одному
fruits.push('Banana');
fruits.push('Apple');

// Сразу массивом
fruits.push.apply(fruits, citrus);
```

Или например, нам нужно выбрать максимальное число из массива:

```js
var numbers = [10, 42, 30, 5, 16];
```

Но, подходящий метод `Math.max` принимает аргументы только через запятую:

```js
Math.max(10, 42, 30, 5, 16); //42
```

При помощи `.apply()` это ограничение можно обойти:

```js
Math.max.apply(Math, numbers); //42
```

В отличие от `.push()`, метод `.max()` не использует внутри себя *this*,
поэтому мы можем передать в качестве первого параметра всё, что угодно:

```js
Math.max.apply(null, numbers); //42
```

Как вы уже знаете функции можно не только вызывать, но и передавать в качестве колбэка.

#### Вызов функции, в качестве callback [⇡](5-this.md#this)

Рассмотрим пример:

```js
'use strict';

var person = {
    name: 'Sergey',
    items: ['keys', 'phone', 'banana'],

    showItems: function () {
        this.items.forEach(function (item) {
            console.log(this.name + ' have ' + item);
        });
    }
}

person.showItems();
```

В метод `.forEach()` массива фруктов мы передаём в качестве колбэка функцию,  
которая для каждого фрукта в наличии выводит строку вида «Sergey has Apple».

К сожалению, в текущем виде данный код не будет работать верно.
Мы помним, что важно не где эта функция опрделена, а **как функция была вызвана**.

Вызов функции в качестве колбэка не отличается от простого вызова,
А значит *this* в нашем примере будет не определён.
И javascript, к счастью, предлагает нам три способа решения данной проблемы.

##### Способ первый – сохранение this в лексическом окружении [⇡](5-this.md#this)

Мы можем сохранить *this* в лексическом окружении (*lexicalEnvironment*) родительского кода.
И чтобы метод в нашем примере работал, мы можем его переписать так:

```js
var person = {
    //...
    showItems: function () {
        var _this = this; // Ещё часто используют self или that

        this.items.forEach(function (item) {
            console.log(_this.name + ' have ' + item);
        });
    }
}
```

Вызов метода `person.showItems` создаст новый контекст и положит его в стек:

```js
showItemsContext = {
    lexicalEnvironment: {
        _this: undefined
    },
    parentLexicalEnvironment: <ссылка на globalContext.lexicalEnvironment>
    this: <Ссылка на `person`> // Так как мы вызвали функцию, как метод объекта
};

contextsStack = [
    globalContext,
    showItemsContext // Running Execution Context
]
```

Затем начинается интепретация кода – в `_this` присваивается ссылка, хранящаяся в *this*:

```js
showItemsContext = {
    lexicalEnvironment: {
        _this: <Ссылка на `person`>
    },
    parentLexicalEnvironment: <ссылка на globalContext.lexicalEnvironment>
    this: <Ссылка на `person`>
};

contextsStack = [
    globalContext,
    showItemsContext // Running Execution Context
]
```

Чуть погодя вызывается колбэк, перед интрепретацией кода которого, создаётся новый контекст:

```js
callbackContext = {
    lexicalEnvironment: {
        item: 'keys'
    },
    parentLexicalEnvironment: <ссылка на showItemsContext.lexicalEnvironment>
    this: undefined // Так как мы в строгом режиме интепретации
};

showItemsContext = {
    lexicalEnvironment: {
        _this: <Ссылка на `person`>
    },
    parentLexicalEnvironment: <ссылка на globalContext.lexicalEnvironment>
    this: <Ссылка на `person`> // Так как мы вызвали функцию, как метод объекта
};

contextsStack = [
    globalContext,
    showItemsContext,
    callbackContext // Running Execution Context
]
```

Читая строчку `console.log(_this.name + ' have ' + item);` интепретатор подсмотрит значение `_this`
в лексическом окружении родителя `showItemsContext.lexicalEnvironment`.

##### Способ второй – передача this в качестве аргумента в метод массива [⇡](5-this.md#this)

Ряд методов массива, в качестве второго параметра принимают значение *this*:
`.map()`, `.every()`, `.some()`, `.forEach()`, `.filter()`, `.find()`, `.findIndex()`.

Поэтому мы можем просто передать в колбэк нужное значение *this*:

```js
var person = {
    //...
    showItems: function () {
        var _this = this;

        this.items.forEach(function (item) {
            console.log(this.name + ' have ' + item);
        }, _this);
    }
}
```

Или ещё проще:
```js
var person = {
    //...
    showItems: function () {
        this.items.forEach(function (item) {
            console.log(this.name + ' have ' + item);
        }, this);
    }
}
```

##### Способ третий – привязка this специальным методом `.bind()` [⇡](5-this.md#this)

Последний способ решить проблему – привязать контекст методом `.bind()`:  
`var bindedFunc = func.bind(<thisArg>, <arg1>, ...)`;

Метод просто возращает новую функцию `bindedFunc`,
которая внутри себя всегда вызвает `func` c равным `thisArg`.

Таким образом мы можем исправить наш код так:

```js
var person = {
    //...
    showItems: function () {
        var callback = function (item) {
            console.log(this.name + ' have ' + item);
        };

        var bindedCallback = callback.bind(this);

        this.items.forEach(bindedCallback);
    }
}
```

В примере мы привязали наш колбэк к *this* родительского контекста.

На самом деле в `.bind()` нет большой магии, так как внутри он использует `.apply()`,
и простую версию легко можно реализовать самим:

```js
Function.prototype.bind = function(_this) {
    var fn = this; // Берём функцию

    return function () {
        var args = [].slice.call(arguments);

        // И вызываем функцию от лица _this с перданными аргументами
        return fn.apply(_this, args);
    };
});
```

> Мы рекомендуем применить второй способ предпочтительнее и если нет возможности
> то третий. А в ES2015 использовать стрелочные функции

#### Вызов функции, в качестве конструктора [⇡](5-this.md#this)

Ещё одна роль для функций в javascript – конструктор объектов.
В начале лекции мы уже приводили пример с ним:

```js
function User () {
    return {
        age: 30,

        showAge: function () {
            console.log(this.age);
        }
    }
}

var sergey = new User();

sergey.showAge(); // 30
```

В конструктор можно передавать значения для инициализации:

```js
function User (age) {
    return {
        age: age,

        showAge: function () {
            console.log(this.age);
        }
    }
}

var sergey = new User(30);

sergey.showAge(); // 30
```

Если функция была вызвана, как конструктор `new User(30)`, то перед интепретацией
в *this* будет записана ссылка на создаваемый объект, который потом вернётся
и запишется в переменную `sergey`.

Рассмотрим последний из четырёх типов кода – *Eval код*.

## This в eval коде [⇡](5-this.md#this)

Функция `eval()` позволяет нам выполнить код,
переданный ей в виде строки и вернуть последнее вычисленное выражение:

```js
var a = 12;
eval('a + 5'); // 17
```

Рассмотрим пример с использованием *this*:

```js
var person = {
    name: 'Sergey',
    showName: function () {
        return eval('this.name');
    }
}

person.showName(); // Sergey
```

Как мы уже знаем перед интепретацией *Eval кода* создаётся новый контекст.
Так вот созданный контекст наследует значение *this* из родительского контекста,
то есть контекста метода `showName`.

Такое использование `eval()` называется **прямым вызовом** (*Direct Call*),
но есть ещё и **косвенный вызов** (*Indirect call*):

```js
var person = {
    name: 'Sergey',
    showName: function () {
        var evil = eval;

        return evil('this.name');
    }
}

person.showName(); // ""
```

В этом случае *this* будет указывать на глобальный объект (в браузере `window`).

## ES2015 This в стрелочных функциях `=>` [⇡](5-this.md#this)

В спецификации ES2015 (или ES6) появилась стрелочная запись функций-выражений (*Function Expressions*):

```js
'use strict';

var person = {
    name: 'Sergey',
    items: ['keys', 'phone', 'banana'],

    showItems: function () {
        this.items.forEach(item => {
            console.log(this.name + ' have ' + item);
        });
    }
}

person.showItems();
```

Помимо сокращённой записи, стрелочные функции обладают одной полезной нам особенностью
– перед своей интепретацией они наследуют *this* из родительского лексического окружения (да, прямо как *eval*);

Поэтому нет необходимости использовать `.bind()`  
или самим сохранять *this* в лексическом окружении `var _this = this;`.

Мы можем использовать их уже сейчас в Node.js (io.js) 4+.
А также в браузерах: Firefox 40+, Chrome 45+, Opera 32+, Edge

## ES2016 Оператор `::` [⇡](5-this.md#this)

Заглянем немного в будущее.

В следующей версии спецификации ES2016 (или ES7) активно обсуждается новый оператор `::`,  
который призван заменить собой употребление `.bind()`, `.call()`, and `.apply()`

Вместо `.bind()`:
```js
let log = ::console.log; // console.log.bind(console)
```

А вместо `.call()`:
```js
user::getAge(); // getAge.call(user);
```

Пока эта возможность на стадии обсуждения и не включена в спецификацию.

## Ссылки [⇡](5-this.md#this)

**Спецификации**

- ECMAScript® Language Specification  
  http://www.ecma-international.org/ecma-262/5.1/

- ECMAScript® 2015 Language Specification  
  http://www.ecma-international.org/ecma-262/6.0/

**О контексте исполнения и исполняемом коде**

- What is the Execution Context & Stack in JavaScript? (David Shariff)  
  http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/

- Тонкости ECMA-262-3. Часть 1. Контексты исполнения (Дмитрий Сошников)  
  http://dmitrysoshnikov.com/ecmascript/javascript-the-core/#execution-context-stack

- JavaScript. The core (Dmitry Soshnikov)  
  http://dmitrysoshnikov.com/ecmascript/javascript-the-core/#execution-context-stack  
  http://dmitrysoshnikov.com/ecmascript/javascript-the-core/#execution-context

**О лексическом окружении**

- ECMA-262-5 in detail. Chapter 3.2. Lexical environments (Dmitry Soshnikov)  
  http://dmitrysoshnikov.com/ecmascript/es5-chapter-3-2-lexical-environments-ecmascript-implementation/

**О строгом режиме интепретации (Strict mode)**

- ECMA-262-5 in detail. Chapter 2. Strict Mode (Dmitry Soshnikov)  
  http://dmitrysoshnikov.com/ecmascript/es5-chapter-2-strict-mode/

**О this**

- This (Mozilla Developer Network)  
  https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this

- Методы объектов и контекст вызова (Илья Кантор)
  https://learn.javascript.ru/object-methods

- How does the “this” keyword work? (Daniel Trebbien)    
  http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work/3127440#3127440

- JavaScript’s “this”: how it works, where it can trip you up (Dr. Axel Rauschmayer)   
  http://www.2ality.com/2014/05/this.html

- Тонкости ECMA-262-3. Часть 3. This (Дмитрий Сошников)  
  http://dmitrysoshnikov.com/ecmascript/ru-chapter-3-this/

- You Don't Know JS: this & Object Prototypes, Chapter 1: this Or That? (Kyle Simpson)  
  https://github.com/getify/You-Dont-Know-JS/blob/master/this%20&%20object%20prototypes/ch1.md

- You Don't Know JS: this & Object Prototypes, Chapter 2: this All Makes Sense Now! (Kyle Simpson)  
  https://github.com/getify/You-Dont-Know-JS/blob/master/this%20&%20object%20prototypes/ch2.md

**О call, apply и bind**

- Явное указание this: «call», «apply» (Илья Кантор)  
  https://learn.javascript.ru/call-apply

- Привязка контекста и карринг: «bind» (Илья Кантор)  
  https://learn.javascript.ru/bind

- More Control over Function Calls: call(), apply(), and bind() (Dr. Axel Rauschmayer)  
  http://speakingjs.com/es5/ch15.html#_more_control_over_function_calls_call_apply_and_bind  

- JavaScript’s Apply, Call, and Bind Methods are Essential for JavaScript Professionals  
  http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/

- Functional JavaScript, Part 3: .apply(), .call(), and the arguments object (Leland Richardson)  
  https://web.archive.org/web/20150317052230/http://tech.pro/tutorial/2010/functional-javascript-part-3-apply-call-and-the-arguments-object

- Functional JavaScript, Part 4: Function Currying (Leland Richardson)  
  https://web.archive.org/web/20150406155703/http://tech.pro/tutorial/2011/functional-javascript-part-4-function-currying

**О this в обработчиках DOM событий**

- JavaScript ‘this’ and Event Handlers (Craig Buckler)  
  http://www.sitepoint.com/javascript-this-event-handlers/

**ES2015 О this в cтрелочных функциях `=>`**

- ECMAScript 6 solution: arrow functions (Dr. Axel Rauschmayer)  
  http://exploringjs.com/es6/ch_arrow-functions.html#leanpub-auto-ecmascript-6-solution-arrow-functions

- JavaScript’s “this”: how it works, where it can trip you up (Dr. Axel Rauschmayer)   
  http://www.2ality.com/2014/05/this.html  

**ES2016 Об операторе `::`**

- Proposal of ECMAScript Function Bind Syntax  
  https://github.com/zenparsing/es-function-bind

- JavaScript ES7 Function Bind Syntax  
  http://blog.jeremyfairbank.com/javascript/javascript-es7-function-bind-syntax/  
