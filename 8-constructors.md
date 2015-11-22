# Конструкторы

- [Строим абстракции!](8-constructors.md#%D0%A1%D1%82%D1%80%D0%BE%D0%B8%D0%BC-%D0%B0%D0%B1%D1%81%D1%82%D1%80%D0%B0%D0%BA%D1%86%D0%B8%D0%B8-)
  - [Делегирование](8-constructors.md#%D0%94%D0%B5%D0%BB%D0%B5%D0%B3%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-)
  - [Свойство `prototype`](8-constructors.md#%D0%A1%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%BE-prototype-)
  - [Контекст исполнения в функции-конструкторе](8-constructors.md#Контекст-исполнения-в-функции-конструкторе-)
  - [Свойство `constructor`](8-constructors.md#%D0%A1%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%BE-constructor-)
- [Механика наследования](8-constructors.md#%D0%9C%D0%B5%D1%85%D0%B0%D0%BD%D0%B8%D0%BA%D0%B0-%D0%BD%D0%B0%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-)
  - [Первая попытка](8-constructors.md#%D0%9F%D0%B5%D1%80%D0%B2%D0%B0%D1%8F-%D0%BF%D0%BE%D0%BF%D1%8B%D1%82%D0%BA%D0%B0-)
  - [Вторая попытка](8-constructors.md#%D0%92%D1%82%D0%BE%D1%80%D0%B0%D1%8F-%D0%BF%D0%BE%D0%BF%D1%8B%D1%82%D0%BA%D0%B0-)
  - [`Object.create`](#objectcreate-)
    - [Полуполифил](8-constructors.md#Полуполифил-)
    - [`Object.create(null)`](#objectcreatenull-)
  - [Успешная попытка](8-constructors.md#%D0%A3%D1%81%D0%BF%D0%B5%D1%88%D0%BD%D0%B0%D1%8F-%D0%BF%D0%BE%D0%BF%D1%8B%D1%82%D0%BA%D0%B0-)
  - [Реиспользуем конструкторы](8-constructors.md#%D0%A0%D0%B5%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D0%BC-%D0%BA%D0%BE%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D0%BE%D1%80%D1%8B-)
- [Переопределение методов из цепочки](8-constructors.md#%D0%9F%D0%B5%D1%80%D0%B5%D0%BE%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%BE%D0%B2-%D0%B8%D0%B7-%D1%86%D0%B5%D0%BF%D0%BE%D1%87%D0%BA%D0%B8-)
- [Инспектирование связей между объектами](8-constructors.md#%D0%98%D0%BD%D1%81%D0%BF%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B2%D1%8F%D0%B7%D0%B5%D0%B9-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0%D0%BC%D0%B8-)
  - [`instanceof`](#instanceof-)
  - [`Object.prototype.isPrototypeOf()`](#objectprototypeisprototypeof-)
- [Связывание объектов](8-constructors.md#%D0%A1%D0%B2%D1%8F%D0%B7%D1%8B%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%B2-)

## Строим абстракции! [⇡](8-constructors.md#Конструкторы)
К этому моменту мы узнали, как работает механизм цепочки прототипов. Теперь нам предстоит узнать, каким образом мы можем строить эти цепочки.

В этой части лекции мы вернемся к нашему примеру про студентов и лекторов. Разберемся, как нам создавать такие объекты.

У нас была выделена сущность студента:
```js
var student = {
    age: 20,
    name: 'John',
    sleep: function () {},
    getAge: function () {},
    getName: function () {}
}
```

Чтобы создавать такого рода объекты, нам нужна будет функция, примерно такая:
```js
function createStudent(name, age) {
    return {
        age: age,
        name: name,
        sleep: function () { console.log('hrrr...') },
        getAge: function () { return this.age; },
        getName: function () { return this.name; }
    };
}
var john = createStudent('John', 20);
var carl = createStudent('Carl', 19);

console.log(john); // {age: 20, name: "John", sleep: ..., getAge: ..., getName: ...}
console.log(carl); // {age: 19, name: "Carl", sleep: ..., getAge: ..., getName: ...}
```

Анаогично можно поступить с лекторами:
```js
function createLecturer(name, age) {
    return {
        age: age,
        name: name,
        talk: function () { console.log('bla-bla-bla...') },
        getAge: function () { return this.age; },
        getName: function () { return this.name; }
    };
}

var iwan = createStudent('Iwan', 33);
console.log(iwan); // {age: 33, name: "Iwan", talk: ..., getAge: ..., getName: ...}
```

Проблемы в этом коде налицо, у нас на каждый объект студента и лектора создаются одни и те же функции. Реализацию функций `getAge` и `getName` мы вообще делаем в двух местах. Вот бы нам выделить три общих сущности:
- `Person` - какая-то персона, обладающая методами `getAge`, `getName`
- `Student` - какой-то студент, обладающий методом `sleep`
- `Lecturer` - какой-то лектор, обладающий методом `talk`

Попробуем решить эту задачу через уже знакомый нам механизм цепочек прототипов.

### Делегирование [⇡](8-constructors.md#Конструкторы)
Как я сказал ранее, классов в нашем распоряжении нет, а также нет и классического наследования. К механизму цепочки прототипов более близко понятие делегирования. Попробуем посмотреть под углом делегирования на следующий пример:

```js
var person = { type: 'none' };          // person --> Object.prototype --> null
var student = { age: 20 };              // student --> Object.prototype --> null
Object.setPrototypeOf(student, person); // student --> person --> Object.prototype --> null
```

Когда мы обращаемся к свойству `age`, объект `student` про него знает и возвращает нам его значение. Когда же мы хотим получить `type`, объект `student` про него ничего не знает и делегирует получение свойства объекту `person`.

Аналогичным образом можно рассмотреть и более длинные цепочки. Например, при вызове метода `hasOwnProperty`, объект `student` ничего не про него не знает и поэтому делегирует вызов объекту `person`, который в свою очередь тоже ничего не знает про этот метод, поэтому делегирует его выполнение объекту `Object.prototype`. Последний про него в курсе, поэтому выполняет его.

Когда же никто ничего не знает, нам возвращается `undefined`.

### Свойство `prototype` [⇡](8-constructors.md#Конструкторы)
Внимательный студент скорее всего уже заметил, что объект `Object.prototype` имеет схожее название с внутренним свйоством `[[Prototype]]`. И это не совпадение!

У каждой функции есть свойство `prototype`. Это свойство доступное и мы властны его изменять.

```js
function person() {}
console.log(person.prototype) //Object {}
```

Мы уже сталкивались с понятием функции-конструктора. Когда к функции применяется оператор `new`, JavaScript запускает эту функцию в режиме конструктора, что предполагает следующее. Перед выполнением функции создается пустой объект, а в его внутреннее свойство `[[Prototype]]` кладется ссылка на свойство `prototype` данной функции. Этот пустой объект становится контекстом исполнения данной функции (устанавливается в `this`). Результом выполнения является этот вновь созданный объект.

```js
function Person() {
    this.type = 'none';
}
var john = new Person(); // john --> Person.prototype --> Object.prototype --> null

console.log(john) //{type:"none"}
```

Если же из функции вернуть объект, то вернется именно он:
```js
function Person() {
    this.type = 'none';
    return { type: '---' };
}
var john = new Person(); // john --> Object.prototype --> null

console.log(john) // {type: "---"}
```
Как видно из примера мы потеряли из цепочки объект `Person.prototype`. В данном примере вызов с `new` и без него даст идентичный результат. Такое происходит только если возвращать объект. Если вернуть примитив, `undefined` или `null`, то вернется конктекст исполнения (`this`).

Важно понимать, что сами по себе функции-конструкторы от обычных функций не отличаются. Отличается то, как они спроектированы и к какому способу вызова приспособлены. Например, если вызвать функцию `Person` без `new`, то произойдет страшное. Конктекстом исполнения будет глобальный объект и именно ему присвоится свойство `type`. В строгом режиме будет выброшена ошибка `TypeError`.

Чтобы исключить такого рода ошибки, в мире JavaScript принято функции-конструкторы называть с большой буквы. Мимикрия под классы :)

```js
function Person() {         // Person --> Function.prototype --> Object.prototype --> null
    this.type = 'none';
}
var john = new Person();    // john --> Person.prototype --> Object.prototype --> null
```

Второе важное замечание касается того, что функция `Person` сама по себе также является объектом и обладает собственной цепочкой прототипов, которая никак не связана с цепочкой `john`. Тут как раз и видно разницу между `Person.[[Prototype]]` и `Person.prototype`.

Вернемся к нашему примеру
```js
function Person() {
    this.type = 'none';
}
var john = new Person(); // john --> Person.prototype --> Object.prototype --> null

console.log(john) //{type:"none"}
```
Из него можно сделать два важных вывода!

Во-первых, во время и после исполнения в наших руках объект, свойство `[[Prototype]]` которого указывает на `Person.prototype`.  А мы помним, что этим свойством мы можем управлять. Это значит, что мы можем что-то положить в `Person.prototype` и вновь созданным объектам эти свойства будут доступны.

```js
function Person() {}
Person.prototype.type = 'none';

var john = new Person();    // john --> Person.prototype --> Object.prototype --> null
console.log(john);      // {}
console.log(john.type); // "none"
```
Мы избавились от необходимости хранить в каждом объекте общую информацию!

Второй важный вывод: внутреннее свойство `[[Prototype]]` хранит ссылку на объект `Person.prototype`. А значит, если мы после создания объекта начнем править объект `Person.prototype`, нам эти изменения будут доступны:
```js
function Person() {}
Person.prototype.type = 'none';

var john = new Person();    // john --> Person.prototype --> Object.prototype --> null
console.log(john);      // {}
console.log(john.type); // "none"

Person.prototype.type = 'person';
console.log(john.type); // "person"
```

Тут скрывается одна опасность. Если мы после создания переопределим `Person.prototype`, то потеряем связь объекта с `Person.prototype`. Потому что после этого `Person.prototype` и свойство `[[Prototype]]` объекта будут ссылаться на разные объекты!

```js
function Person() {}        // Person --> Function.prototype --> Object.prototype --> null
Person.prototype.type = 'none';

var john = new Person();    // john --> Person.prototype --> Object.prototype --> null
console.log(john.type); // "none"

Person.prototype = { type: 'person' };
// john --> {объект, бывший Person.prototype} --> Object.prototype --> null
console.log(john.type);             // "none"
console.log(Person.prototype.type); // "person"
```
При получении свойства `type` мы переходим по цепочке прототипов в объект, на который раньше ссылался объект `Person.prototype`.

Лучше изменять свойство `prototype` до создания объектов и делать это прямо в месте объявления данной функции.

### Контекст исполнения в функции-конструкторе [⇡](8-constructors.md#Конструкторы)
Как мы выяснили ранее при вызове функции через оператор `new` создается новый объект и он же становится контекстом исполнения. Это может пригодиться для инициализации свойств вновь создаваемого объекта:
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}
var john = new Person('John', 20);
console.log(john); // {name: "John", age: 20}
```

Пример это почти то, что нам нужно, но не хватает методов!
```js
function Person(name, age) {
    this.name = name;
    this.age = age;

    this.increaseAge = function () {
        this.age++;
    }
}
var john = new Person('John', 20);
console.log(john); // {name: "John", age: 20}

john.increaseAge();
console.log(john); // {name: "John", age: 21}
```

Теперь у нас методы, но осталась одна проблема, мы создаем функции для каждого экземпляра персоны. Ну дак давайте вынесем эту функцию в `Person.prototype`:
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.increaseAge = function () {
    this.age++;
}

var john = new Person('John', 20);
console.log(john); // {name: "John", age: 20}

john.increaseAge();
console.log(john); // {name: "John", age: ??}
```
Хм! А оно заработает? Ведь в функции используется this. Нам снова нужно вспомнить, как выбирается контекст исполнения. Когда мы вызываем функцию на объекте, контекстом исполнения становится данный объект, а это и именно то, что мы ожидаем. Поэтому пример выше отработает правильно.

К этому моменту мы избавились от дублирования и научились создавать схожие по функциональности объекты!

### Свойство `constructor` [⇡](8-constructors.md#Конструкторы)
Вначале рассказа про свойство `prototype` я говорил, что это свойство ссылается на пустой объект. Я вас обманул, у этого "пустого" объекта есть неперчисляемое свойство `constructor`, которое ссылается на саму функцию:
```js
function Person() {};
console.log(Person.prototype.constructor === person);   //true

var john = new Person();
console.log(john.constructor === person);               //true (по цепочке)
```
Это может показаться удобным для того, чтобы узнать, с помощью какой функции создан данный объект. Но здесь таится опасность. Это свойство не является `read-only` и может быть легко перезаписано. Более того, весь прототип может быть перезаписан. Поэтому нет никакой гарантии, что `constructor` будет содержать правильную ссылку:
```js
function Person() {};
Person.prototype = {}
console.log(Person.prototype.constructor === Person);   //false
console.log(Person.prototype.constructor === Object);   // true
```
Теперь конструктор ссылается на `Object`. Это произошло из-за того, что у литерала объекта свойство `[[Prototype]]` ссылается на `Object.prototype`, а свойство `constructor` ссылается на `Object`.

## Механика наследования [⇡](8-constructors.md#Конструкторы)
Мы научились объявлять функции-конструкторы, через которые можно создавать схожие объекты. Если вернуться к задаче про студентов и лекторов, мы можем выделить три сущности:
```js
function Person() {}
function Student() {}
function Lecturer() {}
```

Но нам не хватает связи между сущностью `Person` и `Student`. Хотелось бы выделить общую функциональность, объединяющую `Student` и `Lecturer` в сущность `Person`, чтобы ее реализацию не дублировать.

На самом деле все знания для этого у нас уже есть, осталось только их применить.

### Первая попытка [⇡](8-constructors.md#Конструкторы)
Первое, что приходит в голову, это сделать как-то так:
```js
function Person() {}
Person.prototype.increaseAge = function () {
    this.age++;
}

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype = Person.prototype;

var john = new Student('John', 20);
console.log(john); // {name: "John", age: 20}

john.increaseAge();
console.log(john); // {name: "John", age: 21}
```

Вроде работает, но есть одна большая проблема: у нас `Student.prototype` и `Person.prototype` ссылаются на один объект. Это значит, что если мы начнем менять `Student.prototype`, например, чтобы добавить метод `sleep()`, он окажется доступным и для `Person.prototype`. Если сюда добавить лекторов, то все три сущности будут делить один прототип: лекторы начнут спать, а студенты что-то там читать.

### Вторая попытка [⇡](8-constructors.md#Конструкторы)
Ну хорошо. А что если нам сделать так:
```js
function Person() {}
Person.prototype.increaseAge = function () {
    this.age++;
}

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype = new Person();

var john = new Student('John', 20);
console.log(john); // {name: "John", age: 20}

john.increaseAge();
console.log(john); // {name: "John", age: 21}
```
Это уже лучше, за одним исключением. В функцию `Person` мы можем захотеть положить что-нибудь важное. Например, логирование, или вызов внутренних методов. Мы вовсе не хотим, чтобы все это исполнялось при присвоении прототипа.

### `Object.create` [⇡](8-constructors.md#Конструкторы)
В ES5 появился метод `Object.create`, который делает то, что нам нужно. А именно создает объект, в свойстве `[[Prototype]]` которого лежит объект, переданный первым аргументом.

```js
var person = {};                        // person --> Object.prototype --> null
var student = Object.create(person);    // student --> person --> Object.prototype --> null

console.log(Object.getPrototypeOf(student) === person);   // true
```

Метод принимает также вторым параметром дескриптор свойств для нового объекта. Дескриптор аналогичен тому, который принимает метод `Object.defineProperties`.

#### Полуполифил [⇡](8-constructors.md#Конструкторы)
В средах, где ES5 все еще не поддерживается, можно самим реализовать. Единственное ограничение, мы можем реализовать только функциональность, связанную с первым параметром. Полную реализацию `Object.defineProperties` на ES3 сделать невозможно.

```js
if (!Object.create) {
    Object.create = function(o) {
        function F() {}
        F.prototype = o;
        return new F();
    };
}
```
Эта реализация очень похожа на нашу вторую попытку,  но за одним исключением: мы берем заведомо пустую функцию. Поэтому мы можем не переживать, что в функции-конструкторе будет какая-то важная функциональность.

#### `Object.create(null)` [⇡](8-constructors.md#Конструкторы)
В рамках рассказа про `Object.create` хочу еще немного отвлечся от основной темы и рассказать про очень интересный прием. А именно создание по-настоящему пустого объекта с пустой цепочкой:
```js
var dict = Object.create(null);

console.log(dict.hasOwnProperty); // false
```

В примере `dict` — пустой объект, у которого нет цепочки прототипов, его внутреннее свойство `[[Prototype]]` указывает на `null`. Такого рода объекты, часто называют словарями, так как их удобно использовать для хранения данных, они лишены особенностей, связанных с делегированием свойств по цепочке прототипов. По ним можно безопасно итерироваться циклом `for..in`.

### Успешная попытка [⇡](8-constructors.md#Конструкторы)
Теперь мы можем воспользоваться функцией `Object.create` для создания прототипа.
```js
function Person() {}
Person.prototype.increaseAge = function () {
    this.age++;
}

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype = Object.create(Person.prototype); // Student.prototype --> Person.prototype
Student.prototype.constructor = Student; // восстанавливаем конструктор

var john = new Student('John', 20);
// john --> Student.prototype --> Person.prototype --> Object.prototype --> null
```
Нам пришлось восстановить `constructor`, потому что вновь созданный объект через `Object.create` ссылается на `Person.prototype`. При установке свойства `Student.prototype.constructor = Student` создается затеняющее свойство на объекте `Student.prototype`, поэтому `Person.prototype.constructor` все еще будет ссылаться на `Person`

Теперь мы можем в полной мере реализовать нашу первичную задачу:
```js
function Person() {}
Person.prototype.getName = function () { return this.name; }
Person.prototype.getAge = function () { return this.age; }

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype = Object.create(Person.prototype); // Student.prototype --> Person.prototype
Student.prototype.constructor = Student; // восстанавливаем конструктор
Student.prototype.sleep = function () {
    console.log('hrrrr...');
};
```

Аналогично с лектором:
```js
function Lecturer(name, age) {
    this.name = name;
    this.age = age;
}
Lecturer.prototype = Object.create(Person.prototype); // Student.prototype --> Person.prototype
Lecturer.prototype.constructor = Lecturer; // восстанавливаем конструктор
Lecturer.prototype.talk = function () {
    console.log('bla-bla-bla...');
};
```

Проверяем в бою:
```js
var john = new Student('John', 20);
var iwan = new Lecturer('Iwan', 33);

// john --> Student.prototype --> Person.prototype --> Object.prototype --> null
// iwan --> Lecturer.prototype --> Person.prototype --> Object.prototype --> null

console.log(john.getName()); // "John"
console.log(iwan.getName()); // "Iwan"
```

Подробную визуализацию можно посмотреть [тут](https://clck.ru/9bdcR).

### Реиспользуем конструкторы [⇡](8-constructors.md#Конструкторы)
В целом мы добились, чего хотели. Избавились от избыточности и повторяющейся реализации. Выделили абстракцию в виде сущности `Person` и конкретные сущности в виде `Student`, `Lecturer`.

На этом примере я хочу показать еще один прием. А именно, что можно сделать с одинаковыми частями кода в функции-конструкторе.

Во-первых, переопределим функцию `Person`:
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.getName = function () { return this.name; }
Person.prototype.getAge = function () { return this.age; }
```

А в конструкторе `Student` и `Lecturer` мы вызовем функцию Person с подмененным контекстом:
```js
function Student(name, age) {
    Person.call(this, name, age);
}
Student.prototype = Object.create(Person.prototype); // Student.prototype --> Person.prototype
Student.prototype.constructor = Student; // восстанавливаем конструктор
Student.prototype.sleep = function () {
    console.log('hrrrr...');
};
```
Что произошло? Мы из конструктора вызываем функцию `Person` (это всего-навсего обычная функция), но контекстом исполнения делаем `this` из `Student`. Соответственно, функция `Person` устанавливает переданные свойства.

## Переопределение методов из цепочки [⇡](8-constructors.md#Конструкторы)
Мы выделили общие сущности, построили их в цепочки, доопределили свойства более конкретным способом. Все свойства и методы, которые мы добавляли, отличались от методов и свойств, определенных в базовой сущности. Что же делать, если мы хотим доопределить метод из базовой сущности?

Проблема заключается в том, что созданные на объекте свойства затеняют собой свойства из цепочки прототипов. Поэтому получить метод, который мы затенили на текущем объекте, можно лишь через объект, лежащий у текущего в цепочке.

Первый способ заключается в том, что бы не создавать методы и свойства, затеняющие свойства из цепочки. "– Доктор, у меня болит, когда я делаю так. – Ну не делайте так!".

```js
function Person() {}
Person.prototype.getName = function () { return this.name; };

function Student(name) { this.name = name; }
Student.prototype = Object.create(Person.prototype);

Student.prototype.getStudentName = function () {
    return 'Student ' + this.getName();
};

var john = new Student('John');
console.log(john.getStudentName()); // "Student John"
```

Это хороший способ и часто подходит. Но есть и другой способ. Он заключается в вызове затеняемого свойства объекта из цепочки прототипов с текущим конктекстом исполнения:
```js
function Person() {}
Person.prototype.getName = function () { return this.name; };

function Student(name) { this.name = name; }
Student.prototype = Object.create(Person.prototype);

Student.prototype.getName = function () {
    return 'Student ' + Person.prototype.getName.call(this);
};

var john = new Student('John');
console.log(john.getName()); // "Student John"
```

## Инспектирование связей между объектами [⇡](8-constructors.md#Конструкторы)
На этом этапе мы научились создавать объекты через функции-конструкторы. Остался один вопрос: каким образом мы можем понять, через какую функцию-конструктор создан наш объект.

Допустим, у нас есть некая система, где нужно разделить права доступа для студентов и лекторов. В некоторые части этой системы есть доступ только у лекторов, поэтому есть функция, проверяющая студент перед ней или лектор:
```js
function checkPerson(person) {}

var john = new Student('John');
var iwan = new Lecturer('Iwan');

checkPerson(john); // should return "false"
checkPerson(iwan); // should return "true"
```
Как же нам это сделать?

### `instanceof` [⇡](8-constructors.md#Конструкторы)
В этом нам может помочь оператор `instanceof`. Левый операнд является проверяемым объектом, а правый функцией-конструктором (на самом деле любой функцией). Оператор проверяет есть ли в в цепочке прототипов объекта ссылка на свойство `prototype` функции:

```js
function Student() {}
var john = new Student();   // john --> Student.prototype --> Object.prototype --> null

console.log(john instanceof Student);   // true
console.log(john instanceof Object);    // true
```
В примере выше, используя оператор `instanceof`, мы задали два вопроса:
- "Есть ли в цепочке прототипов объекта `john` ссылка на объект `Student.prototype`?"
- "Есть ли в цепочке прототипов объекта `john` ссылка на объект `Object.prototype`?"

И получили положительный ответ на оба.

### `Object.prototype.isPrototypeOf()` [⇡](8-constructors.md#Конструкторы)
Второй способ — через метод `Object.prototype.isPrototypeOf`. Так как он лежит в объекте `Object.prototype`, он нам доступен через цепочку прототипов.

```js
function Student() {}
var john = new Student();   // john --> Student.prototype --> Object.prototype --> null

console.log(Student.prototype.isPrototypeOf(john));   // true
console.log(Object.prototype.isPrototypeOf(john));    // true
```
Данный метод решает аналогичную задачу, что и оператор `instanceof`, только его вопросы выглядят более явно. Концептуальная же разница заключается в том, что `instanceof` работает исключительно с функциями, так как проверяет их свойство `prototype`, а метод `isPrototypeOf` работает с объектами, что бывает предпочтительнее в некоторых случаях.

## Связывание объектов [⇡](8-constructors.md#Конструкторы)
Мы полностью разобрались в механизме цепочки прототипов. Увидели, что этот механизм связан с созданием связей между объектами и делегированием свойств.

Сейчас я хочу взглянуть на реализацию нашей задачи про студентов и лекторов только с точки зрения делегирования. Отбросить функции-конструкторы и мимикрию под классы. Все, что для этого необходимо, это метод `Object.create`.

Итак, опишем сущность персоны: объект, который содержит свойства и методы, общие и для студентов, и для лекторов.
```js
var person = {
    getName: function () { return this.name; },
    getAge: function () { return this.age; }
};
```

У нас получился объект с выделенной общей функциональностью, чистой функциональностью!

Теперь нам необходимо выделить сущности `student` и `leсturer` и сделать так, чтобы они делегировали методы `getName` и `getAge` объекту `person`:
```js
var student = Object.create(person);    // student --> person
student.sleep = function () { console.log('hrrrr...'); };

var leсturer = Object.create(person);   // leсturer --> person
leсturer.talk = function () { console.log('bla-bla-bla...'); };
```

Далее мы можем создавать конкретных представилей точно таким же способом:
```js
var john = Object.create(student);      // john --> student --> person
john.name = 'John';
john.age = 20;
```

Или даже сделать метод-хелпер для этих целей:
```js
var person = {
    create: function (name, age) {
        var person = Object.create(this);
        person.name = name;
        person.age = age;
        return person;
    },
    getName: function () { return this.name; },
    getAge: function () { return this.age; }
};

var student = Object.create(person);    // student --> person
student.sleep = function () { console.log('hrrrr...'); };

var john = student.create('John', 20);  // john --> student --> person

john.sleep();                   // hrrrr...
console.log(john.getName());    // "John"
```

Данное решение является чисто объектным. Одни объекты делегируют свойства и методы другим. Объекты, обладающие более общими свойствами, лежат выше в цепочке делегирования. Более конкретные – ниже.

Переопределение методов делается аналогично подходу с функциями-конструкторами:
```js
var person = {
    create: function (name) {
        var person = Object.create(this);
        person.name = name;
        return person;
    },
    getName: function () { return this.name; }
};

var student = Object.create(person);    // student --> person
student.getName = function () { return 'Student ' + person.call(this); };

var john = student.create('John');      // john --> student --> person

console.log(john.getName());    // "Student John"
```
Только под призмой делегирования это выглядит более явно и логично. В примере выше мы явно делегируем вызов объекту `person`. То есть делаем то же самое, что происходит при обычном прототипном делегировании, только руками.

При этом подходе для инспектирования объекта как раз подходит метод `isPrototypeOf` и совершенно не подходит оператор `instanceof`:
```js
var person = {
    create: function (name) {
        var person = Object.create(this);
        person.name = name;
        return person;
    },
    getName: function () { return this.name; }
};

var student = Object.create(person);    // student --> person

var john = student.create('John');      // john --> student --> person

console.log(student.isPrototypeOf(john));   // true
console.log(person.isPrototypeOf(john));    // true
console.log(person.isPrototypeOf(student)); // true
```

Оба подхода — и через функции-конструкторы, и через `Object.create` — хороши, каждый имеет свои плюсы и минусы. Иногда лучше подходит один, иногда другой, но в целом это дело вкуса и зависит от того, как удобнее программисту.
