#Язык описания тренировок
Язык служит для описания упражнений, тренировочных сессий и выстраивания всего тренировочного процесса в целом для тренажера "Кошка ловит мышь".
## Форматирование кода
Для текста описания должна использоваться кодировка UTF-8. 
 
Язык является регистронезависимым, то есть символы в верхнем и нижнем регистрах не различаются.

Для оформления кода можно использовать как пробелы, так и табуляцию. Пробельные символы не оказывают влияния.

Переносы строк оказывают влияние: каждая команда должна быть написана на отдельной строке, в конце строки может находиться комментарий.
Между строками с командами могут находиться любое количество пустых строк или строк с комментариями.

Комментарии могут быть только однострочными и начинаются с двух косых черт `//`. Комментарием считается часть строки, начиная с двух косых черт и до конца строки. Все что записано в комментариях - игнорируется.
## Типы данных
В языке описания допустимы следующие типы данных:
* число
* строка
* временной интервал (длительность)
* цвет
* мишень
* событие
* канал
### Числа
В языке допустимы только натуральные целые числа без знака и нуль.
```
12
0
34567
```
### Строки
Текстовые строки выделяются двойными или одинарными кавычками. Если в строке необходимо использовать символ двойной кавычки, то строка заключается в двойные кавычки, и наоборот, если в строке используются символы одинарной кавычки, то строка заключается в двойные кавычки.

В строке разрешено использовать только символы, имеющие соответствие в кодировке ASCII, так как при преобразовании в байткод строки конвертируются в ASCII кодировку. В строках допустимо использование букв русского алфавита. 

В строках запрещено использовать символы перевода строки и иные непечатаемые символы, за исключением пробела. Длина строки не должна превышать 32 символа.
 ```
"test string"
'тестовая строка'
"ошиб"ка" // ошибочная строка
```
### Временные интервалы

Временной интервал или временная длительность обозначает длительность какого-либо процесса или задержку до наступления какого-либо события. 
Временные интервалы указываются в формате `число` и `ед.изм.` без пробелов между ними, где `число` является целым числом без знака, а `ед.изм.` обозначает единицы измерения времени, в которых выражено числовое значение. Допустимы следующие единицы измерения:
 * `ms` - миллисекунда, является единицей измерения времени по умолчанию и может не указываться
 * `s` - секунда, 1000 миллисекунд
 * `m` - минута, 60 секунд
 * `h` - час, 60 минут
 * `d` - сутки, 24 часа
 
 В одном обозначении временного интервала может быть указано несколько последовательных групп `число` и `ед.изм.`, что интерпретируется как одно значение. 
 Например:
 ```
300 // длительность 300 миллисекунд, единица измерения пропущена, по умолчанию
250ms // длительность 250 миллисекунд
20s // 20 секунд или 20000 миллисекунд
3m10s // 3 минуты 10 секунд или 190000 миллисекунд
120s // 120 секунд или 120000 миллисекунд
1h23m35s120ms // 1 час 23 минуты 35 секунд 120 миллисекунд или 5015120 миллисекунд
```
Максимально допустимое значение для длительности составляет `49d17h2m47s295ms`.
При преобразовании в байткод все длительности переводятся в миллисекунды.
Значения минут и секунд могут быть больше 60, значение часов может быть больше 24.

#### Функция случайного значения
С временными интервалами (длительностями) связана встроенная функция выбора случайного значения. Функция служит для выбора случайного значения длительности в заданном интервале длительностей. Формат вызова функции:
`RND ( min, max )`, где `min` и `max` - минимальная и максимальная длительности.
Например:
```
RND(500ms,2s) // выбор случайного значения в интервале от 500 миллисекунд до 2 секунд
RND(0,1s)     // выбор случайного значения в интервале от 0 до 1 секунды
```
Если в функции указанное минимальное значение больше максимального, то параметры меняются местами.
### Цвет
 Цвет используется для установки яркости и оттенка светового сигнала.
