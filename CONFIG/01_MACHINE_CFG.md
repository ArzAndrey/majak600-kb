# CONFIG: Файл machine.cfg

## Зависимости
- HMI/01_SYNTAX_BASE.md — базовый синтаксис
- PLC/01_SYNTAX_BASE.md — базовый синтаксис ПЭС
- DATA/01_D_CELLS.md — D-ячейки
- DATA/04_IO_ADDRESSES.md — входы/выходы

---

## Общее описание

Файл machine.cfg является основным конфигурационным файлом УЧПУ "Маяк-600". Он определяет:
- Тип и название станка
- Кодировку файлов
- Ссылки на файлы интерфейса и ПЭС
- Количество и обозначение координат
- Настройки таблицы инструментов
- Параметры паспортизации

---

## Структура файла

%Global                     ; начало глобального раздела
; параметры глобальной настройки
...

%coords                     ; раздел настройки координат
; параметры координат
...

%tools                      ; раздел настройки таблицы инструментов
; параметры ТИ
...

%trans                      ; раздел трансляции
; параметры трансляции
...

%passport                   ; раздел паспортизации
; параметры паспортизации
...

---

## Параметры раздела %Global

| Параметр | Описание | Пример |
|----------|----------|--------|
| coding | Кодировка файлов (dos, koi8, win) | coding=dos |
| l600_name | Название станка | l600_name="ТПК-125" |
| lipfile | Ссылка на файл описания экранных интерфейсов | lipfile="maya.dat" |
| access | Ссылка на файл уровней доступа | access="access.dat" |
| defUP | Файл УП по умолчанию | defUP="null.iso" |
| defUP_CPP | Файл подпрограмм для постоянных циклов | defUP_CPP="podpr220.ckl" |
| defUP_UPP | Файл пользовательских подпрограмм | defUP_UPP="upp.iso" |
| defUP_USR | Файл пользовательской ПЭС | defUP_USR="tpk125fr.pes" |
| defUP_SYS | Файл системной ПЭС | defUP_SYS="sys.pes" |
| ipp_path | Закрытие каталога с системными файлами | ipp_path="/majak/cnc/env/ISO" |
| ipp_precompile | Использование транслированной ПЭС (0/1) | ipp_precompile=1 |
| char_XYZA | Обозначение координат | char_XYZA="XYZASBCUVW" |
| General_PES | Номер корневой подпрограммы ПЭС | General_PES=219 |
| RT_PES | Номер подпрограммы быстрой автоматики | RT_PES=216 |
| Console | Вариант УЧПУ (0-M600, 1-M610) | Console=0 |
| console_access | Уровень доступа по умолчанию | console_access=0 |
| console_hvaccess | Наличие ключа для наладки | console_hvaccess=0 |
| instant | Начальный адрес таблицы входов | instant=17 |
| servicescfg | Файл настройки передачи данных | servicescfg="udp.cfg" |
| input_timeout | Ожидание входов перед стартом ПЭС (сек) | input_timeout=1 |
| showsyserrors | Показывать системные ошибки при старте | showsyserrors=1 |
| oscnumsample | Размер буфера осциллографа | oscnumsample=72000 |

---

## Параметры раздела %coords

| Параметр | Описание | Пример |
|----------|----------|--------|
| coordnum | Количество действующих координат | coordnum=5 |
| transcycle_start_param | Стартовый адрес параметров для циклов | transcycle_start_param=220 |
| defUP_CPP | Файл с циклами | defUP_CPP="podpr220.clk" |
| graph_places | Плоскости отображения графики | graph_places="XY:XZ:YZ" |
| graph_start_system | Система отображения (0-станок, 1-заготовка) | graph_start_system=0 |
| g131_axis | Координаты для торцевой интерполяции | g131_axis="-X:C:Z" |
| x_point_koef | Множитель для координат с точкой | x_point_koef=1000 |
| x_koef | Множитель для координат без точки | x_koef=1 |
| f_point_koef | Множитель для F с точкой | f_point_koef=1000 |
| f_koef | Множитель для F без точки | f_koef=1 |
| s_point_koef | Множитель для S с точкой | s_point_koef=1000 |
| s_koef | Множитель для S без точки | s_koef=1 |

---

## Параметры раздела %tools

| Параметр | Описание | Пример |
|----------|----------|--------|
| xdim | Количество элементов таблицы инструментов | xdim=16 |
| ydim | Количество значений в одном элементе | ydim=3 |
| DZ | Комментарий к индексу 0 (коррекция по Z) | DZ=Корр.по Z |
| Z | Комментарий к индексу 1 (длина по Z) | Z=Длина по Z |
| R | Комментарий к индексу 2 (радиус) | R=Радиус |

### Для токарного варианта
| Параметр | Описание |
|----------|----------|
| DX | Коррекция по X |
| DZ | Коррекция по Z |
| X | Длина по X |
| Z | Длина по Z |
| P | Ориентация |
| R | Радиус |
| DR | Коррекция радиуса |

---

## Параметры раздела %trans

| Параметр | Описание | Пример |
|----------|----------|--------|
| transcycle_start_param | Стартовый адрес параметров для циклов | transcycle_start_param=220 |
| defUP_CPP | Файл с циклами | defUP_CPP="podpr220.clk" |

---

## Параметры раздела %passport

