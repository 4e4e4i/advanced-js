
# Advanced js

### Что такое 'use strict' и для чего он нужен

Что делает 'use strict'? Это частый вопрос, который мы можем встретить на собеседовании, неплохое начало для разговора.

Это простой вопрос, обычно ожидают, что кандидат на собеседовании знает ответ на него. Но даже если это простой вопрос
вы все еще можете произвести впечатление на интервьюера, показав глубокое понимание значения этого ключевого слова.

Так что же такое strict mode?

Strict mode позволяет вам разместить программу или функцию в так называемый строгий операционный контекст. 
И я собираюсь объяснить что это значит с несколькими примерами, но если в двухсловах, он позволяет легче дебажить код.
Ошибки кода, которые бы в противном случае были проигнорированы или втихую сфейлелись, теперь будут генерировать
ошибки или выбрасывать исключения. Это позволяет вам раньше обнаружить ошибки в коде и быстро направляет вас к источнику
проблемы.

Так первая вещь о которой вы должны сказать - это как включить его. Как включить strict mode в JS файле?

Один из путей, это просто напечатать строку 'use strict' и поместить ее в самом вверху файла. Если вас попросят на интервью
записать его, помните никогда не пишите как use strict. Это не специальное ключевое словое use и не специальное ключевое
слово strict. Это строка. И вы можете спросить, почему это строка? Это выглядит довольно странно. И ответ по-настоящему
очевидно прост. Когда эта функция была впервые разработана и реализована, старые браузеры не поддерживали ее, 
только новые браузеры поддерживали ее. 
Если бы решили использовать ключевое слово «use» и ключевое слово «strict» при загрузке этого файла в более 
старом браузере, браузер, который бы не знал об «use strict», выдал бы ошибку. И поэтому то что они сделали, они просто
положили его в строку и так старые браузеры читали этот файл, говорили "воу да это же строка в вверху файла, они ничего не делает
и поэтому я просто проигнорирую ее". А в новых браузерах, если он увидит строку 'use strict' они поймут, что необходимо
переключится в strict mode operating context.

Это один из путей, в котором можно сказать JS, чтобы он смотрел на код в strict mode. И используя данный подход,
strict mode будет применяться ко всему коду по всему файлу, но есть и другой способ.

```javascript
// Not strict mode... 
function newCode() {
  "use strict"
  // Strict mode...
}
// Not Strict mode...
```
Если вы возможно когда-либо работали с легаси JS кодом, очень-очень старым JS кодом. Я серьезно сомневаюсь, что вы
на самом деле окажетесь в данной ситуации, потому что 'use strict' существует уже очень давно. Но если у вас есть
полное знание, это действительно возможно применить 'strict mode' только к части кода, с помощью добавления 'use strict' 
в верхнюю часть блока функции, затем все что будет под данной строкой, будет читаться в strict mode, а все что снаружи блока
функции будет в non-strict mode. 

Я показал, как включать его. А какого назначение 'use strict' мода? Что он на самом деле делает?

Одна из первых вещей это, что в 'use strict' моде использование переменной перед тем как она была определена вызовет ошибку.

Например, без strict mode назначение значения недекларированной переменной возможно и будет автоматически создана глобальная
переменная с таким именем. В JS всегда есть глобальный объект и в нем находятся все глобальные переменные и 
глобальные функции. В браузере глобальный объект - 'window'. Но если вы используете его внутри node, глобальный объект
будет называться 'global'.

Поэтому с помощью декларирования asim = 1; мы создаем глобальную переменую, глобальное свойство на глобальной переменной
window, с названием asim и мы назначаем ей значение 1.

```javascript
asim = 1;
console.log(window.asim); // 1
```

Так мы разобрались как это работет...Так почему это плохая вещь? Почему это раздражает? Позвольте мне показать специфичный
пример, когда это может быть невероятно разочаровывающим. Давайте представим, что в моем приложением, я декларировал переменную
под названием theVal. Я декларировал ее должным образом, используя theVal в самом начале моего файла и назначил ей
значение 0. И затем, где-то в приложение я решил изменить значение моей theVal переменной в 1, но я сделал глупую ошибку -
вместо того, чтобы напечать theVal, я написал thVal. И наконец я использую переменную theVal в 'if' операторе, думая будто
я установил значение в 1 и, ожидая что выведет Hello. Но, к сожалению, Hello не будет выведено в консоле, потому что была совершена ошибка
в названии переменной.

```javascript
var theVal = 0;
//
thVal = 1;
//
if (theVal > 0) {
    console.log("Hello")
}
```
  
А теперь если мы используем 'use strict'. Вызов недекларированной переменной вызывает ошибку.