Существуют три способа представления цвета:
* в шестнадцатиричном формате
* с помощью функции RGB
* представление ключевыми словами

Все три способа полностью совместимы с представлением цвета в интернете, поэтому для подбора цвета можно использовать любые браузерные или online инструменты. Исключение составляет только представление ключевыми словами, поскольку не все названия цветов реализованы.

#### Шестнадцатиричный формат 
Представление в виде трёх пар шестнадцатеричных цифр, где каждая пара отвечает за свой цвет:
* две первые цифры — красный
* две в середине — зелёный
* две последние цифры — синий
Возможно также краткое представление цвета в виде #ABC, что будет интерпретировано как #AABBCC. Например:
```
#FF00FF // фуксия, фиолетовый
#F0F    // фуксия, фиолетовый
#FFFFFF // белый
#fff    // белый
```
#### Функция RGB
Цвет может быть представлен с помощью функции в виде `RGB(*,*,*)`, где `*` — числа от 0 до 255, обозначающих количество соответствующего цвета (красный, зелёный, синий) в получаемом цвете. Например:
```
RGB(0,255,255) // бирюзовый
rgb(255,255,0) // желтый
```
#### Представление ключевыми словами, 
Представление ключевыми словами использует небольшой набор основных цветов:
```
red     // красный
green   // зеленый
blue    // синий
yellow  // желтый
cyan    // бирюзовый
magenta // фиолетовый, фуксия
```
### Мишени
Мишенью называется маленький шарик, который необходимо ловить.
В языке описания для мишеней используются идентификаторы в формате: `t*`, где * - число от 1 до 16. Полный список разрешенных мишеней:
```
t1, t2, t3, t4, t5, t6, t7, t8, 
t9, t10, t11, t12, t13, t14, t15, t16 
```
Также для указания мишеней можно использовать ключевое слово `all`, которое обозначает все мишени.
### События
Тип `событие` описывает события, которые могут происходить в процессе тренировки на тренажере. Таких событий четыре:
* `target` - событие постановки мишени
* `capture` - событие ловли или пропуска мишени
* `gesture` - событие жестового управления
* `timer` - события таймера
#### target
Cобытие постановки мишени. Событие `target` происходит в тот момент, когда пользователь устанавливает мишень на электромагнит.
При событии указывается идентификатор мишени. Например:
```
target t1 // первая мишень установлена
target all // установлены все мишени
```
#### capture
Событие ловли. Событие `capture` происходит в тот момент, когда пользователь поймал мишень или мишень не была поймана и упала.
При событии указывается идентификатор мишени. Например:
```
capture t1 // первая мишень поймана (или пропущена)
capture all // пойманы (или пропущены) все мишени
```
#### gesture
Событие управления. Событие `gesture` происходит при жестовом управлении тренажером.
Допустимы следующие жесты управления:
* `up` - взмах рукой от уровня датчика вверх
* `down` - взмах рукой от уровня датчика вниз
* `updown` - взмах рукой вверх-вниз перед датчиком
* `left` - взмах рукой от уровня датчика влево
* `right` - взмах рукой от уровня датчика вправо
* `leftright` - взмах рукой влево-вправо перед датчиком
* `forward`  - приближение ладони вперед к датчику
* `backward`  - удаление ладони от датчика
* `forwardbackward` - приближение-удаление ладони по оси датчика
* `clockwise` - вращение рукой по часовй стрелке
* `anticlockwise` - вращение рукой против часовой стрелки

При событии указывается название жеста. Например:
```
gesture up        // взмах рукой от уровня датчика вверх
gesture clockwise // вращение рукой по часовй стрелке
```
#### timer
Событие таймера. Событие `timer` происходит по истечении указанного в событии интервала времени. Событие может быть использовано в наборе событий для предотвращения блокировки в команде ожидания. (см. ниже)
 ```
 timer 10s // возникновение события через 10 секунд
 ```
