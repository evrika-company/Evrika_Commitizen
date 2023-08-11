# Инструкция по установке и использованию Evrika Standarts и дополнительного набора пакетов
 
1. [Установка nvm и Node.js](#chapter1)
2. [Установка пакета "Evrika_Standarts"](#chapter2)
3. [Подключение и установка "Commitizen" ](#chapter3)
4. [Добавление пакета "semantic-release" и его настройка](#chapter4)
5. [Завершение установки](#chapter5)
6. [Начинаем использование "Evrika Standarts"](#chapter6)

## Шаг 1 : Установка nvm, и с помощью него Node.js <a name="chapter1"></a>

Для уставноки пакета "Evrika Standarts" и дополнительного набора вспомогательных пакетов нам понадобится __Node.js__. В целях создания более простого процесса управления версиями __Node.js__ мы будем использовать систему управления версиями [nvm](https://github.com/nvm-sh/nvm). Выполните следующие шаги для установки: 

__1.Установка nvm:__

Для пользователей Linux и macOS можно выполнить установку NVM, используя curl:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Или использовать wget:

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Для пользователей Windows существует NVM для Windows, который можно загрузить с официального сайта проекта NVM: https://github.com/coreybutler/nvm-windows.

__2.Проверка установленной версии nvm :__

После успешной установки NVM перезагрузите ваш терминал или командную строку, чтобы активировать изменения.
Для проверки установки выполните :

```sh
nvm -v
```

__3.Установка Node.js :__

Просмотр доступных версий Node.js :

```sh
nvm ls-remote
```

Установка определенной версии Node.js :

```sh
nvm install <version>
```

Например, для установки Node.js версии 18.16.1, выполните:

```sh
nvm install 18.16.1
```

Для активации уставновленной версии выполните :

```sh
nvm use <version>
```

Пример активации для версии 18.16.1 :

```sh
nvm use 18.16.1
```

__4.Проверка установленной версии Node.js :__

```sh
node -v
```

## Шаг 2 : Установка пакета "Evrika Standarts" <a name="chapter2"></a>

Для улучшения поставки принятых соглашений и конфигурации был разработан данный пакет.Чтобы его уставновить выполните следующую команду : 

```sh
npm i -D evrika_standarts
```
Данная команда уставновит целевой пакет в качестве завивимости разработки 

## Шаг 3 : Подключение и установка "Commitizen" <a name="chapter3"></a>

Следующим шагом для автоматизации процесса написания коммитов и версионирования является установка пакета [Commitizen](https://commitizen-tools.github.io/commitizen/) и его адаптеров для расширения встроенной конфигурации. Выполните следующие шаги установки :

__1.Установка Commitizen :__

```sh
npm i -D commitizen
```

__2.Глобальная установка Commitizen :__

```sh
npm i -g commitizen
```


__3.Установка адаптера cz-git :__

```sh
npm i -D cz-git
```

__4.Настройка конфигурации :__

В файле __package.json__ на верхнем уровне вложенности добавьте следующую конфигурацию :

```json
"config": {
  "commitizen": {
    "path": "node_modules/cz-git",
    "czConfig": "node_modules/evrika_standarts/src/commitizen_customize/index.js"
  }
},
```

## Шаг 4 : Добавление пакета "semantic-release" и его настройка <a name="chapter4"></a>

В качестве инструмента для автоматического версионирования, выпуска releases и генерации CHANGELOG.md мы используем  [semantic-release](https://semantic-release.gitbook.io/semantic-release/). В дополнение к нему идут официальные плагины. В процессе установки указаны плагины которые используются нашей командой, в вашем случае может отличаться Git hosting или другие элементы. Подробную информацию вы можете найти [здесь](https://semantic-release.gitbook.io/semantic-release/extending/plugins-list).

Для установки выполните следующие шаги :

__1.Установка пакета semantic-release :__

```sh
npm i -D semantic-release
```
__2.Установка плагинов для semantic-release :__

```sh
npm install --save-dev @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog @semantic-release/npm @semantic-release/github @semantic-release/git
```
__3.Настройка конфигурации :__

В файле package.json на верхнем уровне вложенности добавьте следующую конфигурацию :

```json
  "release": {
    "branches": [
      "master"
    ],
    "plugins": [
      "@semantic-release/commit-analyzer",
      "@semantic-release/release-notes-generator",
      "@semantic-release/changelog",
      "@semantic-release/npm",
      "@semantic-release/github",
      [
        "@semantic-release/git",
        {
          "assets": [
            "package.json",
            "CHANGELOG.md"
          ]
        }
      ]
    ],
    "git": {
      "assets": [
        "package.json",
        "CHANGELOG.md"
      ],
      "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
    },
    "preset": "angular",
    "ci": true,
    "npmPublish": false,
    "changelogFile": "CHANGELOG.md"
  }
```

__4.Добавление скрипта для выполнения версионирования :__

В файле package.json на верхнем уровне вложенности в блоке "scripts" добавьте следующий код :


```json
  "release": "semantic-release",
```

Пример :

```json
"scripts": {
    "development": "vite --mode development",
    "production": "vite --mode production",
    "preview": "vite preview --port 4173",
    "release": "semantic-release",
  },
```
## Шаг 5 : Завершение установки <a name="chapter5"></a>

Для заврешения установки необходимо в корне файловой структуры проекта создать файл __.gitignore__ .Это необходимо чтобы зависимости и пакеты необходимые для работы инструмента не попали в наш репозиторий.Далее внутри файла нужно добавить следующий код:

```sh
# node.js
node_modules/
```

## Шаг 6 : Начинаем использование "Evrika Standarts" <a name="chapter6"></a>

__1. Создание commit :__

Для создания сообщения о фиксации более не требуется использовать команду __git commit__ вместо этого выполните следующую команду :

```sh
git cz
```
После чего перед вами откроется интерактивная CLI для упрощенного процесса создания сообщения о фиксации.Следуйте инструкциям и подсказкам  и вы сможете создать осмысленное сообщение о фиксации : )

__2.Автоматическое версионирование проекта, выпуск releases, генерация releases-notes :__

Выше вы могли прочитать список самых рутинных задач котороые , что же с ними делать? Конечно Автоматизировать !!! 

В качестве инсрументов автоматизации вы можете использовать GitLab CI, GitHub Actions, CircleCI в связке [semantic-release](https://semantic-release.gitbook.io/semantic-release/).Ниже представлен пример конфигурации созданный для GitHub Actions. Для более подробной информации касательно создания worflows c GitHub Actions обратитесь к [документации](https://docs.github.com/en/actions).


Пример конфигурации :
```yml
name: Evrika Release
run-name: ${{ github.actor }} is started out Release Actions 🚀
on:
  workflow_dispatch

permissions:
  contents: read 

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest
    permissions:
      contents: write 
      issues: write 
      pull-requests: write 
      id-token: write 
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "lts/*"
      - name: Install dependencies
        run: npm ci
      - name: Release Evrika Project
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: npm run release
```

```sh
npm i -D evrika_standarts
npm i -g commitizen
npm i -D cz-git
npm i -D semantic-release
npm install --save-dev @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog @semantic-release/npm @semantic-release/github @semantic-release/git
```
