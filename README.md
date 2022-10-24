# Тестовое задание на позицию младшего разработчика С++

## Навигация
- #### [Описание](#description)
- #### [Как начать](#how_to_start)
- #### [Как собрать проект](#how_to_build)
- #### [Запуск приложения](#start_app)
- #### [Небольшое замечание по проекту](#remark)
- #### [Скриншоты работы приложений](#screenshoots)



<a name="description"/>

## Описание

Необходимо разработать простую программную систему из двух
приложений на базе Qt Framework (С++) для мониторинга регулярного
непрерывного сигнала с воображаемого источника


Приложение 1 - Эмулятор (сервер)
Производит непрерывную генерацию указанного сигнала на программном
уровне и обеспечивает его передачу TCP\IP транспортом по произвольному
протоколу.


Приложение 2 - Регистратор (клиент)
Принимает и отображает в произвольной форме сигнал, посланный
Эмулятором, с применением встроенной Qt функциональности.
Это может быть текстовое или графическое представление.
- Необходимо построить интуитивно понятную интерактивность оконного
интерфейса с динамическим отображением изменяющегося сигнала в
центральном представлении.
- Приветствуется графическое представление. Для него требуется набор
стандартных инструментов управления просмотром (масштабирование, управление
отдельно амплитудой и периодом сигнала, выбор толщины сигнальной кривой и
цвета).
- Приложение должно быть представлено в форме Qt проекта и исходников

<a name="how_to_start"/>

## Как начать

1. Склонируйте репозиторий на ваш компьютер:

    `git clone --recurse-submodules -j2 https://github.com/vglaznev/inobitec-entry-task.git`

    Если вы склонировали без флага `--recurse-submodules` :

    `cd inobitec-entry-task`

    `git submodule update --init`

2.  Войдите в корень проекта:

    `cd inobitec-entry-task`

3. В директориях `inobitec-entry-task-client` и `inobitec-entry-task-server` будут находиться исходники приложений Регистратора (клиента) и Эмулятора (сервера).

<a name="how_to_build"/>

## Как собрать проект

Шаги сборки приложений клиента и сервера совпадают, поэтому опишу процесс сборки только приложения клиента. Для приложения сервера выполнить аналогичные шаги.

1.  В корне проекта создайте директорию для сборки:

    `mkdir build-client`

2. Перейдите в созданную директорию:

    `cd build-client`

3. Запустите qmake для генерации makefil'а для последующей сборки. В качестве параметров передайте путь к pro файлу проекта, который содержит информацию о подключаемых модулях таких, как, network, widgets и т.п. и информацию о сборке, и переменную qmakespec (после флага -spec), которая содержит описание платформы и компилятора, под которую происходит сборка (в моем случае сборка под Windows, компилятор g++).

    `qmake.exe ../client/client.pro -spec win32-g++`

    Три важных замечания, выявленных в неприятных попытках собрать через консоль, а не через  QtCreator:

    * Если местоположение qmake.exe не прописано в переменных среды (PATH), используйте полный путь;

    * При сборке приложения нужно использовать тот же компилятор, которой использовался для сборки исходников Qt; Если собираетесь делать сборку приложения под другую платформу/компилятор, необходимо пересобрать исходники.

    * Вытекающее из второго пункта: внимательно подойдите к переменным среды (PATH). Если у вас в PATH несколько местоположений к различным версиям компилятора (например, mingw g++, strawberry g++), то следует расположить выше тот, который использовался при сборке Qt. В случае, если он отсутствует в PATH, необходимо его добавить.


4. Соберите проект утилитой make (в моем случае mingw32-make):

    `mingw32-make.exe`

    Дополнительно можно добавить флаг `-j<n>`, где n - число, не превышающее число потоков вашего процессора, для запуска в параллельном режиме для ускорения процесса сборки.

5. (Опционально) Запустите этап очистки объектных файлов, указав в качесте цели для `make` `clean`:

    `mingw32-make.exe clean`

6. Перейдите в директорию `release`, там будет находиться исполняемый файл приложения:

    `cd release`
    
<a name="start_app"/>

## Запуск приложения

Перед запуском приложения необходимо поместить dll файлы в директорию с исполняемым файлом. Для этого запустите утилиту `windeployqt`, указав путь к исполняемому файлу. Если путь к ней указан в PATH и вы находитесь в директории с приложением:

` windeploqt . `

Запустите приложение двойным кликом или командой `client.exe` через консоль.

<a name="remark"/>

## Небольшое замечание по проекту

Приложение клиента я разбил на "GUI Thread" и "Worker Thread", благодаря чему нет искажений на графике при взаимодействии с ui клиента из-за пропуска поступающей информации. Но совсем забыл сделать то же самое с сервером. Из-за чего при перемещении окна сервера происходят искажения на графике (делая то же самое с окном клиента, такого не будет). Но не стал уже исправлять из-за вышедших сроков. 

<a name="screenshoots"/>

## Скриншоты работы приложений

Клиента:

![alt text](img/client.png?raw=true)

Сервера:

![alt text](img/server.png?raw=true)