### Канал
Тип `канал` служит для идентификации канала воздействия вибромотора. 
В языке описания для каналов используются идентификаторы в формате: `v*`, где * - число от 1 до 4. Полный список разрешенных каналов:
```
v1, v2, v3, v4 
```
Также для указания канала можно использовать ключевое слово `all`, которое обозначает все каналы.
## Идентификаторы
Идентификаторы используются для именования всей тренировочной сессии и отдельных упражнений, входящих в нее.
В идентификаторах можно применять только буквы латиницы, цифры и знак подчеркивания, причём первой всегда должна быть буква или знак подчеркивания, а далее может идти произвольная комбинация букв,цифр и знаков подчеркивания. Длина идентификатора не должна превышать 32 символа. Например:
```
MyFirstModule
Excercise_1
Excercise_2
```
## Структура описания тренировки 
Описание тренировки состоит из трех разделов:
* блок требований
* блок описаний упражнений
* блок тренировки

Описание тренировки должно начинаться с зарезервированного слова `module`, за которым через пробел на той же строке следует идентификатор тренировочной сессии.
```
module FirstTraining
```
Далее с новой строки следует блок разрешений. 
## Блок требований (requirements)
В блоке требований указываются требования к состоянию тренажера, при соответствии которым тренировочная сессия может быть выполнена. 
Могут быть заданы требования:
* targets
* vibro
* sound
* internet
* escape

Все требования кроме `targets` могут принимать значение 0 (не требуется). При значении 0 требование может быть полностью пропущено. 

### targets
Требование указывает на минимально необходимое количество мишеней. Требование имеет формат `targets *`, где * - число от 1 до 16, указывающее минимальное количество мишеней, которое должно быть одновременно доступно у тренажера для успешного проведения тренировочной сессии. Например:
```
targets 2 // у тренажера должно быть не менее двух мишеней
```
### vibro
Требование указывает на необходимость наличия дополнительного устройства управления вибромоторами, а также на минимально необходимое количество вибромоторов, которое должно поддерживать устройство. Требование имеет формат `vibro *`, где * - число от 1 до 4. В случае отсутствия устройства некоторые упражнения могут работать неправильно. 
```
vibro 2 // у тренажера должно быть 2 вибромотора
```
### sound
Требование указывает на необходимость наличия дополнительного устройства вывода звука. В случае его отсутствия некоторые упражнения могут работать неправильно.  Требование имеет формат `sound *`, где * - число от 1 (моно) или 2 (стерео).  
```
sound 2 // у тренажера должны быть подключены стереоколонки
```
### internet
Требование указывает на необходимость наличия доступа к сети интернет и к серверу системы. В случае его отсутствия некоторые упражнения могут работать неправильноТребование может принимать два значения on или off. При значении off требование может быть полностью пропущено. 
```
internet on // у тренажера должен быть доступ в интернет
```
### escape
Требование указывает на жест для прекращения выполнения упражнения или всей тренировки.
Требование имеет формат `escape <название жеста>`, где `<название жеста>` - название одного из жестов.  
```
escape gesture clockwise // прекращение упражнения и переход к следующему по жесту clockwise
```
## Блок описаний упражнений
В блоке находятся описания упражнений, которые могут быть выполнены в процессе тренировки. Возможно наличие одного или нескольких описаний, а также допустимо отсутствие описаний, тогда блок будет пустым.

Упражнение - это именованный набор команд, который имеет определенное смысловое значение. Упражнение используется для упрощения описания тренировки. Определение упражнения начинается с ключевого слова `exercise`, за которым через пробел на той же строке следует идентификатор упражнения и открывающая фигурная скобка. Далее в фигурных скобках перечислены
команды, которые исполняются в упражнении. Например:
```
exercise MyFirstExercise { // это название упражнения
	// тут набор команд
}
```
Далее, в описании тренировки упражнение может быть вызвано для выполнения с указанием количества повторений. Например:
```
MyFirstExercise(5) // выполнить упражнение 5 раз
```
Возможна ситуация, когда количество повторений упражнения задано достаточно большое и пользователь может захотеть перейти к следующему упражнению, не выполняя все повторения. В таком случае выполнение упражнения можно прекратить по жесту, указанному в требовании `escape`.

