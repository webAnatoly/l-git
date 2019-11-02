Это репозитарий git для тренировки работы с системой контроля версий git :)

Глобальный конфиг лежит в домашней дериктории пользователя ~/.gitconfig 

.git/config - локальный конфиг конкретного проекта

git config --global core.pager 'less -RFX' - этой командой устанавливаем, чтобы гит по умолчанию использовал программу less для показа справки. Он её и так использует, но без параметров -RFX

git show --pretty=fuller - показать подробно коммит с выведением автора кода, и того кто сделал коммит.

git add . - команда git add c точкой передает в git весь текущий каталог. 

git config --global core.excludesFile ~/.gitignore
эта команда добавляет в глобальный конфиг строку о том, что существует глобальный 
файл .gitignore в директории пользователя. Глобальный .gitignore используется для игнора например вспомогательных файлов IDE, например WebStorm создает каталог .idea для своих вспомогательных файлов. Вот её можно добавлять в глобальный .gitignore

Синтаксис .gitignore
Слеш на конце означает папку. Причем если слеш только в конце, то будет игнорится папка с таким именем в любом месте проекта
build/

Если нужно игнорить только корневую папку, то ставим слеш и в начале тоже, вот так
/build/

Общее правило для слеша такое, что если где-то есть слеш (кроме как в конце), то включается режим поиска совподений строго от корня. Поэтому в таком примере secret/key будет проигнорирован файл key в корневой папке secret, но например тут doc/secret/key файл key не будет проигнорирован.


Правило больших описаний к коммитам: Первая строка делается короткой и без точки и после неё пустая строка. Дальше уже списком описываем все изменения в подробностях.
Пример:
1 Created welcome page
2
3 * Added feature A
4 * Fix feature B
Конец примера. Звездочки в примере ничего не значат, просто элементы форматирования. Можно использовать точки, тире и т.д. Номера 1,2,3,4 это номера строк в текстовом редакторе. Здесь я их указал, чтобы показать что после заголовка коммита на первой строке идет пустая строка.


git branch <branchname>  команда для создания новой ветки
git checkout <branchname> переключится на другую ветку. По сути эта команда обновляет ссылку ref: refs/heads/master на ref: refs/heads/<branchname> в файле HEAD
git checkout -b <branchname> создать ветку и тут же переключится на неё
git checkout -f <branchname> удалит все несохраненные (незакоммиченные) изменения в текущей ветке и переключится на новую ветку. Вызов checkout c флагом -f без указания ветки можно использовать для отмены всех незакоммиченных изменений в текущей ветке

git stash	прячет незакоммиченные изменения где-то у себя внутри. И теперь можно переключится с этой ветки на другую, используя обычную команду checkout <branchname>. В другой ветке можно что-то поделать и вернуться обратно. Теперь, чтобы вернуть изменения нужно выполнить команду git stash pop.
git stash pop	возвращает изменения, которые были спрятаны командой git stash
git stash list	показывает есть ли застэшенные данные и на какой ветке они находятся

Если мы вносим изменения в файл, который в обоих ветках одинаков и затем НЕ коммитим их и переключаемся на другую ветку командо checkout <branchname>, то гит спокойно переключится потому что ему не надо переписывать файл ибо он одинаков в обоих ветках. 

git log master..<branchname> выведет историю коммитов в ветке без коммитов унаследованных от родительской ветки master

Если, в процессе разработке мы поняли, что несколько последних коммитов в ветке master стоило бы выделить в отдельную ветку, то это можно сделать так. Сначала создаем новую ветку на текущем коммите командой git branch <имя новой ветки>. Затем либо командой git branch -f master <номер коммита откуда хотим чтобы ветка master разветвлялась>. Либо другой вариант git checkout -B master <номер коммита>.  Подробнее смотри видео 4.4 Скринкаст по Git Ильи Кантора - Передвижение веток "вручную".

