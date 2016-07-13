# Поиски святого грааля: Как я пришёл к выражениям для элементов и как пользоваться ими уже сегодня

Какое-то время я испытывал трудности с тем, **на что способен CSS**. Тот, кто
разрабатывает адаптивные веб-дизайны, охотно признает, что в CSS есть
разочаровывающие недостатки, которые заставляют применять препроцессоры, плагины
и прочие утилиты для написания тех стилей, которые при помощи одного только CSS
написать невозможно. Но даже эти утилиты имеют свои ограничения в том, чем они
могут нам помочь.

Педставьте на мгновение строительство здания. Если вы строите большое сооружение
из непрочного материала, то потребуется много внешних стяжек и подпорок, чтобы
оно могло устоять. При строительстве сайта из HTML, CSS и JavaScript в качестве
таких стяжек могут выступать фреймворки, плагины, препроцессоры, транспилеры,
редакторы, пакетные менеджеры и процессы сборки.

Вместо того, чтобы добавить очередной плагин в довесок к имеющимся, я задумался:
а может, **расширив один из основных языков, CSS**, мы могли бы усилить тот
материал, из которого строятся сайты, и разрабатывать более хорошие, прочные
сайты, требующие меньше стяжек и инструментов для постройки.

## Текущее положение дел

При использовании утилит вроде препроцессоров мы пишем CSS в кратком виде, а
затем он разворачивается в своё полное представление (перед тем, как он будет
использован в браузере). Плагины на странице могут взамодействовать с элементами
на одном уровне, но чтобы применить стили, им приходится или писать стили CSS
прямо в HTML, или манипулировать именами классов, которым соответствуют разные
правила CSS. В обоих случаях нам приходится писать или генерировать нужный нам
CSS перед тем, как страница загрузится.

### Проблема

Проблема такого подхода в том, что даже самые лучшие плагины часто **требуют
настройки и конфигурации** под каждый используемый вами макет. К тому же,
если JavaScript пишет стили за вас, при рефакторинге или переиспользовании кода
может быть трудно следить за тем, чтобы логика плагина работала правильно с
вашими стилями CSS.

Другая проблема с препроцессорами состоит в том, что любая ошибка в краткой
записи быстро разбухает в огромное месиво после преобразования CSS в полную
форму. При использовании плагинов мы добавляем много потенциальных точек отказа.
Может быть, мы используем множество плагинов, чтобы достичь тех немногих целей,
которые, возможно, были бы и не нужны, будь CSS немного мощнее. Это создаёт
лишние накладные расходы, которые разрабочикам сложнее поддерживать, браузерам
отрисовавать, а пользователям загружать.

### Есть ли надежды на будущее веб-разработки?

В 2013 Тайсон Матанич (Tyson Matanich) написал статью, озаглавленную
[«Медиа-выражения не решение: Полифилл выражений для элементов»][1], в которой
представил широкой публике концепцию выражений для элементов (element queries).
С неё началось обсуждение того, как можно было бы обойти недостатки CSS при
помощи плагинов и полифиллов.

С тех пор, пока мы ждём улучшения возможностей CSS, появился ряд плагинов,
позволяющих разрабочикам использовать выражения для элементов несколькими
различными способами.

### Что такое выражения для элементов?

Выражения для элементов похожи на медиа-выражения, с тем отличием, что правила
зависят от свойств конкретного элемента, а не порта просмотра браузера.

## Как появился EQCSS

В конце 2013 мне довелось работать над фронтендом веб-приложения на Ruby on
Rails. Приложение должно было отображать пользователям подробную информацию, и
целью стала разработка адаптивного интерфейса, который бы одинаково подходил и
для телефонов, и для планшетов, и для десктопов. Это вызвало ряд затруднений,
оно из которых было в том, что большую часть важной информации предполагалось
отображать в виде таблиц — да, самых настоящих элементов `table` (финансовые
транзакции, спортивные рекорды и т.п.).

![][3]
_Проект EQCSS появился как результат исследования выражений для элементов.
Теперь, когда он [наконец выпущен][5], вы тоже можете им воспользоваться.
[Посмотреть демо.][4]_

Я написал адаптивные стили с использованием медиавыражений, которые отображали
элемент `table` правильно для браузеров различных размеров. Но как только одна
из таких таблиц оказывалась в шаблоне, содержащем боковую панель, внезапно
все ключевые точки моих медиавыражений ломались. Они попросту не учитывали
двухсотпиксельную боковую панель, элементы наползали друг на друга и всё
выглядело сломанным.

Другое препятствие: имена пользователей могли содержать от 3 до 20 букв, и я
осознал, что хочу. чтобы **размер шрифта имени пользователя изменялся в
зависимости от числа символов** в нём. Мне нужно было располагать все имена
пользователей в боковой панели, и было нелегко подобрать такой размер шрифта,
чтобы имена из 20 букв вместились, а трёхбуквенные хорошо читались.

Чтобы решить проблемы наподобие этих, я часто копировал медиавыражения целиком,
дублировал большие куски кода, просто из-за того, что мне не хватало более
разумного способа применения адаптивных стили к каждому макету. Я также
полагался на JavaScript, но такое решение тоже не было красивым, я писал почти
идентичные функции, каждую для своей страницы, применяющие стили к тем местам,
с которыми CSS не справлялся. Позже кодовая база распухла от всего этого
копипаста, и вносить изменения в код стало затруднительно.

