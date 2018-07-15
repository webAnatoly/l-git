Это репозитарий git для тренировки работы с системой контроля версий git :)

Глобальный конфиг лежит в домашней дериктории пользователя ~/.gitconfig 

.git/config - локальный конфиг конкретного проекта

git config --global core.pager 'less -RFX' - этой командой устанавливаем, чтобы гит по умолчанию использовал программу less для показа справки. Он её и так использует, но без параметров -RFX

git show --pretty=fuller - показать подробно коммит с выведением автора кода, и того кто сделал коммит.

git add . - команда git add c точкой передает в git весь текущий каталог. 

git config --global core.excludesFile ~/.gitignore
эта команда добавляет в глобальный конфиг строку о том, что существует глобальный 
файл .gitignore в директории пользователя. Глобальный .gitignore используется для игнора например вспомогательных файлов IDE, например WebStorm создает каталог .idea для своих вспомогательных файлов. Вот её можно добавлять в глобальный .gitignore 

