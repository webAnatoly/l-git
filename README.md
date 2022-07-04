Это репозитарий git для тренировки работы с системой контроля версий git

Глобальный конфиг лежит в домашней дериктории пользователя `~/.gitconfig`

`.git/config` - локальный конфиг конкретного проекта

`git config --global core.pager 'less -RFX'` - этой командой устанавливаем, чтобы гит по умолчанию использовал программу less для показа справки. Он её и так использует, но без параметров -RFX

`git show --pretty=fuller` - показать подробно коммит с выведением автора кода, и того кто сделал коммит.

`git add .` - команда git add c точкой передает в git весь текущий каталог. 

`git config --global core.excludesFile ~/.gitignore` эта команда добавляет в глобальный конфиг строку о том, что существует глобальный файл .gitignore в директории пользователя.  
Глобальный .gitignore используется для игнора например вспомогательных файлов IDE, например WebStorm создает каталог .idea для своих вспомогательных файлов. Вот её можно добавлять в глобальный .gitignore

#### Синтаксис .gitignore
Просто имя, например `vendor` - git игнорирует везде все файлы и деректории с таким названием.

Слеш на конце означает папку. Причем если слеш только в конце, то будет игнорится папка с таким именем в любом месте проекта.  
`build/`

Если нужно игнорить только корневую папку, то ставим слеш и в начале тоже, вот так  
`/build/`

Общее правило для слеша такое, что если где-то есть слеш (кроме как в конце), то включается режим поиска совпадений строго от корня. Поэтому в таком примере `secret/key` будет проигнорирован файл `key` в корневой папке `secret`, но например тут `doc/secret/key` файл key не будет проигнорирован.

####
Если файл уже был проиндексирован, то после внесения его в .gitignore нужно выполнить следующею команду
git update-index --assume-unchanged <yourfile>
# Подробнее о синтаксисе см. по ссылке http://webnotes.by/docs/raznoe/pravila-sintaksisa-fayla-gitignore
Для папки git rm -r --cached <yourfolder>


#### Правило больших описаний к коммитам:
Первая строка делается короткой и без точки и после неё пустая строка. Дальше уже списком описываем все изменения в подробностях.
Пример:  
1 Created welcome page  
2  
3 * Added feature A  
4 * Fix feature B  
Конец примера.  

Звездочки в примере ничего не значат, просто элементы форматирования. Можно использовать точки, тире и т.д. Номера 1,2,3,4 это номера строк в текстовом редакторе. Здесь я их указал, чтобы показать что после заголовка коммита на первой строке идет пустая строка.


`git branch <branchname>`  команда для создания новой ветки  
`git checkout <branchname>` переключится на другую ветку. По сути эта команда обновляет ссылку `ref: refs/heads/master` на `ref: refs/heads/<branchname>` в файле HEAD  
`git checkout -b <branchname>` создать ветку и тут же переключится на неё  
`git checkout -f <branchname>` удалит все несохраненные (незакоммиченные) изменения в текущей ветке и переключится на новую ветку. Вызов checkout c флагом -f без указания ветки можно использовать для отмены всех незакоммиченных изменений в текущей ветке  

`git stash`	прячет незакоммиченные изменения где-то у себя внутри. И теперь можно переключится с этой ветки на другую, используя обычную команду `checkout <branchname>`. В другой ветке можно что-то поделать и вернуться обратно. Теперь, чтобы вернуть изменения нужно выполнить команду `git stash pop`.  
`git stash pop`	возвращает изменения, которые были спрятаны командой git stash  
`git stash list`	показывает есть ли застэшенные данные и на какой ветке они находятся  

Если мы вносим изменения в файл, который в обоих ветках одинаков и затем НЕ коммитим их и переключаемся на другую ветку командо `checkout <branchname>`, то гит спокойно переключится потому что ему не надо переписывать файл ибо он одинаков в обоих ветках.  

`git log master..<branchname>` выведет историю коммитов в ветке без коммитов унаследованных от родительской ветки master