Я знал, должно же быть решение получше. И через какое-то время я начал думать,
что мне не нужны медиазапросы, то, что мне нужно — это выражения для элементов!

### Исследование и разработка

С начала 2014 я начал экспериментировать с различными способами сообщить CSS о
свойствах отображаемого на странице элемента, это помогло бы улучшить мои стили.
Я надеялся найти подход к написанию стилей, сочетающий красоту CSS и мощь
JavaScript.

Вот некоторые из неудачных экспериментов: добавление атрибутов тегам HTML в
в надежде получить поддержку адаптивности и попытки найти способ внедрять целые
блоки CSS внутрь конструкций `if` в JavaScript, чтобы создать этакого монстра
Франкенштейна, сшитого нитками из JavaScript и CSS.

Но вместо того, чтобы облегчать задачу, все мои неудачные попытки имели кое-что
общее: они добавляли ещё больше работы! Я знал, что правильное решение должно
упрощать и облегчать работу, так что я продолжал поиски. После всех
экспериментов у меня выработалось чёткое представление о том, как должен
выглядеть синтаксис, при котором выражения для элементов будут работать хорошо.

Как я говорил выше, у сайта, построенного из HTML, CSS и JavaScript, роль стяжек
выполняют фреймворки, плагины, препроцессоры, транспилеры, редакторы, пакетные
менеджеры и процессы сборки. Вместо того, чтобы добавить очередной плагин в
довесок к имеющимся, я задумался: а может, расширив один из основных языков,
CSS, мы могли бы усилить тот материал, из которого строятся сайты, и
разрабатывать более хорошие, прочные сайты, требующие меньше стяжек и
инструментов для постройки.

### Рождение синтаксиса

Имея лучшее представление о том, какой синтаксис мне требуется, в концу 2014
я обратился к Максиму Узье (Maxime Euzière), феноменальному JavaScript
код-гольферу, и спросил его, возможно ли расширять CSS на уже загруженной
странице при помощи браузерного JavaScript. Он не только сообщил мне, что это
возможно, но и предложил свою помощь в реализации! Мы назвали синтаксис EQCSS,
как сокращение от «element query CSS». Также название намекает на слово
«excess» (англ. «избыток»), потому что он делает то, что превышает возможности
CSS.

### Потребность

Моё требование к синтаксису было таковым: он должен быть максимально **похожим
на CSS**. Настолько похожим, что даже подсветка синтаксиса не должна замечать
подвоха и считать, что это обычный CSS. Так я наметил синтаксис выражений
для элементов, настолько логичный, что люди удивляются, как же так вышло, что
его ещё не существует.

Я знал, что если мы собираемся расширять браузерную поддержку CSS используя
JavaScript, то плагин должен быть настолько легковесным и простым, насколько
это возможно. Так что вариант оформить это в виде плагина библиотеки вроде
jQuery вычеркнули из списка. Мне нужна была библиотека на чистом JavaScript,
которая добавляла бы требуемые возможности из будущего в те браузеры, которые
мне нужно поддерживать сейчас.

На текущий момент сообщество CSS сосредоточено на обсуждении нестандартных
директив, и обсуждение выражений для элементов
[до сих пор несёт предварительный характер][9]. Скорее всего, нас отделяют годы от
какой-либо официальной спецификации CSS по возможностям вроде этой. Да и будь
у нас спецификация, перед тем, как использовать эти возможности при разработке
сайтов, нам всё равно бы пришлось ждать, пока достаточное количество браузеров
реализует её.

Нет смысла ждать, пока эти возможности попадут в CSS, если нам нужна эта
функциональность, чтобы создавать и поддерживать сайты уже сейчас.

### Результат

Результатом этого исследования стало создание синтаксиса, включающего в себя
ряд новых продвинутых условий для адаптивности, стилей с изолированной областью
применения и новых селекторов для выборки элементов, а также библиотеку на
чистом JavaScript под названием [EQCSS.js][11]. Плюс, поддержка Internet
Explorer (IE) 8 осуществляется при помощи необязательного внешнего полифилла.
И плагин и полифилл выпущены под [лицензией MIT][13], их использование бесплатно
для всех.

## Где полезны выражения для элементов

### Разработка плагинов

При создании интерфейсных компонентов и виджетов разработчики часто оказываются
ограниченными медиа-запросами. Нам часто приходится делать выбор между тем,
чтобы создавать множесто различных лейаутов, настраиваемых при помощи плагина, и
тем, чтобы сделать интерфейс как можно проще, так что одно решение будет
работать в большинстве ситуаций.

Однако, при разработке плагинов и интерфейсов с выражениями для элементов мы
с лёгкостью можем писать адаптивные стили для любой нужной нам стиуации.
Получается безотказное решение, неважно, какое содержимое пользователь решит
поместить в блок, или где плагин находится. Если мы зададим стили виджета для
всех ширин от 150 до 2000 пикселей, то куда его на сайте ни помести, он везде
будет выглядеть отлично.

