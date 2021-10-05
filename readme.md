#Шаблонный код контейнера для страта микросервиса платформы WhatsHelp
Предполагается включение в виде git submodule в репозиторий с непосредственной реализацией сервиса, упакованного в модуль.

#package.json
Для получения набора пакетов необходимо предварительно выполнить скрипт
bash ./package.install.sh

Произойдёт склейка json-файлов зависимостей шаблонного контейнера и целевого сервиса, после чего выполнится
```
npm install
```

Для переноса кода модуля в папку Module контейнера и компиляции необходимо запустить
```
grunt
```
#Запуск контейнера
Скрипт-пускатель контейнера конкретного типа сервиса.
Для облегчения залдачи в шаблонном контейнере есть скрипт  
```
./bin/container.sh start|stop
```
который запустит forever и контейнер с именем вида `<hostname>-<type>-<build>`
#Deploy

Сценарий выгрузки сделан на основе shipit
[https://github.com/shipitjs/shipit]

За основу взят workflow shipit-deploy
[https://github.com/shipitjs/shipit-deploy]

Поддерживаются workflow: deploy, rollback и добавлен дополнительный deploy-build.

Внимание!
rollback настроен без удаления предыдущего релиза для возможности анализа проблем по месту!
Перед повторной выгрузкой ОБЯЗАТЕЛЬНО необходимо удалить поломанный релиз, иначе не остановится контейнер "прошлого релиза". 

Внимание!
Workflow deploy-build содержит все шаги типового workflow, кроме создания нового workspace, вытягивания и сборки в нём проекта.
При выполнении попытается загрузить релиз из подготовленного ранее workspace на сервер со всеми сопутствующими шагами.  

Примеры выполнения команд, если shipit утсановлен локально:
```
./node_modules/shipit-cli/bin/shipit test deploy
./node_modules/shipit-cli/bin/shipit test rollback
./node_modules/shipit-cli/bin/shipit test deploy-build
```