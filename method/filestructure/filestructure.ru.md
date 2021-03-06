# Организация файловой структуры

Принятый в методологии БЭМ компонентный подход применяется и к организации БЭМ-проектов в файловой структуре. В БЭМ не только интерфейс делится на независимые компоненты — блоки, но и реализация блоков делится на независимые части — файлы.

На этой странице вы найдете:

* [Принципы организации файловой структуры](#Принципы-организации-файловой-структуры-БЭМ-проекта)
* [Примеры схем файловых структур БЭМ-проекта](#Примеры-схем-файловых-структур-БЭМ-проекта)
* [Примеры использования уровней переопределения](#Примеры-использования-уровней-переопределения)

## Принципы организации файловой структуры БЭМ-проекта

В БЭМ-проекте код разделяется на мелкие независимые части для удобства работы с отдельными блоками. Перед отправкой в браузер файлы собираются и оптимизируются. Таким образом мы разграничиваем код, с которым работают люди, и код, который передается в браузер.

В файловой структуре код БЭМ-проекта организован по следующим принципам:

* [Реализация блока разделяется на отдельные файлы](#Реализация-блока-разделяется-на-отдельные-файлы)
* [Опциональные элементы и модификаторы выносятся в отдельные файлы](#Опциональные-элементы-и-модификаторы-выносятся-в-отдельные-файлы)
* [Файлы объединяются по смыслу, а не по типу](#Файлы-объединяются-по-смыслу-а-не-по-типу)
* [Проект разделяется на уровни переопределения](#Проект-разделяется-на-уровни-переопределения)

### Реализация блока разделяется на отдельные файлы

Набор файлов блока (например, `input.css`, `input.js`) зависит от [технологий](../key-concepts/key-concepts.ru.md#Технология-реализации), входящих в реализацию блока.

*Зачем?*

* **Улучшение навигации по проекту**

Структура проекта построена по единому принципу, а имена блоков уникальны. Это позволяет разработчикам быстро ориентироваться в разных частях проекта и находить нужные файлы.

* **Упрощение переноса блоков между проектами**

Реализация блоков разделена по отдельным файлам. При переносе блока из проекта в проект достаточно скопировать нужные файлы или директории.

### Опциональные элементы и модификаторы выносятся в отдельные файлы

*Зачем?*

* **Подключение только нужной реализации блока**

В сборку подключаются только файлы, действительно необходимые для данной реализации блока.

### Файлы объединяются по смыслу, а не по типу

Файлы блока группируются с помощью общих [правил именования](../naming-convention/naming-convention.ru.md). Для удобства работы они могут быть объединены в директорию этого блока.

*Зачем?*

* **Подключение в проект только необходимых блоков**

Блоки реализованы независимо. Это позволяет настраивать сборку так, чтобы в проект попадали только нужные блоки.

### Проект разделяется на уровни переопределения

Конечная реализация блока может быть разделена по [уровням переопределения](#Примеры-использования-уровней-переопределения).

*Зачем?*

* **Отсутствие дублирования кода**

  Вынесение общей реализации для всех платформ на отдельный уровень позволяет избежать дублирования кода и сократить время на отладку проекта.

* **Переопределение и доопределение блоков готовых библиотек**

  Изменение блока библиотеки не требует его копирования на уровень проекта. Достаточно создать требуемый файл с правками или новым кодом на другом уровне переопределения и подключить в сборку.

* **Обновление подключенных в проект библиотек**

  Разделение проекта на уровни позволяет изменять блоки, не затрагивая код библиотеки. При обновлении библиотеки модификация блоков сохраняется на другом уровне проекта.

## Примеры схем файловых структур БЭМ-проекта

### Nested

* Блоку соответствует директория в файловой структуре.
* Имена блока и его директории совпадают.

```files
blocks/
    input/     # Директория блока input
    button/    # Директория блока button
```

* Реализация блока разделена на отдельные файлы — файлы технологий.
* Имена файлов совпадают с именем блока.
* Расширение соответствует технологии.

```files
blocks/
    input/
        input.css       # Реализация блока `input` в технологии CSS
        input.js        # Реализация блока `input` в технологии JavaScript
    button/
        button.css
        button.js
        button.png
```

Имена файлов и директорий [БЭМ-сущностей](../key-concepts/key-concepts.ru.md#БЭМ-сущность) соответствуют [соглашению по именованию](../naming-convention/naming-convention.ru.md):

* Элемент — `block__elem.extension` (`input__box.css`).
* Модификатор блока — `block_mod_val.extension` (`input_type_search.css`) или `block_mod.extension` (`input_disabled.css`). Значение булевого модификатора не указывается.
* Модификатор элемента — `block__elem_mod_val.extension` (`input__clear_size_large.css`) или `block__elem_mod.extension` (`input__clear_visible.css`).


Модификаторы и элементы выделяются в отдельные файлы и группируются в поддиректории блока с соответствующими именами.

```files
blocks/
    input/
        _type/                                # Директория модификатора `type`
            input_type_search.css             # Реализация модификатора `type`
                                              # со значением `search` в технологии CSS
        __box/                                # Директория элемента `box`
            input__box.css
        __clear/                              # Директория элемента `clear`
            _visible/                         # Директория модификатора `visible`
                input__clear_visible.css      # Реализация булевого модификатора `visible`
                                              # со значением `true` в технологии CSS
            _size/                            # Директория модификатора `size`
                input__clear_size_large.css   # Реализация модификатора `size`
                                              # со значением `large` в технологии CSS
            input__clear.css
            input__clear.js
        input.css
        input.js
    button/
        button.css
        button.js
        button.png
```

При создании модификаторов с разными значениями (например, `popup_target_anchor.extension` и `popup_target_position.extension`), общий код может быть вынесен в отдельный файл (`popup_target.extension`) без указания значения модификатора в имени.

```files
blocks/
    popup/
        _target/
            popup_target.css           # Общий код модификатора `target`
            popup_target_anchor.css    # Модификатор `target` в значении `anchor`
            popup_target_position.css  # Модификатор `target` в значении `position`
        _visible/
            popup_visible.css          # Булевый модификатор visible
    popup.css
    popup.js
```

#### Примеры из жизни

* [bem-core](https://github.com/bem/bem-core/tree/v2/common.blocks/page)
* [bem-components](https://github.com/bem/bem-components/tree/v2/common.blocks/button)

### Flat

* Директории для блоков не используются.
* Опциональные элеметы и модификаторы реализованы в отдельных файлах.

```files
blocks/
    input_type_search.js
    input_type_search.bemhtml.js
    input__box.bemhtml.js
    input.css
    input.js
    input.bemhtml.js
    button.css
    button.js
    button.bemhtml.js
    button.png
```

### Flex

Flex схема весьма гибкая по отношению к организации файловой структуры БЭМ-проекта:

* Блоку соответствует отдельная директория.
* Элементы и модификаторы реализованы в отдельных файлах.

```files
blocks/
    input/
        input_layout_horiz.css
        input_layout_vertical.css
        input__elem.css
        input.css
        input.js
    button/
```

* Блоку соответствует отдельная директория.
* Элементы и модификаторы реализованы в файлах блока.

```files
blocks/
    input/
        input.css
        input.js
    button/
```
* Директории для блоков не используются.
* Элементы и модификаторы реализованы в файлах блока.

```files
blocks/
    input.css
    input.js
    button.css
    button.js
```

* Директории для блоков не используются.
* Опциональные элеметы и модификаторы реализованы в отдельных файлах.

```files
blocks/
    input_type_search.js
    input_type_search.bemhtml.js
    input__box.bemhtml.js
    input.css
    input.js
    input.bemhtml.js
    button.css
    button.js
    button.bemhtml.js
    button.png
```

Модификаторы и элементы выделяются в отдельные файлы и группируются в поддиректории блока с соответствующими именами.

```files
blocks/
    input/
        _type/                                 # Директория модификатора `type`
            input_type_search.css              # Реализация модификатора `type`
                                               # со значением `search` в технологии CSS
        __box/                                 # Директория элемента `box`
            input__box.css
        input.css
        input.js
    button/
        button.css
        button.js
        button.png
```

## Примеры использования уровней переопределения

Реализация блока может быть разделена по [уровням переопределения](../key-concepts/key-concepts.ru.md#Уровень-переопределения).

Рассмотрим несколько примеров:

* [Подключение библиотеки](#Подключение-библиотеки)
* [Разделение проекта на платформы](#Разделение-проекта-на-платформы)

### Подключение библиотеки

В проект можно подключить библиотеку как отдельный уровень. Изменять (до- или переопределять) блоки можно на другом уровне проекта. При сборке подключится исходная реализация блоков с уровня библиотеки и переопределенная — с уровня проекта.

Такое разделение позволяет сохранить изменения в блоках при обновлении библиотеки. Код самой библиотеки обновится, а специфическая реализация блоков проекта останется прежней, так как находится на другом уровне проекта.

```files
library.blocks/
    button/
        button.css    # CSS-реализация кнопки в библиотеке (высота 20px)

project.blocks/
    button/
        button.css    # Переопределение на уровне проекта (высота 24px)
```

### Разделение проекта на платформы

Проект разделен на платформы (`mobile` и `desktop`) и соответствующие уровни переопределения в файловой структуре. Уровень `common` содержит общую реализацию блоков для всех платформ. На уровне `desktop` и `mobile` находится специфика реализаций блоков, характерная для каждой из платформ.

Рассмотрим на примере:

```files
common.blocks/
    button/
        button.css    # Базовая CSS-реализация кнопки

desktop.blocks/
    button/
        button.css    # Особенности кнопки для desktop

mobile.blocks/
    button/
        button.css    # Особенности кнопки для mobile
```

При сборке в файл `desktop.css` попадут все базовые CSS-правила кнопки с уровня `common` и переопределенные правила с уровня `desktop`.

```css
@import "common.blocks/button/button.css";    /* Базовые CSS-правила */
@import "desktop.blocks/button/button.css";   /* Особенности desktop */
```

Файл `mobile.css` будет включать базовые CSS-правила кнопки с уровня `common` и переопределенные правила с уровня `mobile`.

```css
@import "common.blocks/button/button.css";    /* Базовые CSS-правила */
@import "mobile.blocks/button/button.css";    /* Особенности mobile */
```
