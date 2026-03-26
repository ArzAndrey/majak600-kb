# Инструкция для ИИ (DeepSeek, ChatGPT, Claude)

## База знаний УЧПУ "Маяк-600"
База знаний доступна по raw-ссылкам GitHub. Используйте их для самостоятельной загрузки файлов при работе с HMI и ПЭС.

---
## КРИТИЧЕСКИЕ ПРАВИЛА СИНТАКСИСА (ПЕРЕД ГЕНЕРАЦИЕЙ)
**НАРУШЕНИЕ ЭТИХ ПРАВИЛ ДЕЛАЕТ КОД НЕРАБОТОСПОСОБНЫМ!**

1. **Все конструкции** HMI обрамляются в `{ ... }`
2. **Закрывающая конструкция** повторяет открывающую: `{ form ... } form`, `{ view ... } view`, `{ item ... } item`, `{ menu ... } menu`, `{ grid ... } grid`
3. **Координаты** используют константы `GAP_H`, `GAP_V` из `CONFIG/04_CONSTANTS.md`
4. **Цвета** используют константы из `CONFIG/03_COLORS.md` (например, `MAIN_BGR`, `Error_C`)
5. **Меню** описывается как `{ menu N { f1 ... f14 } } menu`, а не `menu 0 ... horizontal`
6. **Условия** в item пишутся в конце: `{ if m30=255 ... }`, могут быть множественными и использовать логические операторы `&&` (И) и `||` (ИЛИ)
7. **Для статического текста** в item используется источник `#`: `{ item # ... 'Текст' }`
8. **Для вывода значений** используются форматы: `[5]` — целое число, `[+9.3]` — число со знаком, `[+10.3]` — с увеличенной шириной
9. **Для кнопок увеличения/уменьшения** используется синтаксис `+действие -действие` (например, `+ff10 -ff20`, `+I10 -I20`)
10. **Для подсказок и команд** используется синтаксис `!"<команда>"#"<подсказка>"`
11. **Пробелы в строке формата** являются литералами: `'N[5]'` выведет `N00123`, `'N [5]'` выведет `N 00123`
12. **Для условного изменения текста кнопки меню** используется синтаксис `{ text "Текст" { if условие text="Новый текст" } }`
13. **Для объявления переменных HMI** используется `{ variable ИМЯ_ПЕРЕМЕННОЙ }`, присвоение — `%(Имя=значение)`
14. **Для создания действий** используется `{ action ИМЯ_ДЕЙСТВИЯ { ... } } action`, вызов — `{ pressed ИМЯ_ДЕЙСТВИЯ }`
15. **Запрещено** использовать `if ... endif`, `box 0 0 800 600`, `item x y w h` без скобок
---

# ⚠️ КРИТИЧЕСКИЕ ЗАПРЕТЫ (ОБЯЗАТЕЛЬНЫ К ПРОЧТЕНИЮ)

**Никогда не используй следующие конструкции — они НЕ РАБОТАЮТ в УЧПУ "Маяк-600":**

| ЗАПРЕЩЕНО | ПОЧЕМУ | ЧТО ИСПОЛЬЗОВАТЬ |
|-----------|--------|------------------|
| `form 0 "Автомат"` | Неверный синтаксис | `{ form 0 ... } form` |
| `size 800 600` | Неверный параметр | `xmax=800, ymax=600` |
| `bgcolor` | Неверное имя параметра | `bgr = MAIN_BGR` |
| `{ grid rows 7 cols 1 }` | Неверный синтаксис | `{ grid name (grid_rows=7, grid_cols=1) } grid` |
| `type text` | Нет такого атрибута | Используй `{ item # ... }` для статического текста |
| `if ... else ... endif` | Неверный синтаксис условий | `{ if ... }` в конце item, несколько условий подряд |
| `{ menu 0 { f1 "ПУСК" } }` | Неверный синтаксис меню | `{ menu 0 { f1 { text "ПУСК" } { pressed ... } } } menu` |
| `xoffset`, `yoffset`, `row`, `col` | Неподдерживаемые атрибуты | Только абсолютные координаты `x` и `y` |
| `field`, `label`, `input` | Конструкции из других ЧПУ | Только `{ item ... }` с атрибутами |
| `menu horizontal`, `menu vertical` | Неверный синтаксис меню | `{ menu N { f1 ... f14 } } menu` (всегда вертикальное) |
| `{ text "Текст" } { if условие text="Новый" }` | Условие вне блока text | `{ text "Текст" { if условие text="Новый" } }` |
| `{ if (p10&1) }` | Неверная проверка бита | `{ if p10&1#0 }` |