## Блок тренировки 
В блоке тренировки перечислены команды и вызовы упражнений, которые должны быть выполнены в процессе тренировочной сессии.
Блок тренировки начинается с ключевого слова `training`, затем после открывающей фигурной скобки со следующей строки перечисляются команды и упражнения, которые должны быть выполнены. В конце находится закрывающая фигурная скобка.
После закрывающей скобки блока тренировки никакие команды не допускаются. Например:
```
training { // тут начинается блок тренировки
    // тут команды
}
```
## Описание команд
### Команды вывода на экран
`print "<строка>"` - печать строки на экране. На экран начиная с левого верхнего угла выводится текст строки.
Для вывода доступны печатаемые символы ascii, в том числе кириллица.
```
print "установите"
print "мишень"
```

`icon <иконка>`  - вывод изображения иконки на экран. Доступны несколько предопределенных иконок: "stop", "alert", "wait". ()
```
icon "stop"
```
`clear` - полная очистка экрана.
```
clear
```

### Команда сброса
`drop <мишень> <длительность>` - сброс мишени с задержкой времени <длительность>. По команде удерживающий электромагнит отключается и мишень падает. Можно задать интервал ожидания до момента отключения с помощью параметра `длительность`. Для этого может быть использована функция `rnd()`.
```
drop  t1 3s          // мишень t1 упадет через 3 секунды
drop  t1 rnd(1s, 5s) // мишень t1 упадет через случайное время от 1 до 5 секунд
drop all 0           // все мишени упадут немедленно
```
### Команды задержки
`delay <мишень> <длительность>` - задержка мишени на интервал времени `<длительность>`. Команда может использоваться для установки постоянной задержки перед сбросом мишени. Если мишень уже сброшена, команда не имеет смысла и выполняться не будет. **Полезность команды под вопросом, может быть удалена.**
```
delay t1 3s          // пауза для мишени t1 на 3 секунды
```

`sleep <длительность>` - общая пауза на время `<длительность>`. По команде выполнение тренировки или упражнения приостанавливается. После истечения времени выполнение продолжается со следующей команды. 
Команда может использоваться для организации отдыха между упражнениями.
```
print "Отдых"       // сообщаем пользователю о необходимости отдохнуть
sleep 2m            // прерываем тренировку на 2 минуты
```

### Команда ожидания
`wait <набор событий>` - ожидание возникновения события. В команде ожидания в качестве параметра может использоваться одиночное событие или набор событий.
Набор событий - это перечисление ряда событий, разделенных символом запятой `,` или обратной косой чертой `/`. 

Если используется разделитель запятая, то в команде ожидается возникновение всех событий, перечисленных в наборе. Если используется разделитель косая черта, то ожидается возникновение любого одиночного события из перечисленных в наборе. Например:
```
wait target t1, target t2    // ожидание установки мишеней t1 и t2
wait target all              // ожидание установки всех мишеней
wait gesture up/gesture left // ожидание любого из жестов: вверх или влево
wait capture t1/capture t2/timer 5s   // ожидание ловли любой из мишеней t1 или t2 или таймера через 5 секунд
wait ожидание gesture up/gesture left // ожидание любого из жестов: вверх или влево
```

### Команды подачи сигналов
В тренажере реализованы четыре типа сигналов:
* световой
* звуковой 
* звуковой (тоновый) 
* тактильный (вибрация) 
Все сигналы имеют две команды: включение сигнала и обратная - выключение сигнала.