Если, в процессе разработке мы поняли, что несколько последних коммитов в ветке master стоило бы выделить в отдельную ветку, то это можно сделать так. Сначала создаем новую ветку на текущем коммите командой git branch <имя новой ветки>. Затем либо командой git branch -f master <номер коммита откуда хотим чтобы ветка master разветвлялась>. Либо другой вариант git checkout -B master <номер коммита>.  Подробнее смотри видео 4.4 Скринкаст по Git Ильи Кантора - Передвижение веток "вручную".

`git show HEAD~` показать родительский коммит HEAD. Кол-во тильд не ограничено. Для удобства их можно заменять цифрами. Например `git show HEAD~4.`  
`git show HEAD`	сокращенно можно писать `git show @` В OS Windows собачку надо брать в ковычки `git show '@'`

Примеры:  
`git show @` покажет последний коммит  
`git show @~` покажет предпоследний коммит  
`git show @~5` покажет пятый коммит от HEAD  
 
`git show <номер коммита || @~n> <path>` - Это команда чтобы посмотреть предедущею версию файла т.е. каким файл был во время того или иного коммита 
`git show :/<word>` - Поиск коммита по слову в его описании  

Когда мы соединяем ветки командой git merge <имя ветки> гит записывает номер коммита ветки master в файл `.git/ORIG_HEAD` И теперь если мы когда захотим отменить слияние мы можем это сделать командой `git branch -f master ORIG_HEAD`

`git branch -D <branchname>` форсированно удаляет ветку. Форсированно удалять ветки не надо, потомучто все коммиты сделанные в ней исчезнут. Обычно сначало сливают ветки, а потом удаляют, но если мы понимаем, что ветвь тупиковая и не хотим ее сливать с основной ветвью и хотом её просто удалить, то тогда можно удалять форсированно.

Гит на самом деле удаляет только ссылку указывающие на коммиты. Коммиты остаются в базе, но превращаются в недостижимые и со временем он их удалит. Это что-то типа garbage collector. Поэтому сразу же после форсированного удаления ветки, её можно востановить (точнее создать новую ветку с тем же именем) командой `git branch <имя-ветки> <номер-коммита-на-который-указывала-удаленная-ветка> `

Этот номер комита выводится в консоль после удаления ветки примемрно вот так `Deleted branch <branchame> (was 234uyw2)`. Если забыли номер удалённого коммита, то его можно отыскать в рефлогах с помощью команды `git reflog`.

Гит сохраняет историю переходов по коммитам тут `.git/logs/refs/HEAD` для удобного вывода используется команд `git reflog`

#### 4.10 Скринкаст по Git - Ветки - Лог ссылок: reflog
`git reflog` - в записи рефлога с лева указывается коммит на который мы перешли

`git log --oneline -g` показывает коммиты не из ветки, а из рефлога, т.е. это то же самое что и `git reflog show`
`git reflog --date=iso` вывод рефлога с датами

##### Недостижимые коммиты
`git fsck --unreachable`   выводит список недостижимых коммитов

Пока жив коммит - совершенно точно живы и находятся в базе все его родители.
Поэтому если коммит жив, то можно воссоздать всю ветку (по-сути создать новую ветку). 

#### Теги
Теги удобно использовать для пометки версий нашего продукат, который мы разрабатываем.
Тег просто указывает на коммит. Своего рода закладка. 
Пример создания тега с аннотацией. 
git tag -a -m 'Описание тега' v1.0.0 <номер_коммита>

#### Отмена изменений
Отменить все незакоммиченные изменения можно командой git reset --hard
`git reset --hard` это жесткий reset т.е. все незакомиченные изменения удаляются безвозвратно.

`git reset --soft @~` Это мягкий reset т.е. мы переходим на указанный коммит,
а изменения в рабочей деректории не удаляются они переходят в состояние staged т.е. ожидающие коммита.
Мягкий reset идельно подходит, если после совершения коммита нам нужно что-то в неё поправить.
После того как поправили добавляем новые изменения в индекс командой git add
и потом выполняем команду `git commit -c ORIG_HEAD`. Флаг `-с` говорит гиту,
что нужно взять описание коммита из какого-то коммита.
Маленькое `-с` откроет редактор, большое `-С` молча скопирует описание коммита и закоммитит.
В нашем случае мы берем описание из коммита который в файле ORIG_HEAD потому что мы только что с него ушли. 

