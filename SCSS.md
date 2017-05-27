SCSS (SASS) — препроцессор для более эффективного написания CSS-кода.

## Основные используемые возможности

### Каскад

Так выглядит код в SCSS

```scss
.block {
  width: 100px;
  height: 100px;
  
  .element {
    color: white;
  }
}
```

После компиляции он превратится в:

```scss
.block {
  width: 100px;
  height: 100px;
}

.block .element { color: white; }
```

***

### Ссылка на родителя селектора

Символ `&` после компиляции заменяется на class/id родителя селектора, свойства которого мы редактируем:

```scss
.block {
  color: white;

  &.active {
    color: black;
  }
}
```

Скомпилируется в:

```scss
.block { color: white; }
.block.active { color: black; }
```

***

### Использование SCSS с [БЭМ](https://github.com/verstaburo/versta-standarts/wiki/%D0%91%D0%AD%D0%9C)

Пример:

```scss
.block {
  color: black;

  &__text {
    font-size: 14px;

    &_color_red { color: red; }
  }
}
```

Скомпилируется в:

```scss
.block { color: black; }

.block__text { font-size: 14px; }

.block__text_color_red { color: red; }
```

***

### Шаблонные селекторы

Если какая-то структура стилей часто используется без изменений в разных блоках, можно сохранить ее в так называемом "шаблонном селекторе", начав название селектора с символа `%`, и подгружать в нужные селекторы через директиву `@extend`:

```scss
%alert-text {
  color: red;
  font-weight: bold;
}

.warning-one {
  @extend %alert-text;
  border: 2px solid red;
}

.warning-two {
  @extend %alert-text;
  background-color: black;
}
```

Скомпилируется в:

```scss
.warning-one {
  color: red;
  font-weight: bold;
  border: 2px solid red;
}

.warning-one {
  color: red;
  font-weight: bold;
  background-color: black;
}
```

***

### Комментирование

Два вида комментариев в SCSS:

```scss
// Этот однострочный комментарий будет виден только в SCSS-файле, т.к. комментирование через '//' не поддерживается CSS

/* Этот многострочный комментарий
останется в финальном CSS файле,
т.е. такой вид комментирования
поддерживается CSS */
```

***

### Переменные

Через символ `$` можно задавать переменные, чтобы использовать одинаковые значения в разных местах проекта:

```scss
$color-red: #ff0000;

.block { color: $color-red; }
```

***

### RGBA

Стандартно задание цвета через `rgba(red, green, blue, alpha)` довольно громоздкая задача, т.к. предварительно надо получить значения `red, green, blue`, а цвета обычно задаются в `hex`.

SCSS упрощает эту задачу и позволяет подставить вместо трех каналов цвет в `hex` или переменной:

```scss
.block { color: rgba(#ff0000, .6); }

$color-black: #000;

.black-block { color: rgba($color-black, .3); }
```

***

### Операции

SCSS поддерживает математические операции, позволяет выводить результат операций сразу в нужное свойство либо в переменную.

```scss
// Делаем простую сетку на SCSS

// Базовые переменные
$grid-columns: 12;
$container-width: 960px;

// Находим ширину одной колонки
$col-width: $container-width / $grid-columns;

// Высчитываем ширину различных колонок для двенадцатиколоночной сетки
.col_one { width: $col-width * 1; }
.col_two { width: $col-width * 2; }
.col_three { width: $col-width * 3; }
.col_four { width: $col-width * 4; }
.col_five { width: $col-width * 5; }
.col_six { width: $col-width * 6; }
.col_seven { width: $col-width * 7; }
.col_eight { width: $col-width * 8; }
.col_nine { width: $col-width * 9; }
.col_ten { width: $col-width * 10; }
.col_eleven { width: $col-width * 11; }
.col_twelve { width: $col-width * 12; }
```

***

### Подключение вспомогательных SCSS файлов в главный

Основной файл стилей можно для удобства разбить на несколько, которые будут подгружаться в основной.
Делается это через директиву `@import`:

```scss
// Этот код подгрузит файл foo.scss или _foo.scss
@import "foo";
```

Чтобы вспомогательные файлы не компилировались, необходимо перед их названием поставить символ `_`. Например, `_adaptive.scss`

***

### Миксины

Миксины — некие конструкции свойств и селекторов, которые могут подгружаться в нужные нам селекторы с помощью директивы `@include`;

Миксины могут не иметь аргументов:

```scss
@mixin clearfix {
  &:after {
    content: "";
    display: table;
    clear: both;
  }
}

.parent {
  color: white;

  @include clearfix;
}

// Скомпилируется в:
.parent { color: white; }

.parent:after {
  content: "";
  display: table;
  clear: both;
}
```

Миксины могут иметь аргументы:

```scss
@mixin sexy-border($color, $width) {
  border-color: $color;
  border-width: $width;
  border-style: dotted;
}

.block {
  @include sexy-border(red, 5px);
}

// Скомпилируется в:
.block {
  border-color: red;
  border-width: 5px;
  border-style: dotted;
}
```

Миксины могут позволять вставлять внутрь себя конструкции при использовании `@content`:

```scss
@mixin placeholder {
  &::placeholder {
    @content;
  }
}

.block {
  @include placeholder {
    color: red;
  }
}

// Скомпилируется в:
.block::placeholder { color: red; }
```