```javascript
"use strict";

var theVal = 0;
//
thVal = 1; // вызывает ошибку: thVal is not defined
//
if (theVal > 0) {
    console.log("Hello")
}
```

Другая фича "use strict" - то что она останавливает вас от использования слов, которые уже зарезервированны для
будущих версий JS. Давайте например создадим переменную let и назначим ей значение - 1. Затем перезагрузим страницу и все
будет впорядке. 

```javascript
var let = 1;
```

Но если я добавлю "use strict" на вверх файла, вы увидете ошибку вызванную данной переменной

```javascript
"use strict"

var let = 1; // Вызовет ошибку: Unexpected strict mode reserved word
```
Причина данной ошибки, что слово let уже зарезервированно для будущих версий JS. По факту уже используется
в ECMA script 6 версии JS

Еще одна фича 'use strict' - вы не можете удалять функции, переменные или аргументы функций.

Давайте посмотрим на примере без использования use strict. Если вы всего-лишь напечатаете var foo = 0 и затем
попытаетесь удалить foo, перезагрузите страницу, не возникнет никакой ошибки...Тоже самое с функциями

```javascript
var foo = 1;
delete foo;

function moo() {}
delete moo;
```

А если вы будете использовать 'use strict' mode, то при удалении будет вызвана ошибка

```javascript
"use strict"

var foo = 1;
delete foo; // Delete of an unqualified identifier in strict mode

function moo() {}
delete moo; // Delete of an unqualified identifier in strict mode
```

И следующая вещь, о которой я хотел бы рассказать - это ключевое слово eval и use strict mode.
Strict mode делает eval немного безопаснее.

Например в non-use strict mode вы можете делать вещи, такие как var eval = 1; или именовать так функции, и никакой ошибки
не будет вызвано.

А в use strict mode будет вызвана ошибка

```javascript
"use strict"

var eval = 1; // Unexpected eval or arguments in strict mode
```
Если вы знаете eval, он позволяет вам оценивать выражения JavaScript, просто передавая строку. Таким образом,
он позволяет вам выполнять только произвольные биты типа кода JavaScript в 'var a = 1'.

```javascript
eval('var a = 1');
console.log(a); // 1
```
Этот код выполняет 'var a = 1', но в non-use strict mode переменная A вытекает из выражений eval, поэтому, если
вызвать console.log (a); вы можете видеть, что он выводит значение переменной A, которая фактически создается или 
определяется в блоке eval. Таким образом, вы можете представить себе ситуации, в которых это может вызвать проблемы с 
безопасностью или просто загрязнить ваше собственное пространство имен.

Но если мы используем use strict mode, любые переменные, что вы определите внутри eval существуют только внутри eval и
не вытекают наружу.

```javascript
'use strict'

var a = 2;
eval('var a = 1');
console.log(a); // 2
```

### Javascript передает переменные по ссылке или по значению?

Вопрос заключается в том, что когда вы передаете перменную A в функцию foo как параметр, вы передаете по значению переменной
или по ссылке? 

Быстрый ответ на этот вопрос заключается в том, что передаваемые примитивные типы, такие как: строки, числа, булин переменные,
передаются по значению, а объекты передаются по ссылке. И прекрасно, это по правде правильный ответ, но если вы дали
мне такой ответ мгновенно, я бы задал сразу следующий вопрос, который был бы что-то вроде "Объясни мне что означает pass-by value
и pass-by reference". Потому что я хочу знать что ты не только просто запомнил этот ответ, но и глубоко понимаешь, как это
происходит.

#### Позвольте объяснить для начала что значит pass-by value.

Так что же означает то, что если вы изменяете значение примитивного типа внутри функции, изменения не будут влиять на переменную
во внешней области видимости.

```javascript
'use strict'

var a;
function foo(a) {
    
}
```

В данном примере переменная A находятся во внешней области видимости, а внутри фукнции находится область видимости функции
или внутренняя область видимости. Так если var a = 1; 1 - примитивный тип и я сделаю console.log(a) во внешней области видимости
и также я вызову функцию foo с переменной a, внутри которой я измению переменную на значение 2, то консоль лог все равно
выведет 1. И это потому примитивные типы, когда они передаются в функцию, они передаются по значению. Образ мышления о том, что
на самом деле означает pass-by value, есть по-настоящему передача копии переменной A. Поэтому все что вы делаете с переменной
внутри тела функции не будет никак влиять на переменную A во внешней области видимости.

```javascript
'use strict'

var a = 1;
function foo(a) {
    a = 2;
}
foo(a);
console.log(a); // 1
```

#### Что же такое pass-by reference? 

