# TipTip

## В общем

Дополнение создаёт нестандартную всплывающую подсказку, заменяющую стандартную браузерную. Оно достаточно умно, чтобы следить за краями экрана и стараться отобразить подсказку полностью. Его отображение полностью настраиваемо при помощи стилей, а поведение может настраиваться как из разметки (атрибутом `data-tip-tip`), так и из скрипта.

Это неоднократно модифицированная версия дополнения, об истории изменений и возможностях можно почитать в [англоязычной документации](https://github.com/Ser-Gen/TipTip/blob/master/README_en.md) и в [описании релизов](https://github.com/Ser-Gen/TipTip/releases).


## Особенности использования

### Глобальные настройки

Тип можно настроить один раз, доопределяя нужное.
Вместо

```js
$('a').tipTip({
	theme: 'white',
	defaultPosition: 'bottom',
	preserveDirection: true,
	content: 'Типа содержимое'
});

$('b').tipTip({
	theme: 'white',
	defaultPosition: 'bottom',
	preserveDirection: true,
	content: 'Ещё Тип какой-то'
});
```

Пишите

```js
$.fn.tipTip.defaults = {
	theme: 'white',
	defaultPosition: 'bottom',
	preserveDirection: true
};

$('a').tipTip({
	content: 'Типа содержимое'
});

$('b').tipTip({
	content: 'Ещё Тип какой-то'
});
```


### Содержимое

Содержимое Типа передаётся несколькими способами:

* cвойством `content` при инициализации Типа: `.tipTip({ content: "Text" })`, в атрибуте родителя: `data-tip-tip='{"content": "Text"}'`, в глобальных настройках;
* содержимым элемента `TipTip__donor` изнутри родителя.

Чтобы сменить содержимое проинициализированного Типа, используйте метод `content`:

```js

// так содержимое Типа изменится вне зависимости от его активности
$('a').tipTip('content', 'New content');

// если Тип уже активен, можно получить его содержимое (content)
// и элемент, на котором он вызван (target)
$('b').tipTip('content', function (content, target) {

    // изменяем содержимое Типа
    content.html('New content');

    // пересчитываем положение Типа под новое содержимое
    target.tipTip('position');
});
```


### Родитель

Получить элемент, к которому применён Тип, можно двумя способами:

* При активации Типа родителю добавляется класс `TipTip__active`. При этом, если будет разрешено создание множества Типов (`hideOthers: false`), то элементов с таким классом может быть несколько.
* У Типа в данных: `tip.data().tipTip.obj`. Активному Типу добавляется класс `TipTip--is-active`.


### Одноразовый Тип

```js
...,
afterEnter: function () {
	$('body').one('click', function () {
		$('a').tipTip('hide').tipTip('destroy');
	});
},
...
```


### Подстраивание

Тип автоматически стремится оставаться на экране из-за параметра `shiftByViewport` [`true`].
Это поведение можно настроить:

* `preserveDirection` [`false`] — будет ли сохраняться направление, указанное в `defaultPosition` при автоматическом подстраивании.
* `preservePosition` [`false`] — сохранится ли положение выпадения или Тип будет вдвигаться в область видимости (не меняя направление).


### Дополнительно

* Чтобы проверить, привязан ли Тип к элементу, можно поискать данные Типа: `!!$('a').data('tipTip')`.
* Если Тип предполагается использовать в обособленном контейнере с возможными прокруткой и игнорированием переполнения (`overflow: hidden`), нужно указать селектор контейнера в свойстве `container`.
* При отображении Типа все остальные открытые Типы скрываются. Отключить скрытие остальных Типов можно настройкой `hideOthers`.
* Тип можно закрыть изнутри элементом `.TipTip__close`.
* Возможно настроить атрибутом: параметры можно передавать в атрибут `data-tip-tip` в виде правильного `JSON`.
