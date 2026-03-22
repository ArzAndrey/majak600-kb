# HMI: Структура — Form, Grid, View

## Зависимости
- HMI/01_SYNTAX_BASE.md — базовый синтаксис
- CONFIG/04_CONSTANTS.md — константы

---

## Иерархия

Form (форма)
├── Grid (сетка) — опционально
│   └── View (вид)
│       ├── Item (элемент)
│       ├── Box (залитый прямоугольник)
│       ├── Rect (рамка)
│       └── Line (линия)
└── View (вид) — если нет Grid
    ├── Item
    ├── Box
    ├── Rect
    └── Line

---

## Form (форма)

Корневой элемент экрана. Каждому режиму работы соответствует одна форма.

### Синтаксис
{ form номер_формы (параметры)
    { hotkey ... }
    { view ... }
    { grid ... }
} form

### Номера форм (соответствие режимам)

| Номер | Режим |
|-------|-------|
| 0 | Автомат |
| 1 | Ручной |
| 2 | Выход в ноль |
| 6 | Выход в точку |
| 7 | Преднабор |
| 8 | Фиксированные перемещения |

### Параметры формы

| Параметр | Описание | Пример |
|----------|----------|--------|
| menu | Номер меню, вызываемого при переключении на форму | menu=10 |
| xmax | Ширина доступной области (пиксели) | xmax=665 |
| ymax | Высота доступной области (пиксели) | ymax=600 |

### Hotkey (горячие клавиши)

{ hotkey имя_клавиши
    { pressed команда }
}

Имена клавиш задаются в файле hotkeys.dat.

**Пример:**
{ hotkey KEY_ENTER
    { pressed d_set_ust }
}

### Пример формы

{ form 0 (menu=10, xmax=665, ymax=600)
    { hotkey KEY_ENTER
        { pressed d_set_ust }
    }
    { view ()
        @clock.dat@
        { item # () GAP_H GAP_V yes yes 'Автомат' }
    } view
} form

---

## Grid (сетка)

Позволяет разбить экран на строки и столбцы. Линии сетки не отображаются.

### Синтаксис
{ grid имя_сетки (параметры)
    { view ... }
    { имя_модуля ... }
} grid

### Параметры сетки

| Параметр | Описание | Пример |
|----------|----------|--------|
| grid_rows | Количество строк | grid_rows=31 |
| grid_cols | Количество столбцов | grid_cols=18 |
| grid_border | Цвет рамки | grid_border=MAIN_BGR |
| grid_bgr | Цвет фона сетки | grid_bgr=MAIN_BGR |
| menu | Номер меню при переключении на сетку | menu=10 |
| xmax, ymax | Размер доступной области | xmax=800, ymax=600 |

### Переход между областями сетки

Команда grid_next переключает на следующую выбираемую область (grid_selectable=1).

---

## View (вид)

Выделяет область для отображения элементов.

### Синтаксис
{ view (параметры)
    { item ... }
    { box ... }
    { rect ... }
    { line ... }
} view

### Параметры вида (в форме)

| Параметр | Описание |
|----------|----------|
| xmax, ymax | Размер доступной области |
| bgr | Цвет фона |
| fgr | Цвет текста |
| invbgr | Цвет фона при инверсии |
| invfgr | Цвет текста при инверсии |
| menu | Номер меню при переключении на вид |
| page=0 | Запрещает переход на вид клавишами Page Up/Down |

### Параметры вида (в сетке)

| Параметр | Описание |
|----------|----------|
| windowed=1 | Координаты относительно всего экрана |
| windowed=no | Координаты относительно доступной области |
| grid_x, grid_y | Позиция в ячейках сетки |
| grid_w, grid_h | Размер в ячейках сетки |
| grid_selectable | Возможность перехода на вид (1 — можно) |
| bgr, fgr | Цвета |

### Пример вида в форме

{ view (fgr=AN_C bgr=REG_F invfgr=REG_F invbgr=AN_C)
    @clock.dat@
    { item # () GAP_H GAP_V yes yes 'Автомат' }
} view

### Пример вида в сетке

{ grid g_version ()
    { view (xmax=800 ymax=600 windowed=1
           grid_y=0 grid_h=DEF_GRID_Y
           grid_selectable=0 bgr=MAIN_BGR)
        { item # (height=FONTSIZE_MID invfgr=MAIN_BGR invbgr=CHANNEL0)
            0 GAP_V yes yes 'Версия ПрО'
        }
    } view
} grid

---

## Правила вложенности

1. **Form** может содержать:
   - hotkey (несколько)
   - view (один или несколько)
   - grid (один или несколько)

2. **Grid** может содержать:
   - view (несколько)
   - модули (например, ipperrors)

3. **View** может содержать:
   - item (несколько)
   - box (несколько)
   - rect (несколько)
   - line (несколько)

4. **Вложенные файлы** (@file.dat@) могут содержать любые конструкции и вставляются на место вызова.

---

## Пример полной структуры

{ default
    xmax = 800
    ymax = 600
    fgr = MAIN_TEXT
    bgr = MAIN_BGR
} default

{ form 0 (menu=10, xmax=665, ymax=600)
    { hotkey KEY_ENTER
        { pressed d_set_ust }
    }

    { grid g_main (grid_rows=60 grid_cols=38)
        { view (windowed=1 grid_x=0 grid_y=0 grid_w=38 grid_h=10)
            { item # () GAP_H GAP_V yes yes 'Автомат' }
        } view

        { view (windowed=1 grid_x=0 grid_y=10 grid_w=38 grid_h=50)
            @box_coords.dat@
            @box_f_avt.dat@
            @box_up.dat@
        } view
    } grid
} form