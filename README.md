# Рекомендации по архитектуре front-end проектов на Vue.js v2

## Стандарты написания кода

### Принципы проектирования

Использовать общепринятые принципы проектирования кода:

- [Clean Code](https://github.com/devSchacht/clean-code-javascript)
- [DRY](https://web-creator.ru/articles/dry)
- [KISS](https://web-creator.ru/articles/kiss)
- [YAGNI](https://web-creator.ru/articles/yagni)
- [SOLID](https://web-creator.ru/articles/solid)

### Статический анализ и форматирование

Для предотвращения ошибок в коде использовать линтеры:

- [CommitLint](https://commitlint.js.org/)
  ([пример конфигурации](https://github.com/alex-lit/config-commitlint))
- [ESLint](https://eslint.org/)
  ([пример конфигурации](https://github.com/alex-lit/config-eslint))
- [HTMLLint](https://github.com/linthtml/linthtml)
  ([пример конфигурации](https://github.com/alex-lit/config-htmllint))
- [MarkdownLint](https://github.com/DavidAnson/markdownlint)
  ([пример конфигурации](https://github.com/alex-lit/config-markdownlint))
- [NPMLint](https://github.com/tclindner/npm-package-json-lint)
  ([пример конфигурации](https://github.com/alex-lit/config-npmlint))
- [Stylelint](https://stylelint.io/)
  ([пример конфигурации](https://github.com/alex-lit/config-stylelint))

Стандартизировать код-стайл:

- [Prettier](https://prettier.io/)
  ([пример конфигурации](https://github.com/alex-lit/config-prettier))

Пример пакта, объединяющего в себе все вышеперечисленные линтеры:
[lint-kit](https://github.com/alex-lit/lint-kit).

## Поддержка браузеров

Для явного указания поддерживаемых браузеров использовать
[Browserslist](https://github.com/browserslist/browserslist).

Пример конфигурации:

```toml
# .browserslistrc
> 5%
last 2 versions
not Baidu > 0
not Explorer > 0
not OperaMini all
not UCAndroid > 0
not dead
```

## Предпочитаемые технологии

### Общее

- [TypeScript](http://www.typescriptlang.org/) - типизированный ЯП
- [Nuxt.js](https://nuxtjs.org/) - фреймворк для разработки универсальных
  приложений

  Полезные расширения для nuxt:

  - [apollo](https://github.com/nuxt-community/apollo-module)
  - [auth](https://auth.nuxtjs.org/)
  - [axios](https://axios.nuxtjs.org/)
  - [content](https://content.nuxtjs.org/installation/)
  - [i18n](https://i18n.nuxtjs.org/basic-usage/)
  - [image](https://image.nuxtjs.org/api/$img/)
  - [sitemap](https://sitemap.nuxtjs.org/)

  Официальный список расширений:

  - [modules.nuxtjs.org](https://modules.nuxtjs.org/)

### Работа с API

- [Apollo Client](https://apollo.vuejs.org/) - REST/GQL клиент
  - [apollo-link-rest](https://www.apollographql.com/docs/react/api/link/apollo-link-rest/) -
    расширение Apollo для работы с REST API

### Стилизация

- [BEM](https://ru.bem.info/) - методология Yandex для построения масштабируемых
  интерфейсов
- [SCSS](https://sass-lang.com/) - CSS препроцессор

### Тестирование

- [Jest](https://facebook.github.io/jest/) - юнит-тестирование

## Файловая архитектура проекта

Исходники хранятся в директории `src` и группируются по слоям.

Пример структуры:

```toml
[scripts] # npm скрипты (.sh)
[src]
    [api] # слой работы с API
    [assets] # ресурсы используемые при компиляции
    [components] # компоненты
    [layouts] # шаблоны страниц
    [locales] # словари локализации
    [middleware] # функции выполняемые перед переходом по страницам
    [pages] # страницы
    [plugins] # плагины Vue.js и их конфиги
    [router] # маршрутизатор
    [static] # статические ресурсы (robots.txt, иконки и т.д)
    [store] # модули состояния приложения (Vuex)
    [utils] # утилиты
...конфиги
```

Нейминг папок и файлов - `kebab-case`.

## Компоненты

### Файловая структура

Файлы компонент экспортируются из индексного файла. Дочерние компоненты и ассеты
находятся в поддиректориях.

Пример:

```toml
[components]
    [my-component]
        my-component.component.vue # код компонента
        my-component.spec.ts # тесты
        index.ts # экспорт
        [components]
            [my-component-partial]
                my-component-partial.component.vue # код компонента
                my-component-partial.spec.ts # тесты
                index.ts # экспорт
        [images]
            logo.svg # изображение, уникальное для текущего компонента (my-component)
    [my-another-component]
    ...
```

Этот же принцип применяется и к остальным слоям - utils, plugins и т.д.

### Типизация

Для типизации компонент используется синтаксис на основе классов:
[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) +
[vuex-class](https://github.com/ktsn/vuex-class).

### Стили

- Инкапсуляция:
  [Vue Scoped CSS](https://vue-loader.vuejs.org/ru/guide/scoped-css.html)
- Препроцессор: [SCSS](https://sass-lang.com/)
- Методология: [BEM](https://ru.bem.info/) (1 компонент - 1 блок)

**Применение фреймворков типа `Bootstrap`, `Tailwind` и т.д. не допускается.**

## API

Разделение по сущностям с принципом 1 файл - 1 метод. Пример:

```toml
[api]
    [user]
        get-user.query.gql
        update-user.mutation.gql
    [auth]
        check-auth.query.gql
        ...
    index.ts # экспорт API-методов
```

## Хранилище данных

Использовать Vuex, сегментация по модулям.

> Рекомендуется использовать стор только в случае крайней необходимости, все
> стандартные взаимодействия между компонентами решать основным способом:
> параметры вниз / события наверх, provide / inject, слоты и т.д.

## Локализация

Стандартный пакет [Vue I18n](https://kazupon.github.io/vue-i18n/). Языковые
файлы - 1 файл на 1 язык для всего приложения:

```toml
[locales]
    ru.json
    en.json
    ...
```

Структура файлов локализации `плоская`, имена ключей в `CONSTANT_CASE` с
префиксом, пример:

```json
{
  "I18N_GOODBYE": "Пока",
  "I18N_HELLO": "Привет"
}
```

## Контроль версий

Работу с ветками репозитория осуществлять по модели
[GitFlow](https://habr.com/ru/post/106912/), для коммитов использовать стандарт
[Conventional Commits](https://www.conventionalcommits.org/ru/).

Перед коммитом автоматически выполнять проверку кода линтерами на наличие
ошибок.
