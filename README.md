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

#### Fonts

Конвертация шрифтов OTF->TTF, TTF->WOFF, WOFF->WOFF2 с копией в `src/fonts`.
Если, только WOFF/Woff2 - копируются в `build`

Все шрифты автоматически собираются в "src\scss\base\_fontsAutoGen.scss".
При этом, важно! Название - плотность - курсив разделять пробелами

- `Montserrat-Bold-Italic.ttf`
- `Inter-Regular.ttf`

При подключении только внешних шрифтов, `src/fonts` оставить пустым.

Либо, положить в папку `src/files/fonts`. Вообще, все любые дополнительные файлы (pdf, dbf...) можно ложить в `src/files/` и они попадут в `build/files/`

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
