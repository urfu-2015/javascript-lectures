# Прототипы

- [Введение](7-prototypes.md#%D0%92%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-)
- [Получение свойства объекта](7-prototypes.md#%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0-)
  - [`[[Prototype]]`](#Прототипы-)
    - [`__proto__`](#__proto__-)
    - [`Object.getPrototypeOf`](#objectgetprototypeof-)
    - [`Object.setPrototypeOf`](#objectsetprototypeof-)
  - [Получение свойства из цепочки](7-prototypes.md#%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%B7-%D1%86%D0%B5%D0%BF%D0%BE%D1%87%D0%BA%D0%B8-)
  - [`Object.prototype`](#objectprototype-)
  - [Итерация по объекту, проверка свойства, enumerable](7-prototypes.md#%D0%98%D1%82%D0%B5%D1%80%D0%B0%D1%86%D0%B8%D1%8F-%D0%BF%D0%BE-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D1%83-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-enumerable-)
    - [Оператор `in`](7-prototypes.md#%D0%9E%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80-in-)
    - [`Object.prototype.hasOwnProperty()`](#objectprototypehasownproperty-)
    - [Цикл `for..in`](7-prototypes.md#%D0%A6%D0%B8%D0%BA%D0%BB-forin-)
    - [Итерация по собственным ключам](7-prototypes.md#%D0%98%D1%82%D0%B5%D1%80%D0%B0%D1%86%D0%B8%D1%8F-%D0%BF%D0%BE-%D1%81%D0%BE%D0%B1%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D0%BC-%D0%BA%D0%BB%D1%8E%D1%87%D0%B0%D0%BC-)
- [Установка свойства объекта, затенение свойств](7-prototypes.md#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0-%D0%B7%D0%B0%D1%82%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2-)
  - [Затенение свойств](7-prototypes.md#%D0%97%D0%B0%D1%82%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2-)
  - [Установка нового значения для свойства из цепочки](7-prototypes.md#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-%D0%BD%D0%BE%D0%B2%D0%BE%D0%B3%D0%BE-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B4%D0%BB%D1%8F-%D1%81%D0%B2%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0-%D0%B8%D0%B7-%D1%86%D0%B5%D0%BF%D0%BE%D1%87%D0%BA%D0%B8-)

## Введение [⇡](7-prototypes.md#Прототипы)
В этой лекции мы познакомимся с таким понятием как цепочка прототипов. Это мощный инструмент, используемый в JavaScript и позволяющий строить сложные программы.

Рассмотрим пример. Допустим, у нас есть необходимость хранить сущности студентов и лекторов.

Мы выделим сущность студента:
```js
var student = {
    age: 20,
    name: 'John',
    sleep: function () {},
    getAge: function () {},
    getName: function () {}
}
```

А также сущность лектора:
```js
var lecturer = {
    age: 33,
    name: 'Iwan',
    talk: function () {},
    getAge: function () {},
    getName: function () {}
}
```

У этих сущностей очень много общего, например, свойства `getAge`, `getName`. Но также есть и специфичные: `sleep` у студента и `talk` у лектора.

Обычно для выделения таких абстракций используются классы и их наследование друг от друга. Это отлично, за исключением одного маленького но. В JavaScript нет такого понятия как классы. В нашем распоряжении только объекты и функции, которые тоже объекты.

Решить же эту задачу нам позволяет механизм цепочки прототипов. Цепочка прототипов позволяет делегировать свойства одного объекта другому. Мы выделяем абстракции, а потом делегируем свойства от частного объекта более общему.

Лекция будет состоять из двух основных блоков. Вначале мы познакомимся с тем, где нам могут встретиться цепочки прототипов и как работает этот механизм. Во второй части мы научимся строить эти цепочки, а также выделять абстракции, вынося в них общий повторяющийся код и общую функциональность.


## Получение свойства объекта [⇡](7-prototypes.md#Прототипы)
Впервые, где мы сталкиваемся с работой прототипов – это при обращении к свойствам или методам объекта.
Рассмотрим простой пример:
```js
var student = { name: 'John' };
console.log(student.name);
```
Здесь мы хотим получить имя пользователя.

Когда мы обращаемся к свойству объекта, вызывается внутренний метод `[[Get]]`, отвечающий за поиск и возврат значения.

В простом случае метод находит свойство на объекте и возвращает значение данного свойства. Например, в в нашем примере у объекта `student` есть свойство `name`, поэтому вернется его значение и результат будет `John`.

Если же свойства нет, поиск на этом не останавливается. Тут-то на сцену и выходит цепочка прототипов.

### `[[Prototype]]` [⇡](7-prototypes.md#Прототипы)
У каждого объекта в JavaScript есть внутренее свойство `[[Prototype]]`, которое обычно указывает на другой объект или равно `null`. Это скрытое свойство и прямого доступа к нему нет.
```js
var student = { age: 20 };
// student.[[Prototype]] -> Object.prototype
```
Как мы видим по умолчанию при создании объекта свойство `[[Prototype]]` ссылается на объект `Object.prototype`, которого в свою очередь тоже есть скрытое свойство `[[Prototype]]`, равное `null`.
```js
var student = { age: 20 };
// student.[[Prototype]] -> Object.prototype
// Object.prototype.[[Prototype]] -> null
```

Чтобы было проще я упрощю немного обозначение. Если свойство `[[Prototype]]` объекта ссылается на другой объект, то я буду это показывать стрелкой с двумя дефисами:
```js
var student = { age: 20 }; // student --> Object.prototype --> null
```

Есть несколько способов получить доступ к этому объекту.
#### `__proto__` [⇡](7-prototypes.md#Прототипы)
Ранее в спецификации ES3 не было описано прямого способа достучаться до этого свойства, поэтому некоторые браузеры изобрели свойство не документированное свойство `__proto__`. Пользоваться им не рекомендуется, так как это свойство не поддерживается спецификацией и может отсутствовать в какой-нибудь среде.
Тем не менее, для отладки в консоли оно вполне подойдет.
```js
var student = { age: 20 }; // student --> Object.prototype --> null

console.log(student.__proto__ === Object.prototype);   // true
console.log(Object.prototype.__proto__);            // null
console.log(student.__proto__.__proto__);              // null
```

Этот метод также является сеттером:
```js
var person = { type: 'none' };  // person --> Object.prototype --> null
var student = { age: 20 };      // student --> Object.prototype --> null

student.__proto__ = person;     // student --> person --> Object.prototype --> null

console.log(student.__proto__ === person);   // true
```

#### `Object.getPrototypeOf` [⇡](7-prototypes.md#Прототипы)
В спецификации ES5 появился метод `Object.getPrototypeOf` для получения прототипа объекта:
```js
var student = { age: 20 }; // student --> Object.prototype --> null

console.log(Object.getPrototypeOf(student) === Object.prototype);   // true
console.log(Object.getPrototypeOf(Object.prototype));               // null
```

#### `Object.setPrototypeOf` [⇡](7-prototypes.md#Прототипы)
А вот сеттер прототипа для объекта появился только в ES6, поэтому доступен не везде и пользоваться им не рекомендуется.
Однако, пока мы не познакомились с другими и более распротраненными способами управлять цепочкой прототипов, я буду использовать данный метод в примерах.
```js
var person = { type: 'none' };  // person --> Object.prototype --> null
var student = { age: 20 };      // student --> Object.prototype --> null

Object.setPrototypeOf(student, person);  // student --> person --> Object.prototype --> null

console.log(Object.getPrototypeOf(student) === person);   // true
```

### Получение свойства из цепочки [⇡](7-prototypes.md#Прототипы)
И так, вернемся к методу `[[Get]]` и получению свойства объекта. Простой случай, когда свойство существует у объекта мы уже рассмотрели. Теперь обратим внимание на случай, когда свойства нет.

Как я уже сказал, поиск не останавливается. И наверно, раз уж мы столько времени уделили свойству `[[Prototype]]` как-то связан с ним.

Так и есть. Когда метод `[[Get]]` не находит свойтсва на объекте, то переходит к объекту, который ссылается свойство `[[Prototype]]` и повторяет поиск. Проверяется есть ли свойство на объекте (уже другом!), если есть – возвращается его значение, если нет – происходит переход к новому объекту через свойство `[[Prototype]]`.

Процесс этот завершается либо когда свойство находится на каком-нибудь объекте, либо когда очередное свойство `[[Prototype]]` указывает на `null`. Если свойство не находится, то есть доходим до конца, метод `[[Get]]` возвращает `undefined`

Такая цепочка из ссылок `[[Prototype]]` называется цепочкой прототипов.

Примеры:
```js
var person = { age: 0, type: 'none' };  // person --> Object.prototype --> null
var student = { age: 20 };              // student --> Object.prototype --> null

Object.setPrototypeOf(student, person); // student --> person --> Object.prototype --> null

console.log(student.age);  // 20           (свойство найдено на самом объекте student)
console.log(student.type); // "none"       (свойство через цепочку объекте person)
console.log(student.some); // undefined    (свойство не найдено ни на объекте, ни в цепочке)
```

### `Object.prototype` [⇡](7-prototypes.md#Прототипы)
Верхняя граница цепочки прототипов обычно объект `Object.prototype`. Свойство `[[Prototype]]` этого объекта указывает на `null`.

Объект `Object.prototype` имеет множество полезных методов: `.toString()`, `.valueOf()`, `.hasOwnProperty(..)`, `.isPrototypeOf(..)`. Если цепочка прототипов объекта содержит `Object.prototype` (а в большинстве случаев это именно так), то все эти методы доступны для объекта.

```js
var student = { age: 20 }; // student --> Object.prototype --> null
var students = [];         // students --> Array.prototype --> Object.prototype --> null

console.log(student.hasOwnProperty('age'));        // true
console.log(students.hasOwnProperty('length'));    // true
```

В этом примере ни у объекта `student` ни у `students` нет метода `hasOwnProperty`. Но они им доступны за счет того, что у них в цепочке есть объект `Object.prototype`. Это очень удобно!

### Итерация по объекту, проверка свойства, enumerable [⇡](7-prototypes.md#Прототипы)
С правилами получением свойств объекта связаны такие операции как проверка свойства через оператор `in` или итерация по ключаем объекта через цикл `for..in`

#### Оператор `in` [⇡](7-prototypes.md#Прототипы)
Для проверки наличия свойства у объекта используется оператор `in`. Данный оператор проверяет наличие свойства не только на самом объекте, но и в его цепочке прототипов.

```js
var person = { type: 'none' };          // person --> Object.prototype --> null
var student = { age: 20 };              // student --> Object.prototype --> null
Object.setPrototypeOf(student, person); // student --> person --> Object.prototype --> null

console.log('age' in student);             // true (из student)
console.log('type' in student);            // true (из person)
console.log('hasOwnProperty' in student);  // true (из Object.prototype)
```

#### `Object.prototype.hasOwnProperty()` [⇡](7-prototypes.md#Прототипы)
Как же проверить наличие свойства именно на самом объекте? Для этого служит функция `Object.prototype.hasOwnProperty()`. Она доступна объектам через цепочку прототипов.

```js
var person = { type: 'none' };          // person --> Object.prototype --> null
var student = { age: 20 };              // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);    // student --> person --> Object.prototype --> null

console.log(student.hasOwnProperty('age'));            // true
console.log(student.hasOwnProperty('type'));           // false
console.log(student.hasOwnProperty('hasOwnProperty')); // false
```

#### Цикл `for..in` [⇡](7-prototypes.md#Прототипы)
Чтобы получить все свойства и методы объекта используется `for..in`.
```js
var person = { type: 'none' };              // person --> Object.prototype --> null
var student = { name: 'John', age: 20 };    // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);     // student --> person --> Object.prototype --> null

for(var key in student) {
    console.log(key);   // name, age, type
}
```

Как мы видим из примера, данный цикл также проходится по все цепочке прототипов. Но почему же в выводе нет свойств из объекта `Object.prototype`? Ответ прост. Да, цикл `for..in` проходится по всей цепочке, но итерируется только по перечисляемым свойствам. Помните из лекции про объекты? Это те свойства, которые обладают свойством `enumerable`.

```js
var student = { name: 'John', age: 20 };     // student --> Object.prototype --> null

// Объявляем неперечисляемое свойство
Object.defineProperty(student, 'type', {
  enumerable: false,
  value: 'admin'
});

for(var key in student) {
    console.log(key);   // name, age
}
```
Свойство `type` не вывелось, так как оно неперечисляемое.

#### Итерация по собственным ключам [⇡](7-prototypes.md#Прототипы)
Если смиксовать цилк `for..in` и метод `Object.prototype.hasOwnProperty()`, то можно добиться итерации только по собственным свойствам объекта:

```js
var person = { type: 'none' };              // person --> Object.prototype --> null
var student = { name: 'John', age: 20 };    // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);     // student --> person --> Object.prototype --> null

for(var key in student) {
    if (student.hasOwnProperty(key)) {
        console.log(key);   // name, age
    }
}
```

Для облегчения этой задачи в ES5 появилися метод `Object.keys`:
```js
var person = { type: 'none' };              // person --> Object.prototype --> null
var student = { name: 'John', age: 20 };    // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);     // student --> person --> Object.prototype --> null

console.log(Object.keys(student))  // ["name", "age"]

Object.keys(student).forEach(function (key) {
    console.log(key);   // name, age
});
```

## Установка свойства объекта, затенение свойств [⇡](7-prototypes.md#Прототипы)
Как работает механизм получения свойства мы разобрались. Теперь разберемся с тем как происходит установка значения. Присваиванием свойства занимается внутренний метод `[[PUT]]`.

```js
var student = { age: 20 };
student.age = 21;

console.log(use); // {age: 21}
```

В простом случае метод `[[PUT]]` находит свойство на объекте и устанавливает новое значение. Конечно, если это свойство не является `read-only` (`writable: false`)

```js
var student = { name: 'John', age: 20 };     // person --> Object.prototype --> null

// Объявляем свойство read-only
Object.defineProperty(student, 'type', {
  writable: false,
  value: 'none'
});

student.type = 'admin';
console.log(student);  // {name: "John", age: 20, type: "none"}
```

Как видим в примере, свойство `type` не изменилось.

В строгом же режиме при присваивании значения `read-only` свойству выбросится исключение:
`TypeError: Cannot assign to read only property 'type' of #<Object>`

Если же свойства нет ни у объекта ни в цепочке прототипов, то это свойство просто создается на объекте:
```js
var student = { age: 20 };     // student --> Object.prototype --> null

student.name = 'John';

console.log(student);  // {age: 20, name: "John"}
```

### Затенение свойств [⇡](7-prototypes.md#Прототипы)
Мы уже встречались с термином затенения. Сейчас познакомимся с ним в контексте цепочек прототипов. Рассмотрим пример, когда свйоство есть одновременно и у объекта и в цепочке прототипов.

```js
var person = { age: 0, type: 'none' };    // person --> Object.prototype --> null
var student = { age: 20 };                 // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);      // student --> person --> Object.prototype --> null

console.log(student.age);  // 20
```

В примере выше результатом будет `20`. Так произошло, так как метод `[[GET]]` возвращает первое найденое свойство. Получается, что свойство `age` объекта `student` собой заслоняет такое же свойство объекта `person` из цепочки прототипов. Это явление обычно называется затенением. Говорят, что свойство `student.age` затеняет свойство `person.age`.

### Установка нового значения для свойства из цепочки [⇡](7-prototypes.md#Прототипы)
Вернемся к установке свойств. Мы рассмотрели два простых вариант, а именно, когда свойства не существует совсем и когда оно есть на объекте. Теперь рассмотрим более сложный случай, когда свойство есть в цепочке прототипов. возникает три возможных сценария.

**Свойство имеет стандартный сеттер и не помечено как `read-only`.**
В этом случае создаться затеняющее свойство на объекте.

```js
var person = { age: 0, type: 'none' };  // person --> Object.prototype --> null
var student = {};                       // student --> Object.prototype --> null
Object.setPrototypeOf(student, person); // student --> person --> Object.prototype --> null

console.log(student.age);  // 0    (свойство объекта person, полученное по цепочке)

student.age = 20;
console.log(student);      // 20                       (свойство объекта student)
console.log(person);       // {age: 0, type: "none"}   (объект person не изменился)
console.log(student);      // {age: 20}                (у student создалось затеняющее свойство)
```

**Свойство помечено как `read-only`**
Поведение будет аналогичным, которое мы рассматривали `read-only` свойство на объекте. В строгом режиме выбросится исключение, а в нестрогом просто ничего не произойдет.

```js
var person = { type: 'none' };          // person --> Object.prototype --> null
var student = {};                       // student --> Object.prototype --> null
Object.setPrototypeOf(student, person); // student --> person --> Object.prototype --> null

// Объявляем свойство read-only
Object.defineProperty(person, '__name__', {
  writable: false,
  value: 'person'
});

student.__name__ = 'non-person';   // В cтрогом режиме исключение TypeError
console.log(student);              // {}
console.log(student.__name__);     // "person"
```

В примере мы попытались изменить свойство `__name__`. Но у нас ничего не вышло: на объекте никаких свойств не появилось и объект в прототипе не изменился.

**Свойство имеет кастомный сеттер**
В этом случае вызовется кастомный сеттер, а свойства на объекте не создатся.

```js
var person = { type: 'none' };        // person --> Object.prototype --> null
var student = {};                      // student --> Object.prototype --> null
Object.setPrototypeOf(student, person);  // student --> person --> Object.prototype --> null

// Объявляем кастомный сеттер
(function () {
    var NAME = 'person';

    Object.defineProperty(person, '__name__', {
        get: function() { return NAME; },
        set: function(newName) { NAME = newName }
    });
}());

student.__name__ = 'student';

console.log(student);           // {}
console.log(person.__name__);   // {}
```

В примере выше мы попытались установить новое значение в свойство `__name__`. Вызвался наш кастомный сеттер, который исправил свойство `__name__` у объекта `person`. Свойства же на объекте `student` не создалось.