Когда вы передаете что-то по ссылке, вы передаете что-то указывающее на что-то еще. Так с передачей JavaScript объект
передает его по ссылке, когда изменяете свойство этого объекта изнутри функции, изменения будут отражены во внешней
области видимости. Когда вы передаете объект в функцию, вы передаете не ссылку объекта, вы передаете что-то указывающее на 
А объект.

```javascript
'use strict'

var a = {};
function foo(a) {
  a.moo = false;
}
foo(a);
console.log(a); // Object {moo: false}
```

Позвольте мне изменить код немного. Внутри функции вместо изменения свойства A, я буду изменять именно то на что указывает
A переменная. И если мы перезагрузим страницу, то увидим, что переменная А не изменилась. В JavaScript когда мы говорим, что
мы передаем по ссылке (pass-by reference), мы не можем изменять то на что указывает A, поэтому если вы передаете в A, это не
изменит то на что оно указывает. Мы можем только изменять свойства А во что-то другое или добавлять свойства. Мы не можем
изменять то на что указывает А.

```javascript
'use strict'

var a = {'moo': 'too'};
function foo(a) {
  a = {'too': 'moo'};
}
foo(a);
console.log(a); // Object {moo: 'too'}
```


## Types Equality

### Какие типы бывают в Javascript?

Сейчас в ECMAscript 5 спецификация определяет 6 различных типов в JS. У нас есть 5 примитивных типов и один непримитивный
тип. 

```javascript
"use strict";

Boolean // true, false
Number  // 1, 1.0
String // "a", 'a'
Null  // null
Undefined // undefined

Object  // {} , new Object()
```

Со стороны примитивных типов у нас есть boolean, number, string, null, undefined и непримитинвый тип - объект.

Так мы можем использовать функцию в JS, называемую **typeof**, способную выводить тип значения, которую мы в нее передадим.

```javascript
typeof(1) // "number"
typeof('a') // "string"
typeof('true') // "boolean"
typeof('undefined') // "undefined"
typeof('null') // "object"
typeof({}) // "object"
```

Проблема возникает, когда мы получаем тип примитива Null, потому что функция, возвращает нам, что это объект, хотя
null - null. И этот баг известен уже очень давно, к сожалению его пофиксить в данный момент невозможно, так как это
вызовет ряд проблем.

На самом деле вопрос простой, но можно задать посложнее, например:

Какая разница между динамически типизированным языком как JavaScript и статически типизированным языком, таким как Java?

Представьте, что мы пишем Java код. Поэтому если мы пишем Java, мы можем определить строку так:

```java
String a = 'moo';
```

И так должна создаться переменная называемая А и в статически типизированном языке, как Java, мы говорим Java, что эта
A будет содержать строки и только строки. И затем мы говорим Java что переменная А будет содержать строку moo. Так
в статически типизированном языке как Java перед тем как код запустится, пока вы пишите код, вы должны указать, какие
именно типы каждая переменная должна содержать. Если позже вы попытаетесь сделать что-то подобное

```java
String a = 'moo';
a = 1; // Вызовет ошибку
```

Потому что Java знает, данная переменная может содержать только строки и вы пытаетесь заставить переменную содержать число.

Когда как в JavaScript мы можем сделать что-то подобное:

```javascript
var a = 'moo';
```

В JS у нас есть переменная под названием А, но тип переменной А на самом деле определяется тем значением, которое мы
присвоили А. Если мы используем функцию typeof(a), то получим результат "string". Но позже мы можем изменить переменную
А и указать,  что она равна числу 1 и теперь typeof будет показывать нам "number"

```javascript
var a = 'moo';
typeof(a); // string
a = 1;
typeof(a); // number
```

Это и означает динамически типизированный язык. С JavaScript тип переменных определяется динамически в ран тайме, когда мы
действительно запускаем приложение. В то время как в Java typeof переменной определяется статично, в то время когда мы пишем
код для нашего приложения.

В чем преимущество одного над другим? Я бы сказал, что нам самом деле нет чистого преимущества одного над другим.
Они оба полезные в разных сценариях и в различных ситуациях. С динамически типизированным языком вы проще заходите и погружаетесь,
низкий порог вхождения. Он намного легче в написании быстрых JS приложений или быстрых Python приложений. Но вы сможете
обнаружить ошибки только в ран тайме. В тот момент, когда приложение реально запущено. Если, возможно, в нашей логике
приложения некоторая функция ожидала, что А будет строкой, но мы каким-то образом изменили ее на число в какой-то момент
выполнения, мы обнаружим это только тогда, когда запустим приложение и возможно оно выдаст ошибку или даже хуже, возможно
ошибке в самой программе и затем приложение продолжит молча вести к большим и к худшим ошибкам, ведущим к еще большим
ошибкам. 

