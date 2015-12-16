# Асинхронное программирование

### Последовательное выполнение

Рассмотрим пример.

```js
function foo() {
    console.log('функция foo была вызвана');
}
function bar() {
    console.log('функция bar была вызвана')
}

foo();
bar();

// вывод:
// > функция foo была вызвана
// > функция bar была вызвана
```

Этот код называется синхронным, потому что он выполняется последовательно сверху вниз, команда за командой. В любой момент времени можно сказать, какая операция была выполнена до этого, и какая будет выполнена следующей.

Однако, не всегда код выполняется последовательно.

### Асинхронное выполнение

Простейший случай асинхронного кода – отложенный вызов функции по таймеру.

```js
function foo() {
    console.log('функция foo была вызвана');
}
function bar() {
    console.log('функция bar была вызвана')
}

setTimeout(foo, 1000);
bar();

// вывод:
// > функция bar была вызвана
// (пауза в одну секунду)
// > функция foo была вызвана
```

С помочью таймера мы указываем, что функция `foo` должна быть вызвана через 1000 миллисекунд, то есть через одну секунду. То есть, функция `foo` будет вызвана после функции `bar`.

В JavaScript есть ещё одна функция-таймер – `setInterval`.

```js
setTimeout(fn, ms) – запускает функцию один раз через n миллисекунд
setInterval(fn, ms) – запускает функцию через каждые n миллисекунд
```

Оба таймера имеют одинаковую сигнатуру: первым аргументом они принимают функцию, а вторым – интервал времени в миллисекундах. Отличаются они тем, что `setTimeout` вызывает функцию один раз, а `setInterval` – до тех пор, пока таймер не будет отменён.

### Блокирующие операции

В одной из домашних работ вы сталкивались с функцией чтения из файла `readFileSync`.

```js
var fs = require('fs');
var moment = require('./moment');
var robbery = require('./robbery');

var gang = fs.readFileSync('gang.json');

var moment = robbery.getAppropriateMoment(gang, 90, {from: '09:00+5', to: '21:00+5'});
var message = moment.format('%HH:%MM!');
```

Замерим, сколько времени занимает чтения из файла. Для этого воспользуемся функциями `console.time` и `console.timeEnd`.

```js
console.time('Файлы');
var gang = fs.readFileSync('gang.json');
console.timeEnd('Файлы');

console.time('Вычисления');
var moment = robbery.getAppropriateMoment(gang, 90, {from: '09:00+5', to: '21:00+5'});
var message = moment.format('%HH:%MM!');
console.timeEnd('Вычисления');

// вывод:
// > Файлы: 0.562ms
// > Вычисления: 2.759ms
```

На первый взгляд, на чтение файла уходит сравнительно немного времени. Попробуем прочитать файл большего размера.

```js
console.time('Файлы');
var gang = fs.readFileSync('gang.json');
var rules = fs.readFileSync('rules.pdf'); // 8 МБ
console.timeEnd('Файлы');

console.time('Вычисления');
var moment = robbery.getAppropriateMoment(gang, 90, {from: '09:00+5', to: '21:00+5'});
var message = moment.format('%HH:%MM!');
console.timeEnd('Вычисления');

// вывод:
// > Файлы: 5.666ms
// > Вычисления: 2.735ms
```

Теперь на чтение файлов уходит вдвое больше времени, чем на вычисления.

Посмотрим, что будет, если читать файл не с жёсткого диска, а напрямую с Github.

```js
console.time('Файлы');
var file = 'https://rawgit.com/.../gang.json';
var gang = getRequestSync('file');
console.timeEnd('Файлы');

console.time('Вычисления');
var moment = robbery.getAppropriateMoment(gang, 90, {from: '09:00+5', to: '21:00+5'});
var message = moment.format('%HH:%MM!');
console.timeEnd('Вычисления');

// вывод:
// > Файлы: 167.050ms
// > Вычисления: 2.702ms
```

Как видите, разница времени выполнения уже достигает двух порядков.

Часто говорят, что самая медленная часть программы – это IO (англ. input-output), то есть ввод-вывод: чтение файлов, работа с базой данных, http-запросы.

Проблема в том, что Javascript выполняется в одном потоке. Это значит, что в один момент времени может выполнятся только одна операция.

```js
setInterval(function () {
	console.log('Я в порядке!');
}, 1000);

setTimeout(function () {
	console.log('Приступаем к чтению!');
	fs.readFileSync('huge.txt');
	console.log('Файл прочитан!');
}, 5000)

// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Приступаем к чтению!'
// ...
// ...
// ...
// ...
// 'Файл прочитан!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
```

В этом примере программа выводит на экран сообщение раз в секунду, а через пять секунд запускается функция чтения большого файла.

Если мы запустим этот пример (гифка?), то увидим, что сообщение будет выводится до тех пор, пока операция файла не заблокирует выполнение.

В браузере этот эффект выглядит ещё более неприглядно: на время выполнение медленной команды будет полностью заблокирован пользовательский интерфейс: не будут работать нажатия на кнопки, прокрутка и, конечно, любой другой код.

### Асинхронные функции

Решением проблемы блокировок является использование асинхронных функций.

На самом деле, нам не нужно безотрывно ждать, пока файл не прочитается. Достаточно сделать запрос на чтение и сообщить, что мы хотим сделать, когда файл будет прочитан.