### Компоненты шаблона

На этапе прототипа сайта часто происходит реорганизация элементов дизайна, а
сам дизайн представляется как набор модульных компонентов. Если вы на этом этапе
написали медиа-выражение, иногда может оказаться, что это **преждевременная
оптимизация**. Разрабатывая с использованием выражений для элементов, вы
оставляете условия адаптивности независимыми от макета, что даёт вам больше
гибкости и возможность перемещать элементы без необходимости сильно переписывать
стили.

Мне кажется наиболее полезным использование выражений для элементов при
разработке дизайна или набросков таких вещей как:

*   меню навигации;
*   модальные диалоги;
*   формы регистрации и авторизации;
*   подвалы;
*   блоки расценок;
*   лендинги;
*   таблицы;
*   кнопки вкладок;
*   аккордионы;
*   боковые панели;
*   медиапроигрыватели;
*   блоки отзывов.

Любой элемент дизайна можно изолировать и переместить куда угодно — с одной
страницы на другую, или с одного сайта на другой.

### Поддержка устройств

Одна из проблем, с которыми вы сталкиваетесь при поддержке мобильного веба — это
разнообразие устройств. Рынок устройств сегодня более фрагментирован, чем
когда-либо, и новые планшеты и телефоны появляются каждый день. Мы уже не в
состоянии поддерживать список поддерживаемых браузеров и устройств, так что
исключительно важно знать наверняка, что дизайн работает везде, даже на ещё не
выпущенных устройствах.

Используя выражения для элементов, вы можете разрабатывать сайты лучше и
избавиться от различий между браузерами.

Есть большое количество недавних статей про необходимость выражений для
элементов, и в них есть множество примеров их применения в деталях. Так давайте
же разберёмся, как их использовать!

## Как писать выражения для элементов

Начать работать с EQCSS легко. Всё, что вам нужно для того, чтобы использовать
синтаксис EQCSS,— это [подключить скрипт][15] где-либо в HTML.

### Загрузка EQCSS.js

Если вы хотите склонировать проект EQCSS GitHub, вы можете набрать:

    git clone https://github.com/eqcss/eqcss.git

Если вы используете npm, вы можете добавить EQCSS в проект следующей командой:

    npm install eqcss

### Добавление EQCSS.js в HTML

После того, как вы загрузите EQCSS, вы можете подключить его в HTML используя
тег `script`:

    <script src="EQCSS.js"></script>

Этот файл (`EQCSS.js`) включает в себя поддержку всех современных браузеров,
включая IE9 и выше. Для поддержки IE8 нам пришлось бы использовать кучу других
полифиллов. Задумайтесь, в IE 8 даже медиа-выражения без полифиллов не
поддерживаются, поэтому достаточно удивительно, что нам удалось заставить там
работать выражения для элементов. Чтобы добавить EQCSS поддержку IE8, добавьте
следующую строчку перед местом подключения основного плагина:

    <!‐‐[if lt IE 9]><script src="EQCSS‐polyfills.js"></script><![endif]‐‐>

### Запуск EQCSS

По умолчанию плагин EQCSS рассчитывает все стили, которые он найдёт в документе,
в момент загрузки страницы и когда обнаружит изменение размеров окна,
поведение похожее на медиа-запросы. Также можно вручную вызвать `EQCSS.apply()`
из JavaScript, чтобы пересчитать стили в любой момент, это может быть полезно,
если содержимое страницы меняется.

### Написание Element Query CSS

Плагин EQCSS.js может читать стили несколькими различными способами. Вы можете
вставить EQCSS в любой тег `style` на странице. Также можно писать EQCSS во
внешних файлах стилей.

Если вам хочется хранить код для EQCSS отдельно от CSS, вы можете загрузить
стили EQCSS используя тег `script` с атрибутом `type` равным `text/eqcss`.
При использовании такого способа стили можно записать внутри тега, или указать
ссылку на внешний файл `.eqcss`:
`<script type="text/eqcss" src=styles.eqcss></script>` подгрузит файл с именем
`styles.eqcss`.

## Анатомия выражений для элементов

### Области применения стилей

Синтаксис EQCSS для написания выражений для элементов очень похож внешне на
медиа-запросы, но вместо `@media` мы начинаем выражение с `@element`.
Единственная дополнительная информация, которую нужно указать — это минимум один
селектор, к которому эти стили будут применяться. Например, вот так задаётся
область применения для элемента `<div class="widget">`:

    @element '.widget' {
    
    }

Часть кода между кавычками (в нашем случае `.widget`) может быть любым валидным
селектором CSS. Этим выражением мы создали новую область применения для элемента
`.widget`. Но мы пока что не указали ни одного условия адаптивности для этой
области, так что стили внутри неё будут применяться к элементу всегда.

Без возможности изолировать стили, привязывая их к одному или нескольким
элементам (вместо страницы целиком), у нас не получилось бы применять
условия адаптивности только к этим элементам. Но как только мы создаём область
видимости, становится легко использовать продвинутые возможности EQCSS,
например, мета-селектор `$parent`, потому что у JavaScript теперь есть опорная
точка, от которой можно рассчитывать вещи вроде `parentNode` элемента. Это
важно!

