# Инициализация проекта

Главная цель — создать минимально необходимый набор зависимостей в проекте, чтобы сделать ключевую функциональность будущего проекта

В процессе инициализации проекта, мы:

- установим менеджер пакетов.
- создадим базовый шаблон vite+react+typescript.
- установим библиотеку компонентов Mantine.
- отправим код в репозиторий Github.

## Шаг 1

Начнем с выбора версии Node.js. Указываем `@latest`, чтобы точно получить последнюю версию менеджера пакетов.

```text
nvm use 20
corepack prepare pnpm@latest
```

## Шаг 2

Создадим стандартный шаблон React + TypeScript на базе Vite.

```text
pnpm create vite@latest brello-app --template react-ts
cd brello-app
pnpm install
```

## Шаг 3

При необходимости обновляем pnpm в проекте.

```text
corepack use pnpm@latest
pnpm install
```

## Шаг 4

Создадим git-репозиторий и сохраним начальное состояние.

```text
git init
git add .
git commit -m "Initial commit"
```

## Шаг 5

Сразу же стоит создать репозиторий brello в Github/Gitlab/etc, а локально добавить remote:

```text
git remote add origin git@github.com:{YOUR_NAME}/brello.git
git push origin $(git branch --show-current) --set-upstream
```

## Шаг 6

Теперь, локальный и удаленный репозитории связаны и можно выполнять

```text
git push origin main
```

## Шаг 7

Теперь можно установить Mantine со всеми требуемыми плагинами ([туториал](https://mantine.dev/getting-started/#get-started-without-framework)).

```text
pnpm add @mantine/core @mantine/hooks @mantine/dates dayjs @mantine/notifications @mantine/tiptap @tabler/icons-react @tiptap/react @tiptap/extension-link @tiptap/starter-kit @mantine/dropzone @mantine/spotlight @mantine/modals
pnpm add -D postcss postcss-preset-mantine postcss-simple-vars
```

## Шаг 8

Далее по инструкции Mantine, создаем `postcss.config.cjs`. Расширение `.cjs` здесь используется потому что, файл описан как `module.exports = {}`, но в `package.json` `type=module`, значит все `.js` файлы воспринимаются как ESModules.

```js
// postcss.config.cjs

module.exports = {
  plugins: {
    "postcss-preset-mantine": {},
    "postcss-simple-vars": {
      variables: {
        "mantine-breakpoint-xs": "36em",
        "mantine-breakpoint-sm": "48em",
        "mantine-breakpoint-md": "62em",
        "mantine-breakpoint-lg": "75em",
        "mantine-breakpoint-xl": "88em",
      },
    },
  },
};
```

## Шаг 9

Теперь нужно заимпортировать только существующие стили от mantine пакетов. Вставить их нужно в `src/main.tsx`.

```js
// src/main.tsx
import "@mantine/core/styles.css";
import "@mantine/dates/styles.css";
import "@mantine/dropzone/styles.css";
import "@mantine/hooks/styles.css";
import "@mantine/modals/styles.css";
import "@mantine/notifications/styles.css";
import "@mantine/spotlight/styles.css";
import "@mantine/tiptap/styles.css";
// ...
```

## Шаг 10

Теперь запускаем!

`@mantine/modals/styles.css` и `@mantine/modals/styles.css` упадут с ошибкой, пока что импорты этих строк нужно закоментировать. После чего выполнить команду:

```text
pnpm dev
```

## Шаг 11

Пришло время добавить `MantineProvider` из `@mantine/core`. `StrictMode` в нашем случае пользы принесет не много, поэтому можем со спокойной душой удалить его.

```js
// ...
createRoot(document.getElementById("root")!).render(
  <MantineProvider>
    <App />
  </MantineProvider>
);
// ...
```

## Шаг 12

На каждом этапе (добавление/удаление/миграция/обновление) стоит проверять общую работоспособность приложения:

```text
pnpm dev # нет ошибок во время сборки и в браузере
pnpm build # typescript и vite успешно завершают сборку без предупреждений
```

## Шаг 13

И если всё отлично, коммитим и пушим, чтобы не потерять рабочее состояние:

```text
git add .
git commit -m "Integrate Mantine"
git push origin main
```

## Шаг 14

Сейчас ваш проект должен выглядеть как-то так! Здесь пропущены стандартные директории dist, node_modules, а также файлы .gitignore, .editorconfig, .prettierrc.

.
├── public
│ └── vite.svg
├── src
│ ├── assets
│ │ └── react.svg
│ ├── App.css
│ ├── App.tsx
│ ├── index.css
│ ├── main.tsx
│ └── vite-env.d.ts
├── README.md
├── eslint.config.js
├── index.html
├── package.json
├── pnpm-lock.yaml
├── postcss.config.cjs
├── tsconfig.app.json
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts

## Итог

Этого набора файлов в проекте достаточно, чтобы начать реализовывать главную функциональность вашего проекта! Дополнительно, про конфигурацию можно почитать:

- [PostCSS](https://github.com/postcss/postcss)
- [postcss-simple-vars](https://github.com/postcss/postcss-simple-vars)
- [postcss-preset-mantine](https://mantine.dev/styles/postcss-preset/)
- [Почему Vite создает несколько tsconfig файлов?](https://www.geeksforgeeks.org/why-does-vite-create-multiple-typescript-config-files-tsconfigjson-tsconfigappjson-and-tsconfignodejson/)