**Перед генерацией любого HMI кода обязательно загрузи файлы:**
- `HMI/01_SYNTAX_BASE.md`
- `HMI/02_STRUCTURE.md`
- `HMI/03_ELEMENTS.md`
- `HMI/04_MENU.md`
- `HMI/05_FORMATS.md`
- `HMI/06_CONDITIONS.md`
- `HMI/07_COMMANDS.md`
- `CONFIG/03_COLORS.md`
- `CONFIG/04_CONSTANTS.md`


## Базовые файлы (загружать при старте работы)

| Файл | Ссылка |
|------|--------|
| Индекс | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/00_INDEX.md |
| Краткая шпаргалка | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/QUICK_REFERENCE.md |

### HMI
| Файл | Ссылка |
|------|--------|
| Базовый синтаксис | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/01_SYNTAX_BASE.md |
| Структура (Form/Grid/View) | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/02_STRUCTURE.md |
| Элементы (Item/Box/Rect/Line) | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/03_ELEMENTS.md |
| Меню | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/04_MENU.md |
| Форматы вывода | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/05_FORMATS.md |
| Условия if | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/06_CONDITIONS.md |
| Команды +dpl#stop | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/HMI/07_COMMANDS.md |

### ПЭС
| Файл | Ссылка |
|------|--------|
| Базовый синтаксис | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/01_SYNTAX_BASE.md |
| Переменные | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/02_VARIABLES.md |
| Операторы | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/03_OPERATORS.md |
| Функции | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/04_FUNCTIONS.md |
| Объекты | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/05_OBJECTS.md |
| Подпрограммы | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/06_SUBROUTINES.md |
| Технологические константы | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/07_TECH_CONST.md |
| Ошибки и сообщения | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/PLC/08_ERRORS.md |

### Данные
| Файл | Ссылка |
|------|--------|
| D-ячейки | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/DATA/01_D_CELLS.md |
| Параметры R0-R599 | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/DATA/02_PARAMETERS.md |
| Массивы I, M, P | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/DATA/03_ARRAYS.md |
| Входы/выходы A | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/DATA/04_IO_ADDRESSES.md |
| Таблица инструментов T | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/DATA/05_TOOL_TABLE.md |

### Конфигурация
| Файл | Ссылка |
|------|--------|
| machine.cfg | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/CONFIG/01_MACHINE_CFG.md |
| hash_d.cfg | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/CONFIG/02_HASH_D_CFG.md |
| Цвета colors.dat | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/CONFIG/03_COLORS.md |
| Константы defaults.dat | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/CONFIG/04_CONSTANTS.md |
| Конфигуратор оборудования | https://raw.githubusercontent.com/ArzAndrey/majak600-kb/main/CONFIG/05_CONFIGURATOR.md |

---

## Рабочий процесс для ИИ

### 1. Начало работы в новом чате
При получении задачи на генерацию HMI или ПЭС кода:

1. Загрузи базовые файлы:
   - `HMI/01_SYNTAX_BASE.md` — для HMI задач
   - `HMI/03_ELEMENTS.md` — для понимания элементов
   - `HMI/04_MENU.md` — для работы с меню
   - `HMI/05_FORMATS.md` — для форматирования вывода
   - `HMI/06_CONDITIONS.md` — для условных конструкций
   - `PLC/01_SYNTAX_BASE.md` — для ПЭС задач
   - `CONFIG/03_COLORS.md` — цветовые константы
   - `CONFIG/04_CONSTANTS.md` — числовые константы

2. При необходимости загрузи детальные файлы:
   - Для HMI: `HMI/02_STRUCTURE.md`, `HMI/07_COMMANDS.md`
   - Для ПЭС: `PLC/02_VARIABLES.md`, `PLC/03_OPERATORS.md`

3. Используй `QUICK_REFERENCE.md` для быстрых справок