В то время как в статически типизированном языке, как Java, когда мы компилируем приложение, компилятор сам бросит ошибку, если мы
создадим переменную, содержащую неверный тип. ТАк мы можем раскрыть проблемы очень очень рано в нашем цикле разработки.
Это происходит за счет времени создания наших приложений и немного сложнее написание.

Есть и другие преимущества использовать статически типизированный язык. Некоторые проблемы, связанные с управлением памятью
и производительностью, могут более жестко контролироваться статически типизированными языками по сравнению с динамически
типизированными языками. 

Другой последующий вопрос, который я могу задать. 
Вы могли бы заметить, что в наших примитивных типах, смешанных между собой, у нас есть два типа, которые, возможно,
действительно означают одно и то же. У нас есть null, и у нас есть undefined. 
У нас есть два, казалось бы, избыточных способа представить эту концепцию бесполезной, но на самом деле между ними 
есть небольшая разница.

Так, если я объявлю переменную А в нашем приложении и вызову console.log(a)

```javascript
var a;
console.log(a); // undefined
```

Вы можете видеть, что я на самом деле не назначил значения для этой переменной и поэтому приложение не знает какого типа
А, коносоль лог выведет undefined. Потому что это то, что JavaScript устанавливает для переменной А, когда он не знает тип
переменной, когда переменная не была инициализированная с другим значением. Так undefined используется JavaScript и
означает отсутствие значения. Он используется для неинициализированных переменных, поэтому такие переменные, как А, не 
были инициализированы другим значением. Он используется для пропуска параметров в функциях, и он используется для 
неизвестных переменных и свойств для таких вещей, как объект window.

Поэтому если мы сделаем window.hello и это несуществуемое свойство в объекте window, то вернется undefined. 

Так для меня undefined - это основная функция JavaScript, она используется движком JavaScript, чтобы сообщить вам, что
это либо неинициализировнная переменная, либо параметр, осутствующий в списке параметров функции, либо, возможно,
неизвестное свойство объекта.

В то время как, если бы мы напечатали var a = null;

```javascript
var a = null;
console.log(a); // null
```

То консоль вывело бы как очевидно null. Но я бы сказал, что null используется программистами для обозначения отстутствия 
значения. Но JavaScript движок никогда не установит значение в null для вас, он всегда будет устанавливать undefined, если
он не значет что это. Только программисты устанавливают переменную в null. Я думаю, что это тонкое, но важное различие
между undefined и null.

Другой интересный факт относительно null и undefined, который я думаю представляет интерес, пришедший из другого
статически типизированного языка. В языке, например, Java, если мы написали String a = null, и я знаю, что это недопустимый
код в JavaScript, но здесь мы говорим, что есть переменная с именем А, и это строковая переменная и значение этой переменной
в том, что мы еще не знаем, что это такое, поэтому я хочу сказать, что это null. В статически типизированных языках 
null - это не само значение, а концепция отсутствия значение, тогда как в JavaScript Null - значение. И мы знаем, что
null - значение, потому что мы можем написать typeof null и он будет иметь тип - object(null). TypeOf Null имеет значение
null, а тип Null имеет одно значение и только одно значение, которое является строкой, которая является ключевым словом null.

Тоже самое касается undefined.

### Какая разница между == и ===

Первый вариант - двойное равенство, которое называется равенством. Тройное равенство - строгое равенство в JavaScript.

Если вкратце, то тройное равенство проверяет как и тип, так и равенство значений, тогда как двойное равенство проверяет
равенство только значений слева от оператора и справа. Лучший способ объяснить и понять различия между равенством и 
строгим равенством это только черзе ряд примеров.

Например если мы будем писать в консоли:

```javascript
0 === 0; // true
0 !== 1; // true
0 == 0; // true
0 != 1; // true
```

Все это очень очевидно, а вот если мы начнем использовать другие значения:

```javascript
'' == '0' // false
0 == ''  // true
0 == '0'  // true

0 === '' // false
0 === '0' // false
```

Во время двойного сравнения JavaScript пытается разумно найти способ преобразования обоих значений, чтобы они были одного
типа. Поэтому в примере 0 == '0', JS пытается конвертировать число с левой стороны в строку, потому что он обнаруживает
строку с правой стороны и мы видем, что JS делает используя функцию String и передает в нее параметр 0

```javascript
String(0) == '0' // вот что на самом деле происходит при сравнении числа и строки
```

В JavaScript это называется Приведение типа (type coercion) и когда вы используете оператор двойного сравнения или
оператор нестрого равенства, это то, что JS пытается сделать, он пытается разумно привести оба значения к одному типу.