Выглядит это следующим образом.

```js
fs.readFile('huge.txt', function (result) {
	// содержимое файла лежит в переменной result
});

```

Обратите внимание на отсутствие `Sync` в названии метода.

Как видите, асинхронные не возвращают значение напрямую. Вместо этого они принимают последним аргументом функцию, которая будет вызвана при завершении операции. Такую функцию называют callback.

Перепишем пример с таймером, используя асинхронную функцию.

```js
setInterval(function () {
	console.log('Я в порядке!');
}, 1000);

setTimeout(function () {
	console.log('Приступаем к чтению!');
	fs.readFileSync('huge.txt', function () {
		console.log('Файл прочитан!');
	});
}, 5000)

// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Приступаем к чтению!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Файл прочитан!'
// 'Я в порядке!'
// 'Я в порядке!'
// 'Я в порядке!'
```

Как видите, теперь выполнение программы не прерывается.

### Среда выполнения

Итак, мы рассмотрели два сценария работы с асинхронным кодом. Во-первых, мы использовали таймеры для отложенного выполнения функций. Во-вторых, мы использовали асинхронные функции для работы с IO.

В начале лекции я говорил, что Javascript может выполнять только одну операцию в один момент времени. Сейчас я расскажу о механизме, который позволяет коду выполнятся асинхронно.

```js
node index.js
```

Когда мы запускаем программу, среда выполнения выделяет ей какую-то область памяти и стек вызовов.

Стек вызовов – это структура данных, которая позволяет нам отслеживать, в каком месте программы мы находимся в данный момент. Работает она примерно так.

```js
function a() {
    b();
}

function b() {
    c();
}

function c() {

}

a();
```

Изначально, при запуске программы, стек пуст. Когда мы вызываем функцию, она добавляется на вершину стека. При возврате из функции, она, наоборот, снимается с вершины стека. Когда стек пуст, программа завершается.

Вы наверняка сталкивались с стеком вызовов, когда читали сообщения об ошибках, отображаемых node.js. Попробуем вызывать ошибку внутри третьей функции.


```js
function a() {
    b();
}

function b() {
    c();
}

function c() {
    throw new Error();
}

a();
```

Если мы запустим этот код, то увидим примерно следующее: TODO стек трейс.

А что будет, если мы исправим функцию таким образом, чтобы она вызывала сама себя?

```js
function a() {
    а();
}

a();
```

Функция будет добавляться в стек до тех пор, пока не закончится отведённое ему место. Тогда программа завершится с ошибкой TODO текст ошибки

Попробуем сделать наш код асинхронным и добавим него таймер.

```js
function a() {
    b();
}

function b() {
    c();
}

function c() {

}

setTimeout(a, 5000);
```

При вызове таймера он кладётся в стек, затем извлекается из него. Некоторое время стек пуст, но через пять секунд в нём появляется функция `a`.

Каким-то образом среда выполнения запомнила, что функцию надо выполнить через пять секунд, и в назначенное время добавила её в стек.

Для хранения задач используется отдельная структура данных – event queue.

За таймерами и запросами к IO следит среда выполнения.

Когда приходит ответ на запрос или срабатывает таймер, задача кладётся в очередь.

Когда все операции извлекаются из стека, в него кладётся задача из очереди.

Когда очищается и стек, и очередь, программа завершается.

### Источники

- [WHATWG Specification](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops)
- [Concurrency model and Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
- [Delayed Execution](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Threads)
- [We have a problem with promises](http://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)
- [Exploring JS: Asynchronous programming (background)](http://exploringjs.com/es6/ch_async.html)
- [ES6 Generators in depth](http://exploringjs.com/es6/ch_async.html)
- [ES7 async functions](https://jakearchibald.com/2014/es7-async-functions/)
- [Async Functions for ECMAScript](https://github.com/tc39/ecmascript-asyncawait)
- [Using Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)
- [Node.js: managing child processes](http://krasimirtsonev.com/blog/article/Nodejs-managing-child-processes-starting-stopping-exec-spawn)
- [Node.js External and Child Processes](http://www.graemeboy.com/node-child-processes)
- [No promises: asynchronous JavaScript with only generators](http://www.2ality.com/2015/03/no-promises.html)
- [Asynchronous programming and continuation-passing style in JavaScript](http://www.2ality.com/2012/06/continuation-passing-style.html)
- [The JavaScript Event Loop: Explained](http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/)
- [JS Garden: setTimeout and setInterval](http://bonsaiden.github.io/JavaScript-Garden/#other.timeouts)
- [Promise – это не больно](https://speakerdeck.com/azproduction/promise-eto-nie-bol-no)
- [КРиПИ - JavaScript Асинхронность, таймеры, работа с сервером](https://speakerdeck.com/azproduction/kripi-javascript-asinkhronnost-taimiery-rabota-s-siervierom)
- [Synchronous and asynchronous sequential execution of functions](http://www.2ality.com/2015/11/sequential-execution.html)
- [Philip Roberts: What the heck is the event loop anyway?](http://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)
- [JavaScript. Асинхронность - Михаил Давыдов](http://www.youtube.com/watch?v=asP5gAsZ9gc)
- [Сергей Жигалов, Яндекс | Асинхронность в JavaScript | ChellyJS1](http://www.youtube.com/watch?v=UDRGnFEefIc)
