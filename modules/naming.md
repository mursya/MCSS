---
layout: default
title: Именование

context: inner-page
---

Правила именования, хоть и являются модулем документации MCSS, в обобщенном понятии применимы не только к названиям CSS классов, но так же и совершенно любым другим названиям элементов/файлов/сущностей. Исключая специфику отдельных правил именования, советуется придерживаться общих правил именования в рамках всего проекта.

## Абстрактность

Все используемые названия всегда должны быть максимально абстрактными. Нет ничего постоянного, все рано или поздно меняется, обновляется, развивается, а так же мнимо временные варианты часто превращаются в постоянные.

Что бы название оставалось абстрактным — оно должно быть максимально **отделено от представления**.

### Представление

Названия ни в коем случае не должны включать в себя привязанность к внешним признакам отображения, например:

{% highlight css %}
.color-red { color: red } /* цвет ошибки */
{% endhighlight %}

Цвет ошибки рано или поздно может поменяться, и рано или поздно мы столкнемся с такой ситуацией:

{% highlight css %}
.color-red { color: green } /* цвет ошибки */
{% endhighlight %}

С одной стороны проблема не столь велика, но требует дополнительного рефакторинга, который уже как минимум занимает лишнее время и как максимум может вызвать большие сложности и затянуть процесс обновления.

Правила отделения названий от представление покрывает не только привязку к цветам, оно относится практически ко всем свойствам CSS и аналогам в других конктекстах. Все перечисленные ниже названия неприемлемы:

{% highlight css %}
.padding-20 {}
.overflow {}
{% endhighlight %}

Заменять такие название нужно на логическое описание роли элемента. Это может быть сложно, но лучше один раз к этому приучится, и инвестировать немного своего времени, чем потом заморачиваться с поддержкой. В помощь по выбору названия пригодятся следующие разделы.

#### Размеры

С абстрактным описанием размеров всегда возникают сложности, что бы их избежать, советуется использовать правила размеров одежды:

	font-size:
		10px -> S, .font-S {}
		12px -> M, .font-M {}
		14px -> L, .font-L {}

Используя такое правило именования можно легко создавать абстрактные название любым сущностям связанным с размерами, и легко их дополнять с помощью промежуточных значений:

	XS
	XM
	XL
	XXL и пр.

#### Cтили

Если название не привязано к размерам и не возможно придумать логичное значение, но тем не менее разные сущности отличаются между собой только внешним представлением, можно использовать название стилей по порядковому номеру, например:

	.style-red    -> .style-1
	.style-big    -> .style-2
	.style-chaos  -> .style-3

## Ключевое слово

Правило ключевого слова (keyword) применимо ко всем контекстам где важно наглядно определить логическую связь элементов. Если в файловой системе это правило в большинстве случаев не актуально, то в HTML может очень пригодится.

### В CSS

По MCSS (из основ АНБ), использование каскада в CSS ограничено ради поддержания независимости блоков и связанности с другими их элементами. Поэтому, самое основное правило именования заключаются в необходимости использовать название модуля (оно же ключевое слово) в начале каждого класса этого модуля:

{% highlight css %}
.module-name {}
.module-name_child {}
.module-name_child__modifier {}
{% endhighlight %}

Что бы не раздувать названия классов, советуется использовать [cловарь сокращений]({{ site.baseurl }}/modules/shorthand_dictionary.html).

### Логические части и вложенность названий

Что бы определить логическую связь между элементами, в названии используется вложенность логических частей. Логическая часть представляет собой название логического сущности, которой может выступать любой HTML блок, или категория каталога.

Название не обязательно должно тянуть всю цепочку вложенности из логических частей, цепочки в названиях используются исключительно для определения логической связи элементов.

Пример в HTML/CSS:

{% highlight html %}
<div class="module-name">
    <div class="module-name_child">
        <div class="module-name_child_cnt">
            <div class="module-name_child_t">
        </div>
    </div>
</div>
{% endhighlight %}

{% highlight css %}
.module-name {}
.module-name_child {}
.module-name_child_cnt {} /* child content wrapper */
.module-name_child_t {} /* child title */
{% endhighlight %}

В примере видно, что класс **.module-name\_child_t** не унаследовал цепочку названия от **.module-name\_child_cnt**, потому, что этот элемент является просто оберткой, которая не несет логики с точки зрения содержания, а используется только для декорации.

Цепочку вложенности названия можно сокращать до тех пор, пока не будет нарушена логическая связь между элементами. Стоит отметить, что в реальной жизни, зачастую можно ограничиваться 4-5 элементами в названии. Если в какой то ситуации возникла необходимость сделать цепочку длинней 4-5 слов, стоит *остановится и задуматься* — насколько необходима такая сложность, и может стоит выделить вложенные блоки используемого модуля в отдельный модуль.

#### Обертки

Немного про специфику именование оберток — советуется их выносить не на более высокий уровень, а вкладывать в основной модуль. Это позволит более наглядно разделять блоки, проще читать код и поспособствует более простому рефакторингу, ибо основная завязка логики чаще будет ложится на корневой блок, а не на его обертку, которая может в любой момент исчезнуть.

Вместо такого подхода:

	.block_w - обертка
		.block

делаем вот так:

	.block
		.block_cnt - обертка

## Разделители

В названиях используются следующие разделители:

	"-" - разделитель слов
	"_" - разделитель логического части
	"__" - разделитель модификатора

Разделитель слов используется вместо пробела в названии логических частей состоящий из нескольких слов — «длинное**-**название».

По поводу использования разделителей логичных частей понятно из раздела про «ключевое слово». По использованию модификаторов чуть больше правил, поэтому раздел про них вынесен в [отдельный модуль]({{ site.baseurl }}/modules/modifiers.html).

Разделители вполне заменяемы на другие, например в BEM разделитель логичной части "\_\_" а модификатор "_". Главное придерживаться одного подхода, в котором определены несколько типов разделителей.