Полезные ссылки:  
[Скринкаст](https://learn.javascript.ru/screencast/git)  
[GitHowTo](https://githowto.com/ru)  
[Документация](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)  

==

Как переключаться между ветками в git, когда в текущей ветке есть несохраненные изменения?

Как вариант - спрятать изменения `git stash`, перейти на другую ветку,
взглянуть на то, что было нужно, перейти обратно и вытащить изменения обратно `git stash pop`


==
## Про команду `git merge --squash`
Как это работает

```
git checkout master
git merge --squash feature123
git commit -m'merged feature #123'
```

Все изменения в ветке `feature123` становятся одним коммитом в ветке `master`.

Это удобно, если ветка содержит много незначительных коммитов, которые «неинтересны» для общей истории.

==

Статья о том как писать коммиты
[Учимся писать информативные комментарии к GIT-коммитам](https://habr.com/ru/company/otus/blog/537196/)

==

## Про флаги git log

### git log --grep и --author и --committer
Опция --author дает возможность фильтровать по автору коммита, а опция --grep искать по ключевым словам в сообщении коммита.
Например: `git log --grep <pattern>`

### git log -S
показывает только те коммиты, в которых изменение в коде повлекло за собой добавление или удаление этой строки. Например, если вы хотите найти последний коммит, который добавил или удалил вызов определенной функции, вы можете запустить команду: `git log -S function_name`

### Таблица флагов git log

-(n)                Показывает только последние n коммитов.

--since, --after    Показывает только те коммиты, которые были сделаны после указанной даты.

--until, --before   Показывает только те коммиты, которые были сделаны до указанной даты.

--author            Показывает только те коммиты, в которых запись author совпадает с указанной строкой.

--committer         Показывает только те коммиты, в которых запись committer совпадает с указанной строкой.

--grep              Показывает только коммиты, сообщение которых содержит указанную строку.

-S                  Показывает только коммиты, в которых изменение в коде повлекло за собой добавление или удаление указанной строки.

[Источник](https://git-scm.com/book/ru/v2/%D0%9E%D1%81%D0%BD%D0%BE%D0%B2%D1%8B-Git-%D0%9F%D1%80%D0%BE%D1%81%D0%BC%D0%BE%D1%82%D1%80-%D0%B8%D1%81%D1%82%D0%BE%D1%80%D0%B8%D0%B8-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%BE%D0%B2)


Мой пример:
`git log --until=2016-01-31 --author=Jeff --committer=Jeff --grep=http` - Выведет все коммиты до 31 января 2016г. сделанные человеком по имени Jeff и в тексте коммитов присутствует слово "http"

==

## Разные команды

`git log --oneline --all --graph`
`git log -p` or `git log --patch` - shows the difference (the patch output) introduced in each commit.
`git log --pretty=oneline`
`git log --pretty=short`
`git log --pretty=full`
`git log --pretty=fuller`
`git log --pretty=format:"%h - %an, %ar : %s"` - кастомный формат где % это плейсхолдеры гита из документации
`git log --stat`    - to see some abbreviated stats for each commit
`git log --patch`   - смотреть изменения в файлах внесённые коммитом
`git log --patch-with-stat`

`git diff --numstat  ветка1 ветка2`

xev - print contents of X events (появляется окно, на котором можно кликать и вводить символы с клавиатру и инфа о них будет в консоли (коды кнопок и т.д.))

`fg` - если был "выход" из консольной программы через Ctrl+Z - то она "сворачивается" и процесс висит в памяти (так я понял) и покоманде `fg` будет возобновлен этот процесс.

`xed` - текстовый редактор. Есть еще `gedit` но он у меня не установлен

## Рецепт слияня веток перед пулл реквестом.
Зайти на фича ветку `git checkout <task_branch>` и находясь на ней с сервера в неё затащить актуальюную мастер ветку командой `git pull origin master`

## Взять ветку с удалённого репозитория
git checkout -b my_branch origin/my_branch
git pull

## Вывести список конфликтных файлов при мерджинге (слиянии)
git diff --name-only --diff-filter=U