Да, в CSS уже есть селектор прямого потока, с комбинатором `>`, который
позволяет выбирать дочерние элементы указанного элемента. Но CSS пока не
предоставляет никаких возможностей путешествовать вверх по семейному древу и
выбирать родителей, то есть, элементы, содержащие данный элемент. В спецификации
CSS 4 появился [селектор `:has()`][17], принципиально похожий на
[селектор `:has()`][19] в jQuery, но на сегодняшний день
[поддержка браузерами нулевая][21]. С областями применения мы можем обращаться
к родительским элементам по-другому.

Теперь, когда мы объявили область применения в контексте элемента `.widget`,
мы можем использовать его как точку отсчёта и обращаться к родителю:

    @element '.widget' {
      $parent {
        /* Эти стили применятся к родителю .widget */
      }
    }

Другой пример специальных селекторов, использующихся в выражениях для
элементов,— селекторы `$prev` и `$next`, они представляют предыдущий и следующий
соседние элементы соответственно. Опять-таки, в CSS можно добраться до
следующего соседа нашего виджета при помощи селектора вроде `.widget + *`, но
нет возможности пойти назад и получить элемент, который идёт непосредственно
перед другим элементом.

    <section>
      <div>Это предыдущий элемент</div>
      <div class="widget">Это элемент области определения</div>
      <div>Это следующий элемент</div>
    </section>
    <style>
      @element '.widget' {
        $prev {
          /* Эти стили применятся к элементу перед .widget */
        }
        $next {
          /* Эти стили применятся к элементу после .widget */
        }
      }
    </style>

### Выражения для элементов

Чаще всего разработчики использую медиа-выражения для адаптивного дизайна,
применяя стили на основе высоты или ширины порта просмотра браузера. Синтаксис
EQCSS поддерживает новые типы адаптивных условий. Вмсето того, чтобы работать
только с шириной и высотой браузера, вы можете написать стили, которые
применяются к элементу на основе его собственных свойств. Таких, например, как
количество дочерних элементов или количество символов или строк текста на
текущий момент.

Добавление этих условий в область определения стиля похоже на то, как вы пишете
медиа-выражения: нужно добавить `and (условие: значение)` для каждого нужного
условия. В этом примере мы проверяем, есть ли на странице элементы `.widget`
шириной минимум 500 пикселей.

    @element '.widget' and (min‐width: 500px) {
      /* Тут правила CSS */
    }

Подробно синтаксис выражений для элементов таков:

*   **выражений для элемента**
    `@element список_селекторов [ список_условий ] { код_css }` 
*   **список селекторов**
    `" селектор css [ "," селектор css ]* "`
*   **список условий**
    `and ( условие_выражения : значение ) [ "and (" условие_выражения ":" значение ")" ]*`
*   **значение**
    `число [ единица измерения css ]`
*   **условие выражения**
    `min-height | max-height | min-width | max-width | min-characters | max-characters | min-lines | max-lines | min-children | max-children | min-scroll-y | max-scroll-y | min-scroll-x | max-scroll-x`
*   **единица измерения css**
    `% | px | pt | em | cm | mm | rem | ex | ch | pc | vw | vh | vmin | vmax`

Вот другой пример, как написать выражение, при котором элемент `body` становится
красным, если ширина элемента `.widget` достигает 500 пикселей:

    @element '.widget' and (min‐width: 500px) {
      body {
        background: red;
      }
    }

Заметьте, что элемент `body` изменяется, когда `.widget` достигает определённой
ширины, но сам элемент `.widget` не меняется!

## Условия выражений для элементов

Ниже приведён полный список условий адаптивности, поддерживаемый EQCSS.

### Условия на основе ширины

