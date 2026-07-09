# HTML CSS Вёрстка сайта EV Cars

Вёрстка лендинга, адаптив. HTML, CSS, JS

***Макет Figma***: [https://www.figma.com/design/AMBa6eLIYQowg3CbLpNCec/](https://www.figma.com/design/AMBa6eLIYQowg3CbLpNCec/)

[See Demo](https://kovalchuk-alexandr.github.io/EV-Cars/)

## Технологии

- HTML,
- SCSS,
- JS,
- Мобильная адаптация
- Компонентный подход
- SVG-sprite

##### Если глифы фонта разной высоты, делаем одной:

```CSS
.detail__value {
    font-variant-numeric: lining-nums tabular-nums;
}
```

##### Обводка текста:

```CSS
.stroked-text {
  /* Толщина и цвет обводки */
  -webkit-text-stroke: 2px #000000;
  /* Цвет внутри букв (transparent делает текст "контурным") */
  -webkit-text-fill-color: transparent;
  /* Стандартные свойства шрифта */
  font-size: 48px;
  font-weight: bold;
}
```

[-webkit-text-stroke CSS property (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/-webkit-text-stroke)

Для создания идеального контура можно комбинировать эту технику с тенями через `text-shadow`

#### sprite.svg - создание

1. создаем файл `touch "./img/svgsprite/sprite.svg"`
2. в файле `sprite.svg` создаем шапку

```HTML
<?xml version="1.0" encoding="utf-8"?>
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <symbol id="arrow-right-top" width="14" height="14" viewBox="0 0 14 14" fill="none">
        <path d="M1.21221 0H13.6977V12.4854H12.297V2.39516L0.994515 13.6977L0 12.7037L11.3031 1.40065H1.21221V0Z" fill="black" />
    </symbol>

    <symbol id="btn-play" width="64" height="64" viewBox="0 0 64 64" fill="none">
        <path d="M0 0H64V64H0V0Z" fill="#E9FF61" />
        <path d="M17 14L47 32L17 50V14Z" fill="black" />
    </symbol>
</svg>
```

3. Если не в отдельном файле, а в самой разметке `HTML` то добавляем стиль `style="display: none;"`

```html
<svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
    ...
</svg>
```

4. в самой иконке тэг `<svg>` заменяем на `<symbol>`
5. Использование:

```html
<svg class="icon--arrow-right-top">
    <use href="./img/svgsprite/sprite.svg#arrow-right-top"></use>
</svg>
```

### Наложение цвета поверх фонового изображения

Для наложения цвета поверх фонового изображения в CSS используют два основных способа: комбинирование слоев с помощью `background` или применение `background-blend-mode`. Это позволяет добиться эффекта тонирования или создания полупрозрачных фильтров для улучшения читаемости текста

1. Объявление цвета и картинки через запятую

Самый кроссбраузерный способ наложения, при котором цвет указывается первым слоем, а фоновое изображение — вторым
  
```CSS
.element {
  background: 
    linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), 
    url('image.jpg');
  background-size: cover;
}
```

Здесь `linear-gradient` используется для создания цветного фильтра с нужной прозрачностью поверх изображения.

2. Использование `background-blend-mode`

Если нужны более сложные эффекты наложения (осветление, затемнение, умножение) между несколькими фонами, применяется следующее свойство

```CSS
.element {
  background-color: #ff0000; /* Красный цвет фона */
  background-image: url('image.jpg');
  background-blend-mode: multiply; /* Режим наложения */
  background-size: cover;
}
```

### Выравнивание размеров карточек card-review

Два подхода — зависит от приоритета:

1. Фиксированная высота + клэмп текста — самый чистый вариант для слайдера

```CSS
.card-review {
    // добавить:
    height: 280px; // подбераем под свой контент
}

.card-review__text {
    // добавить:
    display: -webkit-box;
    -webkit-line-clamp: 6; // кол-во строк
    line-clamp: 6;
    -webkit-box-orient: vertical;
    overflow: hidden;
    flex: 1; // чтобы текст занимал всё свободное место, а автор прижался вниз
}
```

`flex: 1` на тексте + `flex-direction: column` на карточке — автор всегда внизу, текст обрезается при необходимости

2. Без фиксированной высоты — выравнивание через Swiper

Если слайды в одном ряду, можно просто добавить в стили Swiper-обёртки:

```CSS
.swiper-wrapper {
    align-items: stretch; // все слайды растягиваются по высоте самого высокого
}

.swiper-slide {
    height: auto;
}

.card-review {
    height: 100%; // карточка заполняет слайд
}
```

Тогда все карточки одной высоты без клэмпа — текст не обрезается, просто выравниваются по самой длинной.
