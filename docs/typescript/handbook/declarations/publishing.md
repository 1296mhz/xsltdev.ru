---
layout: default
title: Публикация
nav_order: 6
grand_parent: Справочник Typescript
parent: Декларации Typescript
---

<!-- prettier-ignore-start -->
# Публикация
{: .no_toc }
<!-- prettier-ignore-end -->

<!-- prettier-ignore -->
1. TOC
{:toc}

Теперь, когда вы, следуя шагам из этого руководства, создали файл объявлений, пришло время опубликовать его в npm.
Есть два основных пути для этого:

1. включить объявления в npm-пакет
2. опубликовать в [организации @types](https://www.npmjs.com/~types) в npm.

Если вы управляете пакетом, для которого собираетесь опубликовать объявления, то первый способ подойдет лучше всего.
В этом случае объявления и JavaScript-код всегда будут вместе.

## Включение объявлений в свой npm-пакет

Если у пакета есть главный `.js`-файл, то в `package.json` нужно также указать главный файл с объявлениями.
Для этого установите свойство `types`, которое должно указывать на файл объявлений.
Например:

```json
{
  "name": "awesome",
  "author": "Vandelay Industries",
  "version": "1.0.0",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

Стоит отметить, что поле `"typings"` синонимично `"types"` и также может быть использовано.

Также отметим, что если главный файл объявлений имеет имя `index.d.ts` и находится в корне директории пакета (рядом с `index.js`), то указывать свойство `"types"` не обязательно, хотя и желательно.

### Зависимости

Все зависимости управляются npm.
Убедитесь, что все пакеты с объявлениями, от которых зависит ваш пакет, указаны в файле `package.json` в секции `"dependencies"`.
Например, представим, что мы создали пакет, который использует Browserify и TypeScript.

```json
{
  "name": "browserify-typescript-extension",
  "author": "Vandelay Industries",
  "version": "1.0.0",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts",
  "dependencies": ["browserify@latest", "@types/browserify@latest", "typescript@next"]
}
```

Здесь указывается, что наш пакет зависит от пакетов `browserify` и `typescript`.
Пакет `browserify` из npm не поставляется со своими файлами объявлений, поэтому для их добавления нужно указать зависимость от `@types/browserify`.
`typescript`, напротив, поставляется с файлами объявлений, поэтому никаких дополнительных зависимостей не требуется.

Наш пакет представляет доступ к объявлениям из обоих этих пакетов, и поэтому у пользователя нашего пакета `browserify-typescript-extension` также должны быть установлены эти зависимости.
Поэтому используется `"dependencies"`, а не `"devDependencies"`, поскольку во втором случае нашим пользователям пришлось бы устанавливать зависимости вручную.
Если бы мы писали приложение, работающее из командной строки, и этот пакет не предполагалось бы использовать как библиотеку, то можно было бы использовать `devDependencies`.

### Опасные места

#### `/// <reference path="..." />`

_Не_ используйте `/// <reference path="..." />` в файлах объявлений.

```ts
/// <reference path="../typescript/lib/typescriptServices.d.ts" />
....
```

Вместо этого _используйте_ `/// <reference types="..." />`.

```ts
/// <reference types="typescript" />
....
```

Обязательно еще раз прочтите [Использование зависимостей](./Library Structures.html#Использование-зависимостей) для большей информации.

#### Упаковка зависимых объявлений

Если объявления типов зависят от других пакетов:

- _Не объединяйте_ их со своими, храните каждый в своем файле.
- _Не копируйте_ объявления в свой пакет.
- _Добавьте зависимость_ от npm-пакета с объявлениями для нужного пакета, если они не поставляются вместе с ним.

### Публикация файлов объявлений

После публикации своего пакета с файлами объявлений добавьте ссылку на него в [список внешних пакетов репозитория DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/types-2.0/notNeededPackages.json).
Это даст инструментам поиска понять, что ваш пакет предоставляет свои собственные объявления.

## Публикация в [@types](https://www.npmjs.com/~types)

Пакеты организации [@types](https://www.npmjs.com/~types) автоматически публикуются из репозитория [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) с помощью [инструмента types-publisher](https://github.com/Microsoft/types-publisher).
Чтобы опубликовать свои объявления как пакет в организации @types, отправьте запрос на включение изменений (pull request) в репозиторий [https://github.com/DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped).
Больше информации можно найти на странице [руководства по участию](http://definitelytyped.org/guides/contributing.html).

## Ссылки

- [Публикация](http://typescript-lang.ru/docs/declaration%20files/Publishing.html)