| Параметр | Описание | Пример |
|----------|----------|--------|
| UPDATE_PERIOD | Период проверки сервера (мс) | UPDATE_PERIOD=91 |
| ZIP | Сжатие данных (0-нет, N-группами) | ZIP=5 |
| STEP | Шаг отправки данных | STEP=1 |
| BEFORE | Таблицы для отправки перед стартом | BEFORE=UP |
| AFTER | Таблицы для отправки после завершения | AFTER= |
| COMMENTARY | Отправка сообщений в паспорт | COMMENTARY=0 |
| KEEPALIVE | Количество неподтверждённых пакетов | KEEPALIVE=12 |
| ARCHIVESDIR | Каталог для архивации паспортов | ARCHIVESDIR=/majak/archive |
| ARCHIVESLIMIT | Минимальный остаток места (Мб) | ARCHIVESLIMIT=5 |
| DATADESCRIPTION | Описание параметра для паспортизации | DATADESCRIPTION N @min=Значение@ @max=Значение@ |

---

## Пример файла machine.cfg (фрезерный вариант, 4 координаты)

%Global
coding=dos
l600_name="ТПК-125"
lipfile="maya-4e.dat"
access="access.dat"
defUP="null.iso"
defUP_CPP="podpr220.ckl"
defUP_UPP="upp.iso"
defUP_USR="tpk125fr.pes"
defUP_SYS="sys.pes"
ipp_path="/majak/cnc/env/ISO"
ipp_precompile=1
char_XYZA="XYZASBCUVW"
General_PES=219
RT_PES=216
Console=0
console_access=0
console_hvaccess=0
instant=17
servicescfg="udp.cfg"
input_timeout=1
showsyserrors=1
oscnumsample=72000

%coords
coordnum=5
transcycle_start_param=220
defUP_CPP="podpr220.clk"
graph_places="XY:XZ:YZ"
graph_start_system=0
x_point_koef=1000
x_koef=1
f_point_koef=1000
f_koef=1
s_point_koef=1000
s_koef=1

%tools
xdim=16
ydim=3
DZ=Корр.по Z
Z=Длина по Z
R=Радиус

%trans
transcycle_start_param=220
defUP_CPP="podpr220.clk"

%passport
UPDATE_PERIOD=91
ZIP=5
STEP=1
BEFORE=UP
COMMENTARY=0
KEEPALIVE=12
ARCHIVESDIR=
ARCHIVESLIMIT=5

---

## Пример файла machine.cfg (токарный вариант)

%Global
coding=dos
l600_name="ТПК-125Т"
lipfile="maya-te.dat"
access="access.dat"
defUP="null.iso"
defUP_CPP="podpr220.ckl"
defUP_UPP="upp.iso"
defUP_USR="tpk125t.pes"
defUP_SYS="sys.pes"
ipp_path="/majak/cnc/env/ISO"
ipp_precompile=1
char_XYZA="XZCS"
General_PES=219
RT_PES=216
Console=0
console_access=0
console_hvaccess=0
instant=17
servicescfg="udp.cfg"
input_timeout=1
showsyserrors=1
oscnumsample=72000

%coords
coordnum=3
transcycle_start_param=220
defUP_CPP="podpr220.clk"
graph_places="XZ"
x_point_koef=1000
x_koef=1
f_point_koef=1000
f_koef=1
s_point_koef=1000
s_koef=1

%tools
xdim=5
ydim=7
DX=Корр.по X
DZ=Корр.по Z
X=Длина по X
Z=Длина по Z
P=Ориентация
R=Радиус
DR=Корр.радиус

%trans
transcycle_start_param=220
defUP_CPP="podpr220.clk"

%passport
UPDATE_PERIOD=91
ZIP=5
STEP=1
BEFORE=UP
COMMENTARY=0
KEEPALIVE=12
ARCHIVESDIR=
ARCHIVESLIMIT=5

---

## Выбор файла интерфейса (lipfile)

| Файл | Тип станка | Кол-во координат | Меню |
|------|------------|------------------|------|
| maya-t.dat | Токарный | 2 + шпиндель | F1-F6 |
| maya-te.dat | Токарный | 2 + шпиндель | F1-F12 |
| maya-3.dat | Фрезерный | 3 + шпиндель | F1-F6 |
| maya-3e.dat | Фрезерный | 3 + шпиндель | F1-F12 |
| maya-4.dat | Фрезерный | 4 + шпиндель | F1-F6 |
| maya-4e.dat | Фрезерный | 4 + шпиндель | F1-F12 |
| maya-5.dat | Фрезерный | 5 + шпиндель | F1-F6 |
| maya-5e.dat | Фрезерный | 5 + шпиндель | F1-F12 |
| maya-6.dat | Фрезерный | 6 + шпиндель | F1-F6 |
| maya-6e.dat | Фрезерный | 6 + шпиндель | F1-F12 |

---

## Рекомендации

1. **Изменения в machine.cfg вступают в силу после перезагрузки УЧПУ**

2. **Параметр ipp_path** используется для защиты системных файлов от просмотра

3. **Параметр ipp_precompile** после отладки ПЭС рекомендуется установить в 1 для защиты исходного кода

4. **Обозначение координат** в char_XYZA определяет порядок битов в масках координат

5. **Для работы с таблицей инструментов** необходимо правильно настроить xdim и ydim

6. **Параметры паспортизации** настраиваются для сбора статистики работы станка