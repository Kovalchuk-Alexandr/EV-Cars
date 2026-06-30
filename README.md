# HTML CSS Вёрстка сайта EV Cars

Вёрстка лендинга, адаптив. HTML, CSS

***Макет Figma***: [https://www.figma.com/design/AMBa6eLIYQowg3CbLpNCec/](https://www.figma.com/design/AMBa6eLIYQowg3CbLpNCec/)

[See Demo](https://kovalchuk-alexandr.github.io/EV-Cars/)

## Технологии

- HTML,
- SCSS,
- JS,
- Мобильная адаптация
- Дизайн-система (UI Kit)
- Компонентный подход

##### Установить зависимости:

```bash
npm i
```

#### Fonts

Конвертация шрифтов OTF->TTF, TTF->WOFF, WOFF->WOFF2 с копией в `src/fonts`.
Если, только WOFF/Woff2 - копируются в `build`

Все шрифты автоматически собираются в "src\scss\base\_fontsAutoGen.scss".
При этом, важно! Название - плотность - курсив разделять пробелами

- `Montserrat-Bold-Italic.ttf`
- `Inter-Regular.ttf`

При подключении только внешних шрифтов, `src/fonts` оставить пустым.

Либо, положить в папку `src/files/fonts`. Вообще, все любые дополнительные файлы (pdf, dbf...) можно ложить в `src/files/` и они попадут в `build/files/`

##### Images

Обязательно добавить `{ encoding: false }` в путь источника, т.к. начиная с GULP 5.0 изображение по-умолчанию передается текстовой строкой
`.pipe(src(path.source.img, { encoding: false }))`

[`gulp-picture-html`](https://github.com/WpWebr/gulp-picture-html) - расширение для Gulp, создает для html `<img>` стэк `<picture>`

###### Например

```html
// Изменяет тэг 'img'
  <img src="img/image.jpg" alt="image">
```

```html
// на 'picture'
<picture>
    <source srcset="img/image.avif" type="image/avif">
    <source srcset="img/image.webp" type="image/webp">
    <img src="img/image.jpg" alt="image">
</picture>
```

##### Svg Sprite

Собирает svg в './docs/img/svgsprite'
`gulp sprite` - в 'sprite.symbol.svg'
`gulp stack` - в 'sprite.stack.svg'

[GitHub сборка:](https://github.com/Kovalchuk-Alexandr/Gulp-v04-2025.git)

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