### 2. Генерация кода по картинке
1. Проанализируй изображение экрана
2. Определи тип экрана (Автомат, Ручной и т.д.) по номеру form
3. Определи координаты элементов (используй константы GAP_H, GAP_V, BOX_X и т.д.)
4. Для статического текста используй `{ item # ... 'Текст' }`
5. Для вывода значений используй соответствующие форматы: `[5]` для целых, `[+9.3]` для вещественных
6. Для редактируемых полей добавь кнопки `+действие -действие`
7. Для условного форматирования добавь блоки `{ if ... }` в конце item
8. Сгенерируй код с использованием констант и синтаксиса из базы

### 3. Формат ответа
Предоставляй код в формате:
- Полный путь к файлу (например, `form_avt.hmi`)
- Содержимое файла в отдельном блоке с возможностью копирования

---

## Основные правила синтаксиса HMI (быстрая справка)

### Переменные HMI
{ variable PRNvar }
{ variable Nvar }
{ variable Xvar }
%(Nvar=Nvar+1)
{ if Nvar=NAN format='N ---' }

### Item для статического текста
{ item # (height=FONTSIZE_MID fgr=CHANNEL0) 0 GAP_V yes yes ' Диалог ' }

### Item для вывода значения
{ item d77 (fgr=B3 bgr=B6) BOX_X+BOX_GAP_H+28 BOX_S_Y+BOX_GAP_V+HEIGHT_MID-1 no yes '[5]' }

### Item с кнопками увеличения/уменьшения
{ item i2 (height=FONTSIZE_SMALL fgr=B1 bgr=B6 invfgr=B1 invbgr=B5)
    BOX_X+BOX_GAP_H+80+10 BOX_S_Y+BOX_GAP_V+HEIGHT_MID+0.5+4 no yes '[3]%' +ff10 -ff20 !"<ust>PF"#"<ust>" }

### Item с условным форматированием
{ item # (invbgr=Error_C invfgr=MAIN_BGR width=106)
    456 16 yes no ''
    { if m30=0 invfgr=MAIN_BGR format=' ЧПУ: Авария  ' }
    { if m30=255 invbgr=GREEN format=' ЧПУ: Готово  ' } }

### Меню с условным изменением текста
{ menu 10001
    { f1 { text "Записать$в УП" { if PRNvar=1 text="Записать$кадр" } } 
        { pressed inpop c " N=Nvar G=Gvar X=Xvar. Y=Yvar. F=Fvar" cc blockgather inpush quit } }
    { f6 { text "Возврат" } { pressed indropall quit } }
    { f15 { text "Возврат" } { pressed indropall quit } }
} menu

### Действие (action)
{ action xvar_set_sel c "<?xvar>" cc parse ifok 3 "Ошибка!" errormsg 0 xvar_set }

### Dlgdata (табличное меню)
{ dlgdata menu=10000 cfgfile="ciklt.dat" columns=1 }

---

## Структура репозитория

majak600-kb/
├── 00_INDEX.md
├── QUICK_REFERENCE.md
├── AI_INSTRUCTIONS.md          (этот файл)
├── HMI/
│   ├── 01_SYNTAX_BASE.md
│   ├── 02_STRUCTURE.md
│   ├── 03_ELEMENTS.md
│   ├── 04_MENU.md
│   ├── 05_FORMATS.md
│   ├── 06_CONDITIONS.md
│   └── 07_COMMANDS.md
├── PLC/
│   ├── 01_SYNTAX_BASE.md
│   ├── 02_VARIABLES.md
│   ├── 03_OPERATORS.md
│   ├── 04_FUNCTIONS.md
│   ├── 05_OBJECTS.md
│   ├── 06_SUBROUTINES.md
│   ├── 07_TECH_CONST.md
│   └── 08_ERRORS.md
├── DATA/
│   ├── 01_D_CELLS.md
│   ├── 02_PARAMETERS.md
│   ├── 03_ARRAYS.md
│   ├── 04_IO_ADDRESSES.md
│   └── 05_TOOL_TABLE.md
└── CONFIG/
    ├── 01_MACHINE_CFG.md
    ├── 02_HASH_D_CFG.md
    ├── 03_COLORS.md
    ├── 04_CONSTANTS.md
    └── 05_CONFIGURATOR.md

---

## Версия
3.0 — март 2026 (обновлено: добавлены правила работы с пробелами в форматах, условное изменение текста меню, переменные HMI, действия action, элемент dlgdata, вложенные view)