*   `min-width`
    [демо для пикселей](http://codepen.io/tomhodgins/pen/MeKwaY "Min width in pixels"), [демо для процентов](http://codepen.io/tomhodgins/pen/ezJNpp "Min width in percent")
*   `max-width`
    [демо для пикселей](http://codepen.io/tomhodgins/pen/EyPjVg "Max width in pixels"), [демо для процентов](http://codepen.io/tomhodgins/pen/oLbXzG "Max width in percent")

### Условия на основе высоты

*   `min-height`
    [демо для пикселей](http://codepen.io/tomhodgins/pen/PzZqPd "Min height in pixels"), [демо для процентов](http://codepen.io/tomhodgins/pen/KMVpdO "Min height in percent")
*   `max-height`
    [демо для пикселей](http://codepen.io/tomhodgins/pen/EyPjPg "Max height in pixels"), [демо для процентов](http://codepen.io/tomhodgins/pen/xOZGZg "Max height in percent")

### Условия на основе количества

*   `min-characters`
    [демо для блочных элементов](http://codepen.io/tomhodgins/pen/vKLOLd "Min characters on block elements"), [демо для элементов формы](http://codepen.io/tomhodgins/pen/OXMVMB "Min characters on form inputs")
*   `max-characters`
    [демо для блочных элементов](http://codepen.io/tomhodgins/pen/pbgJyz "Max characters on block elements"), [демо для элементов формы](http://codepen.io/tomhodgins/pen/MeKwyY "Max characters on form inputs")
*   `min-lines`
    [демо](http://codepen.io/tomhodgins/pen/JKGdXN "Min lines demo")
*   `max-lines`
    [демо](http://codepen.io/tomhodgins/pen/oLbXxG "Max lines demo")
*   `min-children`
    [демо](http://codepen.io/tomhodgins/pen/dXGoMZ "Min children demo")
*   `max-children`
    [демо](http://codepen.io/tomhodgins/pen/mEVJPK "Max children demo")

### Условия на основе прокрутки

*   `min-scroll-y`
    [демо](http://codepen.io/tomhodgins/pen/OXMVNa "Min scroll Y demo")
*   `max-scroll-y`
    [демо](http://codepen.io/tomhodgins/pen/beEdpZ "Max scroll Y demo")
*   `min-scroll-x`
    [демо](http://codepen.io/tomhodgins/pen/ZOQGOb "Min scroll X demo")
*   `max-scroll-x`
    [демо](http://codepen.io/tomhodgins/pen/ezJNzJ "Max scroll X demo")

Вы можете сочетать любое количество этих условий в выражении для элемента и
получить по-настоящему многомерные адаптивные стили. Это предоставляет больше
гибкости и контроля над тем, как элементы выглядят. К примеру, чтобы уменьшить
размер шрифта заголовка, в котором содержится более 15 символов, и если меньше
600 пикселей по ширине доступно для отображения, вы можете применить совместно
условия `max-characters: 15` и `max-width: 600px`:

    h1 {
      font‐size: 24pt;
    }
    @element 'h1' and (min‐characters: 16) and (max‐width: 600px) {
      h1 {
        font‐size: 20pt;
      }
    }

### Мета-селекторы

Одна из проблем, с которыми вы можете столкнуться при использовании стилей с
областями определения и адаптивнмы условиями: если на странице есть несколько
элементов с одинаковым селектором, то использование этого селектора в выражении
для элемента приведёт к тому, что стили будут применены *ко всем* элементам
на странице, подпадающих под этот селектор, если *хотя бы один* из них
соотвествует условию. Вернёмся к нашему примеру с `.widget` и предположим, что
на странице есть два виджета (например, один в боковой панели, а другой
отображается на всю ширину), а мы написали выражение для элемента таким образом:

    @element '.widget' and (min‐width: 500px) {
      .widget h2 {
        font‐size: 14pt;
      }
    }

Это будет значить, что когда *любой из* элементов `.widget` на странице шириной
минимум 500 пикселей, то стиль применится к обоим элементам `.widget`. В большей
части случаев, пожалуй. это не то, что нам нужно. Но в этой ситуации нам помогут
мета-селекторы!

Выражение для элемента состоит из двух частей: селектора и условия. И если мы
хотим применять стили только к тем элементам на страницы, которые соответствуют
*и* селектору *и* условию одновременно, то можно использовать мета-селектор
`$this`. Давайте перепишем наш последний пример так, чтобы стили применялись
только к элементам `.widget`, соответствующим условию `min‐width: 500px`:

    @element '.widget' and (min‐width: 500px) {
      $this h2 {
        font‐size: 14pt;
      }
    }

A number of new selectors are supported by the EQCSS syntax that aren’t in
regular CSS. Here is the full list:

*   `$this`  
    [demo][23][32][24] 
*   `$parent`  
    [demo][25][33][26] 
*   `$root`  
    [demo][27][34][28] 
*   `$prev`  
    [demo][29][35][30] 
*   `$next`  
    [demo][31][36][32] 

These selectors will only work in an element query.

#### Opening the Portal Between JavaScript and CSS {#opening-the-portal-between
-javascript-and-css
}

The last and final feature of EQCSS is the wildest of all: `eval('')`. Through
this doorway lies all the power of JavaScript, accessible from CSS. Though 
JavaScript can apply styles to elements, it currently has a hard time reaching 
some things, such as the`:before` and `:after` pseudo-elements. But what if CSS
could reach JavaScript from the other direction? Instead of JavaScript setting 
CSS properties, what if CSS could be aware of JavaScript?

This is where `eval('')` comes in. You can access and evaluate any JavaScript,
whether to use the value of a JavaScript variable in your CSS, to execute a 
JavaScript one-liner (like`eval('new Date().getFullYear()')`), to ascertain
values about the browser and other elements that JavaScript can measure (like
`eval('innerHeight')`) or to run a JavaScript function and use the value it
returns in your CSS styles. Here’s an example that outputs`© 2016` in a 
`<footer>` element:

    @element 'footer' {
      $this:after {
        content: "© eval('new Date().getFullYear()')";
      }
    }

While evaluating JavaScript, `eval('')` also works from the context of the
actively scoped element, the same element to which styles for`$this` apply. You
can use`$it` in JavaScript written in `eval('')` as a placeholder for the
scoped element if it helps you to structure the code, but if omitted, it will 
work the same way. For example, let’s say we have a`div` with the word “hello
” in it. The following code would output “hello hello
”:

    <div>hello</div>
    <style>
      @element 'div' {
        div:before {
          content: "eval('$it.textContent') ";
        }
      }
    </style>

Here, `$it` refers to the `div` because it’s the currently scoped selector.
You could also omit the`$it` and write the following code to display the same
thing:

    @element 'div' {
      div:before {
        content: "eval('textContent') ";
      }
    }

`eval('')` can be helpful in situations where CSS isn’t aware of measurements
or events that happen on the page after it has loaded. For example, iframe 
elements used for embedding YouTube videos come with a specified width and 
height. While you can set the width to`auto` in CSS, there’s no easy way to
maintain the correct aspect ratio of width to height when the video expands to 
fill the available space.

A common workaround for maintaining responsive aspect ratios while scaling is
to place the content that needs to maintain its aspect ratio in a wrapper, and 
then add padding to the wrapper with a value based on the ratio of width to 
height that you want to maintain. This works, but requires you to know the 
aspect ratio of all videos in advance, as well as requires more HTML markup (a 
wrapper element) for each video that you want to embed responsively. Multiply 
that across every website that has responsive videos, and that’s a lot of cruft 
that wouldn’t need to be there if CSS were just a little smarter.

Maybe a better approach to responsive aspect ratios involves placing each video
in a wrapper without padding and then writing a JavaScript library that 
calculates the aspect ratio of each video it finds and applies the correct 
amount of padding to each wrapper.

But what if CSS *could* access those measurements directly? Not only could we
consolidate the CSS for all different aspect ratios into one rule, but if we 
could evaluate it once the page loads, we could write one rule that responsively
resizes any video we ever give it in future. And we could do it all without any 
wrappers!

If this seems too good to be true, check this out. Here’s how simple it is to
write wrapper-free responsively resizing iframe elements in EQCSS:

    @element 'iframe' {
      $this {
        margin: 0 auto;
        width: 100%;
        height: eval('clientWidth/(width/height)');
      }
    }

Here, `width` and `height` refer to the `width=""` and `height=""` attributes
in the iframe in HTML. JavaScript can perform the calculation of
`(width/height)`, which gives us the aspect ratio; and to apply it at any width
, we would just divide the current width of the iframe element by the ratio.

This has the succinctness and legibility of CSS, and all the power of
JavaScript. No extra wrappers required, no extra classes and no extra CSS.

Be careful with `eval('')`, though. [There’s a reason][33][38][34] why CSS
expressions were considered dangerous in the past, and there’s also a reason why
we tried out the idea. If you aren’t careful with how many elements you are 
applying them to on the page or with how frequently you are recalculating styles,
then JavaScript could end up running things hundreds of times more than needed. 
Thankfully, EQCSS allows us to invoke`EQCSS.apply()` or `EQCSS.throttle()` to
recalculate the styles manually, so that we have more control over when styles 
are updated.

#### The Danger Zone! {#the-danger-zone}

Other problems can occur if you create queries with conflicting conditions or
styles. EQCSS, like CSS, is read top to bottom using a
[hierarchy of specificity][35][39][36]. Although CSS is a declarative language
, it does contain some advanced features. It’s only a couple steps away from 
being Turing-complete as a programming language. So far, debugging CSS has been 
a pretty straightforward affair, but EQCSS shifts CSS from being simply an 
interpreted declarative language into being a**dynamic style sheet language**
with an additional layer of interpretation between CSS and the browser. With 
this new territory comes a variety of potential new pitfalls.

Here’s an example of a reciprocal loop in EQCSS, something that normal CSS
media queries are immune to by design:

    @element '.widget' and (min‐width: 300px) {
      $this {
        width: 200px;
      }
    }

I call this `jekyll: hide;` CSS. But in addition to one style continually
triggering itself, there’s also the possibility of writing multiple queries that
trigger each other, in what we call a “double-inverted reciprocal loop,” the 
nastiest of all:

    @element '.widget' and (min‐width: 400px) {
      $this {
        width: 200px;
      }
    }
    @element '.widget' and (max‐width: 300px) {
      $this {
        width: 500px;
      }
    }

In theory, that unfortunate widget would be stuck in a loop, resizing between
200 and 500 pixels until the end of time, unable to settle on a width. For cases
like this, EQCSS simply computes the rules in the order they are evaluated, 
awards the winner and moves on. If you were to rearrange the order in which 
these rules appear, the latter style would always win if they are of equal 
specificity.

Some people say that the ability to create loops (or even double-inverted
reciprocal loops) is a design flaw, but in order to prevent loops from being 
possible, you would need to limit the power of EQCSS in a way that would remove 
most of the value that the syntax provides. On the other hand, creating infinite
loops in JavaScript is very easy, but that’s not viewed as a flaw of the 
language — it’s seen as evidence of its power! It’s the same with element 
queries.

#### Debugging Element Queries {#debugging-element-queries}

Currently, debugging element queries can feel a bit like debugging media
queries before we had tools like the web inspector to show us the styles as they
were computed on the page. For now, debugging and developing element queries 
require the developer to maintain a mental model of what responsive behavior 
should be happening. In the future, building EQCSS-aware developer tools might 
be possible, but as of now, the developer tools in all major browsers are only 
aware of the styles that EQCSS has already applied to the elements on the page, 
and they are not aware of the responsive conditions that EQCSS is watching.

### How To Design With Element Queries {#how-to-design-with-element-queries}

The simplest way to make use of element queries is to convert existing designs
using media queries into element queries, “liberating” elements and their 
responsive styles from one layout and making it easy to reuse that code in other
pages and projects. The following media query and element query could mean the 
same thing:

    footer a {
      display: inline-block;
    }
    
    @media (max‐width: 500px) {
      footer a {
        display: block;
      }
    }

    footer a {
      display: inline-block;
    }
    
    @element 'footer' and (max‐width: 500px) {
      $this a {
        display: block;
      }
    }

The difference is that, in the original example, the footer links stay as 
`display: block` until the *browser* is at least 500 pixels wide. The second
example, using element queries, would look the same but only if the`footer`
element was full width.

After liberating this style from its original media query, we can now place the
footer in a container of any width and be sure that when the footer needs the 
responsive style to be applied (i.e. when it is narrower than 500 pixels), it 
will be applied.

1.  Ensure that `EQCSS.js` is present in the destination document’s HTML.
2.  Replace `@media` with `@element` in the CSS.
3.  Add a CSS selector to the scope of each `@element` query.
4.  Optional: Replace occurrences of the scoped element with `$this` in element
    queries.
   

Unless the design component you are converting was originally designed to
display using the full width of the browser’s viewport, you will likely have to 
tweak the breakpoints after converting to element queries.

### Avoiding Lock-In {#avoiding-lock-in}

The experience of authoring EQCSS styles is similar to the experience of
writing regular CSS: All of your favorite CSS properties and techniques are 
still there, just with some additional features that help them work together in 
new ways. Because EQCSS outputs standard CSS to the browser, any CSS feature 
that your browser has built-in support for will work when used with EQCSS. If 
someday features such as element queries and scoped styles are specified in CSS 
and supported in browsers, then you would be able to begin using them in your 
EQCSS code right away, while still relying on EQCSS for the other features that 
the browser doesn’t yet support natively.

Because you can use EQCSS syntax both directly in CSS as well as in its own
script, with the type`text/eqcss`, if CSS ever develops a syntax for native
element queries, you would still be able to load EQCSS as a script and avoid 
conflicts.

Looking forward, one solution that browser developers are experimenting with
right now is[Houdini][37][40][38], which would open up access for plugin
developers to extend CSS in new ways, like adding support to the browser itself.
Some day, it might be possible to write more efficient plugins that interpret 
EQCSS syntax and bring these features to browsers in a more direct and 
performant way than the current JavaScript library allows.

Yes, even though element queries provide many new and exciting ways to target
elements with styles, media queries (though limited) will always run faster in 
the browser than a style that relies on JavaScript to compute. However, CSS 
media queries are for more than just styling HTML for screens. CSS media queries
also support print-based queries and other ways that websites display 
information. EQCSS can be used in conjunction with media queries for things like
print styles, so there’s no need to retire the media query just because element 
queries can now also be used!

### Designing With 20/20 Vision {#designing-with-20-20-vision}

The best thing we can do for the future of CSS is to experiment with these
ideas as much as possible today. No amount of brainstorming and theorizing about
these features will be as useful as actually trying to implement them and put 
them to use, discovering the techniques that make them useful and powerful.

In addition to providing a solution for element queries, EQCSS.js hopefully
also serves as a platform for other experiments in extending CSS. If you have an
idea for a new responsive condition, a new CSS property or a new selector to use
in your code, forking EQCSS.js and modifying it to include your idea can get you
most of the way there with little effort.

### Modular Design {#modular-design}

In designing layouts using element queries, the biggest shift is learning how
to stop viewing the DOM from the top down and from the perspective of only the 
root HTML element, and to start thinking about individual elements on the page 
from their own perspectives within the document.

The old paradigms of “desktop-first” and “mobile-first” responsive
design aren’t relevant any longer — the new way of building layouts approaches 
design “element-first.” Using element queries enables you to work on the 
individual parts that make up a layout, in isolation from one another, styling 
them to a greater level of detail. If you are using a modular approach for your 
back-end code already but have so far been unable to package your CSS with your 
modules because of the difficulties of styling with media queries alone, then 
element queries will finally allow you to modularize your styles in the same way.

### Thinking Element-First {#thinking-element-first}

Element-first design is in the same spirit as the [atomic design principle][39]
[41][40] but looks different in practice from how most people have implemented
atomic design in the past.

For example, let’s say you have some HTML like the following, and the desired
responsive behavior can be explained as, “The search input and button are 
displayed side by side until the form gets too narrow. Then, both the input and 
the button should be stacked on top of each other and displayed full width.
”

    <form>
      <input type=search>
      <input type=button value=Search>
    </form>

#### Desktop-First Approach {#desktop-first-approach}

In a desktop-first mindset, you would write styles for the desktop layout first
and then add responsive support for smaller screens.

    input {
      width: 50%;
      float: left;
    }
    @media (max‐width: 600px) {
      input {
        width: 100%;
        float: none;
      }
    }

#### Mobile-First Approach {#mobile-first-approach}

In a mobile-first mindset, you would design the mobile view first and add
support for the side-by-side view only when the screen is wide enough.

    input {
      width: 100%;
    }
    @media (min‐width: 600px) {
      input {
        width: 50%;
        float: left;
      }
    }

#### Element-First Approach {#element-first-approach}

In the first two examples, the media breakpoint was set to 600 pixels, and not
because that’s how wide the inputs will be when they switch. Chances are, the 
search input is probably inside at least one parent element that would have 
margins or padding. So, when the browser is 600 pixels wide, those inputs might 
be somewhere around 550 or 525 pixels wide on the page. In a desktop-first or 
mobile-first approach, you’re always setting breakpoints based on the layout and
how elements show up within it. With an element-first layout, you’re saying, “I 
don’t care how wide the browser is. I know that the sweet spot for where I want 
the inputs to stack is somewhere around 530 pixels wide.” Instead of using a 
media query to swap that CSS based on the browser’s dimensions, with element-
first design, we would scope our responsive style to the`form` element itself,
writing a style like this:

    input {
      width: 100%
    }
    @element 'form' and (min‐width: 530px) {
      $this input {
        width: 50%;
        float: left;
      }
    }

The code is similar to that of the two previous methods, but now we are free to
display this search input anywhere — in a sidebar or full width. We can use it 
in any layout on any website, and no matter how wide the browser is, when the 
form itself doesn’t have enough room to display the inputs side by side, it will
adapt to look its best.

### Resources For Getting Started {#resources-for-getting-started}

#### EQCSS-Enabled Template {#eqcss-enabled-template}

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf‐8">
      <title></title>
      <style></style>
    </head>
    <body>
    
      <!‐‐[if lt IE 9]><script src="http://elementqueries.com/EQCSS‐polyfills.min.js"></script><![endif]‐‐>
      <script src="http://elementqueries.com/EQCSS.min.js"></script>
    </body>
    </html>

#### Demos {#demos}

#### Further Reading {#further-reading}

You can find the [EQCSS project on GitHub][41][70][42], [demos][7][71][43],
documentation and articles on the[EQCSS website][44][72][45]. An ever-growing
number of[Codepens use EQCSS][46][73][47], and you can create your own pen by
forking the[batteries-included template][48][74][49] that comes hooked up with
EQCSS. You can also[play with the EQCSS tool][50][75][51] that I built to
preview if your EQCSS code is working as expected.

Happy hacking!

*(vf, il, al)*

[↑ Back to top][52] [Tweet it][53][Share on Facebook][54]</article>


 [1]: https://www.smashingmagazine.com/2013/06/media-queries-are-not-the-answer-element-query-polyfill "Media Queries Are Not The Answer: Element Query Polyfill"

 [3]: img/eqcss-logo-opt.png "Логотип EQCSS"

 [5]: https://github.com/eqcss/eqcss

 [7]: https://github.com/eqcss/eqcss#element-query-demos

 [9]: https://discourse.wicg.io/t/element-queries/26 "WICG Discourse Forum Element Queries Thread"

 [11]: https://github.com/eqcss/eqcss "EQCSS.JS"

 [13]: https://tldrlegal.com/license/mit-license "MIT License at TLDR Legal"

 [15]: http://elementqueries.com/EQCSS.js "EQCSS.js"

 [17]: https://drafts.csswg.org/selectors-4/#relational "Description of Has in CSS Selectors Level 4"

 [19]: https://api.jquery.com/has-selector/ "jQuery Has selector"

 [21]: http://caniuse.com/#feat=css-has "Can I use browser support for Has"

 [23]: http://codepen.io/tomhodgins/pen/xOZGOq "This selector demo"

 [25]: http://codepen.io/tomhodgins/pen/VjeLjy "Parent selector demo"

 [27]: http://codepen.io/tomhodgins/pen/RRrPRy "Root selector demo"

 [29]: http://codepen.io/tomhodgins/pen/gMPpMd "Prev selector demo"

 [31]: http://codepen.io/tomhodgins/pen/PzZqzy "Next selector demo"

 [33]: https://blogs.msdn.microsoft.com/ie/2008/10/16/ending-expressions/ "MSDN Blogs Ending Expressions"

 [35]: https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/ "CSS specificity things you should know"

 [37]: https://www.smashingmagazine.com/2016/03/houdini-maybe-the-most-exciting-development-in-css-youve-never-heard-of/ "Houdini may be the most exciting development in CSS youve never heard of"

 [39]: http://bradfrost.com/blog/post/atomic-web-design/ "Atomic web design"

 [41]: https://github.com/eqcss/eqcss "EQCSS on Github"

 [44]: http://elementqueries.com "element queries dot com"

 [46]: http://codepen.io/search/pens/?q=eqcss&limit=all&order=newest&depth=everything&show_forks=true "Pens on Codepen using EQCSS"

 [48]: http://codepen.io/pen?template=gagyrz "EQCSS batteries included template"

 [50]: http://elementqueries.com/repl.html

 [52]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/

 [53]: https://twitter.com/intent/tweet?original_referer=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/&source=tweetbutton&text=The%20Search%20For%20The%20Holy%20Grail%3A%20How%20I%20Ended%20Up%20With%20Element%20Queries%2C%20And%20How%20You%20Can%20Use%20Them%20Today&url=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/&via=smashingmag

 [54]: http://www.facebook.com/sharer/sharer.php?u=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/

