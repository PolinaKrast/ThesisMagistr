![build](https://img.shields.io/github/actions/workflow/status/NikitaDmitryuk/ThesisMagistr/main.yml)
![downloads](https://img.shields.io/github/downloads/NikitaDmitryuk/ThesisMagistr/total)
![release](https://img.shields.io//github/v/release/NikitaDmitryuk/ThesisMagistr?display_name=tag)

# Latex шаблон диплома и презентации МГТУ им. Баумана (Latex BMSTU diploma and presentation template)

Выпускная квалификационная работа магистра, оформленная в Latex. Ее можно использовать как шаблон.
Она включает в себя шрифт Times New Roman и автоматическое создание библиографии по ГОСТ 2008.
Проходит Бауманскую программу нормоконтроля TestVKR. Подходит для бакалаврских работ.

**! Пример скомпилированных файлов можно найти в релизах !**

[**Статья про написание диплома и использование данного шаблона**](https://habr.com/ru/post/692596/)

## Достоинства шаблона

1. Шрифт Times New Roman
2. Автоматическая библиография по ГОСТ (bibtex)
3. Автоматизированная сборка в Linux и Windows
4. Автоматическое создание релизов на GitHub
5. Любое количество собираемых файлов (диплом, презентация и что либо еще)
6. Собираемые файлы .tex могут иметь любое имя **без пробелов**
7. Автоматическая очистка временных файлов Latex до и после сборки
8. На компьютере не нужен установленный Latex, вся сборка происходит в контейнере docker
9. Диплом проходит нормоконтроль в Бауманской программе TestVKR
10. Используется самый популярный компилятор - pdflatex

## Использование как шаблон

Клонировать данный репозиторий в свой GitHub можно используя кнопку **Fork**, после чего данный репозиорий появится в ваших репозиториях и его можно будет менять.

## Структура шаблона

```
.
├── extra
│   └── ваши дополнительные файлы
│
├── biblio
│   └── библиография
│
├── chapters
│   └── подключаемые главы
│
├── settings
│   └── преамбула с настройками
│
├── images
│   └── используемые картинки
│
├── install
│   └── скрипт установшик для linux
│
├── diploma.tex
│   └── главный файл диплома
│
├── presentation.tex
│   └── презентация
│
└── makewin.bat
    └── скрипт компиляции для Windows
```

## Сборка в Arch Linux

Рекомендуется собирать этот шаблон в Arch Linux, однако без особых проблем получится собрать его и в Windows.

Для установки и настройки всего необходимого надо запустить скрипт **install.sh** в папке **install**.
Возможно понадобится перезагрузить компьютер, или отдельно выполнить команды в этом скрипте еще раз.

```bash
sudo bash install.sh
```

После чего можно собирать диплом и презентацию командой

```bash
make
```

Собрать что то одно можно так:

```bash
make diploma
make presentation
```

## Сборка под Windows

- [Установить Docker](https://docs.docker.com/desktop/install/windows-install/)

После установки Docker надо перезапустить компьютер (он предложит сам).
Затем, если он сразу не заработает, следовать его инструкциям, он предложить выполнить [следующие шаги](https://docs.microsoft.com/ru-ru/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package).
При выборе дистрибутива Linux выбрать [Ubuntu 22.04 LTS](https://www.microsoft.com/store/apps/9PN20MSR04DW).
Далее следовать иструкциям установки Ubuntu.
После этих шагов Docker должен заработать.

Теперь можно собирать полный диплом и презентацию как они задуманы, для этого необходимо запустить скрипт **makewin.bat**.
Это можно сделать в консоли или просто два раза нажать на **makewin** мышкой.

Первый запуск будет довольно долгий, потому что будет скачиваться контейнер весом около 4гб.

Если вы хотите собирать что-то одно (диплом или презентацию), вы можете временно переместить другой `tex` файл в папку **extra** или раскомментировать нужные строки в `makewin.bat`.

## Создание релиза

Создание релиза происходит на Github в специальном контейнере Docker по пушу тега вида v*. 

Пример создания релиза:

```bash
git add .
git commit -m "your comment"
git push
git tag -a v0.1 -m "your comment"
git push --tags
```

Автоматически запустится процесс GitHub Actions, который закончится примерно через 2-3 минуты. Его можно наблюдать во вкладке *Actions* в репозитории. 
После этого справа в рпозитории появится релиз содержащий диплом со шрифтом times new roman и презентация.

## Работа с библиографией

Для автоматической компиляции библиографии используется специальный файл **.bib** (biblio/biblio.bib), в который необходимо добавлять свои источники в нужном формате.

Рекомендую удалить все содержимое файла **biblio.bib** и добавлять по одному свои источники, так как при случайном добавлении дубликата bibtex будет выдавать ошибки.

P.S. В новой версии диплома добавлена утилита на python для очистки списка литературы от неиспользуемых источников. Она находится в папке **biblio**.

Для ее запуска воспользуйтей следующей командой в терминале, отрытом в папке с библиографией:

```bash
python rm_extra_bib_items.py
```

Программа ищет цитирования во всех файлах `.tex` в папке **chapters**, и нужные источники из файла **bibliography.bib** сохраняет в новый файл с названием **bibliography.bib.new**.

Программа не убирает дубликаты источников. Для избежания дубликатов для *ID* источника нужно использовать `DOI`. Как это делать написано далее.

### Алгоритм добавления нового литературного источника

`DOI` - цифровой идентификатор объекта (digital object identifier). Используется для однозначного идентифицирования статьи. Кроме того, статья может находится на сайтах с разными именами, но ссылка `https://doi.org/{DOI}` будет всегда указывать на нее.

1. Ищем DOI статьи, которую хотим добавить в список литературы. Удобно делать это через сайт [Google Scholar](https://scholar.google.com/). Зачастую это бывает часть URL ссылки на работу.
2. Вставляем DOI на сайте [doi2bib](https://www.doi2bib.org/).
3. Полученое содержимое вставляем в файл **biblio.bib**, ***заменяя автоматически сгенерированное название на DOI статьи*** (это распространенная практика в научной сфере, так вы не добавите случайно дубликат в вашу работу под разными именами).

В тексте работы сослаться на данный источник можно будет так:

```tex
... в работе~\cite{DOI}
```

Если у работы нету DOI, то все необходимые поля придется заполнить самостоятельно (если у работы нету DOI, лучше ее вообще не использовать, это, как правило, плохие работы, которые не публикуются в нормальных местах).

## Возможные проблемы

Странные, непонятно откуда взявшиеся символы могут появиться перед списком литературы.
Это может случится, если картинка будет последней в главе, а за ней не будет никакого текста (или картинка слишком большая, просто уменьшите ее).
Пофиксить это скорее всего не представляется возможным, стоит этого просто избегать.

## От автора

По возможностям улучшениям шаблона пишите на почту или создавайте pull request.

Почта: [dmitryuk.nikita@gmail.com](dmitryuk.nikita@gmail.com)
