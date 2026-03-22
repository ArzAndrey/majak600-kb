# Инструкция для ИИ (DeepSeek, ChatGPT, Claude)

## База знаний УЧПУ "Маяк-600"

База знаний доступна по raw-ссылкам GitHub. Используйте их для самостоятельной загрузки файлов при работе с HMI и ПЭС.

---
## КРИТИЧЕСКИЕ ПРАВИЛА СИНТАКСИСА (ПЕРЕД ГЕНЕРАЦИЕЙ)
**НАРУШЕНИЕ ЭТИХ ПРАВИЛ ДЕЛАЕТ КОД НЕРАБОТОСПОСОБНЫМ!**

1. **Все конструкции** HMI обрамляются в `{ ... }`
2. **Закрывающая конструкция** повторяет открывающую: `{ form ... } form`, `{ view ... } view`, `{ item ... }`
3. **Координаты** используют константы `GAP_H`, `GAP_V` из `CONFIG/04_CONSTANTS.md`
4. **Цвета** используют константы из `CONFIG/03_COLORS.md` (например, `MAIN_BGR`, `Error_C`)
5. **Меню** описывается как `{ menu N { f1 ... f14 } } menu`, а не `menu 0 ... horizontal`
6. **Условия** в item пишутся в конце: `{ if m30=255 ... }`
7. **Запрещено** использовать `if ... endif`, `box 0 0 800 600`, `item x y w h` без скобок
---

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
   - HMI/01_SYNTAX_BASE.md — для HMI задач
   - PLC/01_SYNTAX_BASE.md — для ПЭС задач
   - CONFIG/03_COLORS.md — цветовые константы
   - CONFIG/04_CONSTANTS.md — числовые константы

2. При необходимости загрузи детальные файлы:
   - Для HMI: HMI/02_STRUCTURE.md, HMI/03_ELEMENTS.md, HMI/04_MENU.md
   - Для ПЭС: PLC/02_VARIABLES.md, PLC/03_OPERATORS.md

3. Используй QUICK_REFERENCE.md для быстрых справок

### 2. Генерация кода по картинке
1. Проанализируй изображение экрана
2. Определи тип экрана (Автомат, Ручной и т.д.) по номеру form
3. Определи координаты элементов
4. Сгенерируй код с использованием констант и синтаксиса из базы

### 3. Формат ответа
Предоставляй код в формате:
- Полный путь к файлу (например, form_avt.hmi)
- Содержимое файла в отдельном блоке

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
1.0 — март 2026