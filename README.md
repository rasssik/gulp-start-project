# Шаблон для проекта с gulp
<table>
  <thead>
    <tr>
      <th>Команда</th>
      <th>Описание</th>
    </tr>
  </thead>
   <tbody>
    <tr>
      <td><code>npm i</code></td>
      <td>Установить зависимости</td>
    </tr>
    <tr>
    <tr>
      <td><code>npm start</code></td>
      <td>Запуск локального сервера </td>
    </tr>
      <td><code>npm run build</code></td>
      <td>Запуск сборку проекта</td>
    </tr>
    <tr>
      <td><code>gulp ЗАДАЧА</code></td>
      <td>Запустить gulp задачу (список задач в <code>gulpfile.js</code>)</td>
    </tr>
   </tbody>
</table>

## Парадигма
- Используется именование классов, файлов и переменных по БЭМ.
- Список использованных в проекте БЭМ-блоков и доп. файлов указан в `./projectConfig.json`.
- Каждый БЭМ-блок в своей папке внутри `./src/blocks/` (стили, js, картинки, разметка; обязателен только стилевой файл).
- Есть глобальные файлы: стилевые, js, шрифты, картинки.
- Диспетчер подключения стилей `./src/scss/style.scss` генерируется автоматически при старте любой gulp-задачи.
- Для разметки можно использовать [микрошаблонизацию](https://www.npmjs.com/package/gulp-file-include). А можно и не использовать.
- Перед созданием коммита запускается проверка стилевых файлов, входящих в коммит.

## Структура проекта

```bash
build                 # Папка сборки, здесь работает сервер автообновлений
resources             # Папка для файлов с макетами psd-макетами и тд
src/                  # Исходные файлы
  blocks/             # - блоки проекта
  components/         # - готовые html-шаблоны (формы, кнопки и тд)
  fonts/              # - шрифты проекта (автоматически скопируются в папку сборки)
  img/                # - графика для проекта
  js/                 # - js-скрипты
  scss/               # - диспетчер подключений и глобальные стили
  index.html          # - главная страница проекта
```

## Стили

Файл-диспетчер подключений (`.src/scss/style.scss`) формируется автоматически на основании указанных в `./projectConfig.json` блоков и доп. файлов. Писать в `.src/scss/style.scss` что-либо руками бессмысленно: при старте автоматизации файл будет перезаписан.

Используемый постпроцессинг:

1. [autoprefixer](https://github.com/postcss/autoprefixer)
2. [css-mqpacker](https://github.com/hail2u/node-css-mqpacker)
3. [gulp-csso](https://www.npmjs.com/package/gulp-csso)

## Подключение блоков

Список используемых блоков и дополнительных подключаемых файлов указан в `./projectConfig.json`. Список файлов и папок, взятых в обработку можно увидеть в терминале, если раскомментировать строку `console.log(lists);` в `gulpfile.js`.

### `blocks`

Объект с блоками, используемыми на проекте. Каждый блок — отдельная папка с файлами, по умолчанию лежат в `./src/blocks/`.

Каждое подключение блока — массив, который можно оставить пустым или указать файлы элементов или модификаторов, если они написаны в виде отдельных файлов. В обоих случаях в обработку будут взяты одноименные стилевые файлы, js-файлы и картинки из папки `img/` блока.

Пример, подключающий 3 блока:

```
"blocks": {
  "page": [],
  "page-header": [],
  "page-footer": []
}
```

### `addCssBefore`

Массив с дополнительными стилевыми файлами, которые будут взяты в компиляцию ПЕРЕД стилевыми файлами блоков.

Пример, берущий в компиляцию переменные, примеси, функции и один дополнительный файл из папки зависимостей (он будет преобразован в css-импорт, который при постпроцессинге будет заменен на содержимое файла).

```
"addCssBefore": [
  "./src/scss/variables.scss",
  "./src/scss/mixins.scss",
  "./src/scss/functions.scss"
],
```

### `addCssAfter`

Массив с дополнительными стилевыми файлами, которые будут взяты в компиляцию ПОСЛЕ стилевых файлов блоков.

```
"addCssAfter": [
  "./src/scss/somefile.scss"
],
```
## Удобное создание нового блока

Предусмотрена команда для быстрого создания файловой структуры нового блока.

```bash
# формат: node createBlock.js ИМЯБЛОКА [доп. расширения через пробел]
node createBlock.js block-1 # создаст папку блока, block-1.html, block-1.scss
node createBlock.js block-2 js img # создаст папку блока, block-2.html, block-2.scss, block-2.js, и подпапку img/ для этого блока
```

Если блок уже существует, файлы не будут затёрты, но создадутся те файлы, которые ещё не существуют.