#### Световой сигнал
`light <мишень> <длительность> <цвет>` - включить источник света. Команда включает источник света с заданным цветом <цвет> в течение времени <длительность>, соответствующий идентификатору мишени <мишень>. По истечении времени источник света отключается автоматически.

`light <мишень> off`  - выключить свет. Команда немедленно выключает источник света, соответствующий идентификатору мишени <мишень>. Допускается использование ключевого слова `all` в качестве значения параметра <мишень> для управления всеми источниками одновременно.
Например:
```
light t1 10s cyan // для мишени t1 включить световой сигнал бирюзового цвета на 10 секунд
```

#### Тоновый сигнал
`beep <мишень> <длительность> <скважность>` - включить источник тонового звука. Команда включает источник звука (бипер) с тоном <скважность> в течение времени <длительность>, соответствующий идентификатору мишени <мишень>. По истечении времени источник звука отключается автоматически.

`beep <мишень> off` - выключить звуковой тон. Команда немедленно выключает источник тонового звука, соответствующий идентификатору мишени <мишень>.

В командах допускается использование ключевого слова `all` в качестве значения параметра <мишень> для управления всеми источниками тонового сигнала одновременно.

#### Звуковой сигнал (не реализовано в текущей версии)
`sound <канал> <длительность> <громкость> <название сигнала>` - включить источник звука. Команда включает источник звука (динамик). По истечении времени источник звука отключается автоматически. 

Параметры команды:
* <канал> - обозначает стереоканал и может принимать значения left, right или all, что соответствует левому, правому каналу или обоим каналам одновременно. 
* <длительность> - интервал времени, в течение которого звук будет воспроизводиться. 
* <громкость> - уровень громкости звука. Может изменяться от значения 0 (нет звука) до значения 255 (максимальный звук).
* <название сигнала> - строка с названием звукового файла, в котором находится запись сигнала. Может использоваться как имя файла во внутренней файловой системе SPIFFS, так и относительный URL сигнала, лежащего на сервере проекта
 
`sound <канал> off` - выключить звук. Команда немедленно выключает источник звука, соответствующий идентификатору канала <канал>.

В командах допускается использование ключевого слова `all` в качестве значения параметра <канал> для управления всеми источниками звука одновременно.

  
#### Вибросигнал
Вибросигнал воспроизводится с помощью вибромоторов, управляемых через дополнительное устройство управления вибромоторами. По истечении времени вибросигнал отключается автоматически.

`vibro <канал> <длительность> <скважность>` - включить сигнал вибромотора. Аналогично команде `beep`, команда включает вибромотор, соответствующий идентификатору канала <канал> с уровнем вибрации <скважность> в течение времени <длительность>. Допускаются номера каналов от 1 до 4.

`vibro <канал> off`  - выключить сигнал. Команда выключает вибрацию вибромотора, соответствующего идентификатору канала <канал> немедленно.

В командах допускается использование ключевого слова `all` в качестве значения параметра <канал> для управления всеми вибромоторами одновременно.

## Полный пример описания тренировки
```
module FirstTraining // название тренировки
	targets 2
	vibro 2
	sound 1
	internet on
	escape gesture clockwise

// первое упражнение
exercise First {
	clear
	print "установите"
	print "мишень"
	wait target t1, gesture down
	light t1 100ms #f0f
	drop  t1 rnd(300ms, 3s)
}

// второе упражнение
exercise Second {
	clear
	print "установите"
	print "мишень"
	wait target t1, gesture down
	vibro t1 v1 300ms 128
	drop  t1 rnd(300ms, 3s)
}

// тренировка
training {
	clear
	print "первое"
	print "упражнение"
	light all 5s #800
	sleep 5s
	First(5)
	clear
	print "Отдых"
	sleep 30s
	clear
	print "второе"
	print "упражнение"
	light all 5s #800
	sleep 5s
	Second(5)
	clear
	print "тренировка"
	print "завершена"
}
```