git show HEAD~	показать родительский коммит HEAD. Кол-во тильд не ограничено. Для удобства их можно заменять цыфрами. Например git show HEAD~4. 
git show HEAD	сокращенно можно писать git show @ В OS Windows собачку надо брать в ковычки git show '@' 
Примеры:
git show @	покажет последний коммит
git show @~	покажет предпоследний коммит
git show @~5	покажет пятый коммит от HEAD
 
git show <номер коммита || @~n> <path>	Это команда чтобы посмотреть предедущею версию файла т.е. каким файл был во время того или иного коммита 
git show :/<word>			Поиск коммита по слову в его описании

Когда мы соединяем ветки командой git merge <имя ветки> гит записывает номер коммита ветки master в файл .git/ORIG_HEAD И теперь если мы когда захотим отменить слияние мы можем это сделать командой git branch -f master ORIG_HEAD

`git branch -D <branchname>` форсированно удаляет ветку. Форсированно удалять ветки не надо, потомучто все коммиты сделанные в ней исчезнут. Обычно сначало сливают ветки, а потом удаляют, но если мы понимаем, что ветвь тупиковая и не хотим ее сливать с основной ветвью и хотом её просто удалить, то тогда можно удалять форсированно.

Гит на самом деле удаляет только ссылку указыавющие на коммиты. Коммиты остаются в базе, но превращаются в недостижимые и со временем он их удалит. Это что-то типа garbage collector. Поэтому сразу же после форсированного удаления ветки, её можно востановить (точнее создать новую ветку с тем же именем) командой git branch <имя-ветки> <номер-коммита-на-который-указывала-удаленная-ветка> Этот номер комита выводится в консоль после удаления ветки примемрно вот так `Deleted branch <branchame> (was 234uyw2)`. Если забыли номер удалённого коммита, то его можно отыскать в рефлогах с помощью команды git reflog. Гит сохраняет историю переходов по коммитам тут .git/logs/refs/HEAD для удобного вывода используется команд git reflog

### 4.10 Скринкаст по Git - Ветки - Лог ссылок: reflog
`git reflog` - в записи рефлога с лева указывается коммит на который мы перешли

`git log --oneline -g` показывает коммиты не из ветки, а из рефлога, т.е. это то же самое что и `git reflog show`
`git reflog --date=iso` вывод рефлога с датами

`fsck --unreachable`   выводит список недостижимых коммитов

Пока жив коммит - совершенно точно живы и находятся в базе все его родители. Поэтому если коммит жив, то можно воссоздать всю ветку (по-сути создать новую ветку). 

Теги удобно использовать для пометки версий нашего продукат, который мы разрабатываем. Тег просто указывает на коммит. Своего рода закладка. 
Пример создания тега с аннотацией. 
git tag -a -m 'Описание тега' v1.0.0 <номер_коммита>

Отменить все незакоммиченные изменения можно командой git reset --hard
`git reset --hard` это жесткий reset т.е. все незакомиченные изменения удаляются безвозвратно.

`git reset --soft @~` Это мягкий reset т.е. мы переходим на указанный коммит, а изменения в рабочей деректории не удаляются они переходят в состояние staged т.е. ожидающие коммита.
Мягкий reset идельно подходит, если после совершения коммита нам нужно что-то в неё поправить. После того как поправили добавляем новые изменения в индекс командой git add и потом выполняем команду `git commit -c ORIG_HEAD`. Флаг `-с` говорит гиту, что нужно взять описание коммита из какого-то коммита. Маленькое `-с` откроет редактор, большое `-С` молча скопирует описание коммита и закоммитит. В нашем случае мы берем описание из коммита который в файле ORIG_HEAD потому что мы только что с него ушли. 

Полезные ссылки:  
[Скринкаст](https://learn.javascript.ru/screencast/git)  
[GitHowTo](https://githowto.com/ru)  
[Документация](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)  

