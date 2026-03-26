# HMI: Команды (действия при нажатии клавиш)

## Зависимости
- HMI/01_SYNTAX_BASE.md — базовый синтаксис
- HMI/03_ELEMENTS.md — элемент item
- HMI/04_MENU.md — меню

---

## Общее описание

Команды в HMI позволяют:
- Изменять значения переменных при нажатии клавиш → и ←
- Выполнять действия при нажатии/отжатии клавиш меню
- Переключать меню, сетки, модули

Команды описываются в двух местах:
1. В элементе item — команды +aa#bb -cc#dd
2. В блоке `{ action ... }` — определение макросов (может быть в отдельном файле или в основном файле HMI)
3. В меню — команды pressed и released

---

## Команды в элементе item

### Синтаксис
{ item имя_элемента (параметры) x y inv always 'текст' +aa#bb -cc#dd }

### Структура команды
+aa#bb -cc#dd

где:
- +aa#bb — увеличение (aa — команда при нажатии →, bb — при отжатии)
- -cc#dd — уменьшение (cc — команда при нажатии ←, dd — при отжатии)

### Принцип работы
1. Навести курсор (инверсное изображение) на элемент с помощью клавиш ↑ или ↓
2. Нажать → для выполнения команды +aa
3. Отжать → для выполнения команды bb
4. Нажать ← для выполнения команды -cc
5. Отжать ← для выполнения команды dd

---

## Определение команд (action)

### Синтаксис
{ action имя_команды действие }

### Где может находиться
- В отдельном файле (action.dat, actions.dat, cmd.dat, macro.dat и т.д.)
- В основном HMI-файле перед form
- В подключаемых файлах через !"файл.dat"

### Условные действия (ifok / ifnotok)
Синтаксис: %=условие ifok число_команд команда_если_ложь 0 команда_если_истина

Пример:
{ action ff10 %=(d9+5)>200 ifok 2 d9=d9+5 0 d9=200 0 }

### Действия
| Действие | Описание | Пример |
|----------|----------|--------|
| переменная = значение | Присвоить значение | d65 = 1 |
| переменная = выражение | Вычислить и присвоить | d70 = d70 + 1 |
| call имя | Вызвать подпрограмму | call set_mode |
| grid_next | Переключить область сетки | grid_next |
| mm | Переключить меню | mm |

---

## Примеры из реальных проектов

### 1. Битовые команды для управления осями
{ action I10 d317=1 }
{ action I11 d317=2 }
{ action I12 d317=4 }
{ action I13 d317=8 }
{ action I14 d317=16 }
{ action I15 d317=32 }
{ action I16 d317=64 }
{ action StopIt d317=0 d318=0 }
{ action I20 d318=1 }
{ action I21 d318=2 }
{ action I22 d318=4 }
{ action I23 d318=8 }
{ action I24 d318=16 }
{ action I25 d318=32 }
{ action I26 d318=64 }

### 2. Команды для управления режимами
{ action N10 d301=1 }
{ action N20 d302=1 }
{ action StopNt d301=0 d302=0 }

### 3. Команды изменения значений с ограничением
; %F в ручном режиме
{ action ff1 %=(d12+5)>200 ifok 2 d12=d12+5 0 d12=200 0 }
{ action ff2 %=(d12-5)<0   ifok 2 d12=d12-5 0 d12=0   0 }

; %F в автоматическом режиме
{ action ff10 %=(d9+5)>200 ifok 2 d9=d9+5 0 d9=200 0 } 
{ action ff20 %=(d9-5)<0   ifok 2 d9=d9-5 0 d9=0   0 }

; %S
{ action ps1 %=(d62+5)>200 ifok 2 d62=d62+5 0 d62=200 }
{ action ps2 %=(d62-5)<0   ifok 2 d62=d62-5 0 d62=0   }

; %БХ (максимум 100%)
{ action bh1 %=(d33+5)>100 ifok 2 d33=d33+5 0 d33=100 }
{ action bh2 %=(d33-5)<0   ifok 2 d33=d33-5 0 d33=0   }

### 4. Дискретный выбор значений (перебор)
{ action df1 %=d385>=1    ifok 2  d385=1    0 
             %=d385>=4    ifok 2  d385=4    0
             %=d385>=30   ifok 2  d385=30   0
             %=d385>=50   ifok 2  d385=50   0
             %=d385>=100  ifok 2  d385=100  0
             %=d385>=150  ifok 2  d385=150  0
             %=d385>=240  ifok 2  d385=240  0
             %=d385>=500  ifok 2  d385=500  0
             %=d385>=800  ifok 2  d385=800  0
             %=d385>=1200 ifok 2  d385=1200 0
                                  d23=d23|2    }

### 5. Команды для наборной строки
{action set_ust
  { F  %=(x<0)||(x>24000) ifok 3 %i23=x d23=d23&~2 0 
       "Неверное значение F"  errormsg }
  { S  %=(d63#1)||(x<d50)||(x>d51) ifok 2 d1=x 0 
       %=(d63#2)||(x<d52)||(x>d53) ifok 2 d1=x 0
       %=(d63#3)||(x<d54)||(x>d55) ifok 2 d1=x 0
       %=(d63#4)||(x<d56)||(x>d57) ifok 2 d1=x 0
       "Неверное значение S"  errormsg }
  { PF  %=(x<1)||(x>200)  ifok 2 %d9=x 0 "Неверное значение %F"   errormsg }
  { PS  %=(x<0)||(x>200)  ifok 2 %d62=x 0 "Неверное значение %S"   errormsg }
  { PFN %=(x<0)||(x>200)  ifok 2 %d12=x 0 "Неверное значение %F"   errormsg }
  { PBH %=(x<0)||(x>100)  ifok 2 %d33=x 0 "Неверное значение %БХ"   errormsg }
}
{action d_set_ust c "<?ust>" cc parse ifok 3 "Ошибка!" errormsg 0 set_ust }

---

## Примеры item с командами

### Пример 1: Простое изменение значения
{ item d65 (height=30) 516 66 no yes 'Позиция [1]' +dpl -dmn }

### Пример 2: Битовые команды
{ item d317 (height=20) 260 65 no yes '+I10#StopNt -I20#StopNt' }

### Пример 3: Инкремент и декремент
{ item d70 (height=30) 100 50 no yes 'Значение [3]' +inc -dec }

### Пример 4: Вызов подпрограммы
{ item # (height=30) 100 50 no yes 'Сохранить' +save_data }

### Пример 5: Специальный синтаксис !"<ust>..."
Вызывает действие с передачей значения из наборной строки (<ust>).
{ item i23 (height=20) 260 65 no yes '[5]' +df1 -df2 !"<ust>F"#"<ust>" }

---

## Команды в меню (pressed / released)

### Синтаксис в меню
{ f1
    { text "Текст" }
    { pressed команда }
    { released команда }
}

### Команды pressed/released

#### Переключение меню
| Команда | Описание |
|---------|----------|
| mm | Переключение на предыдущее меню |
| mmN | Переключение на меню с номером N |
| mm10 | Переключение на меню 10 |

#### Вызов сетки или модуля
| Команда | Описание |
|---------|----------|
| pressed имя_сетки | Вызов сетки |
| pressed имя_модуля | Вызов модуля |

#### Изменение переменных
| Команда | Описание | Пример |
|---------|----------|--------|
| переменная = значение | Присвоить значение | d130=3 |
| переменная = выражение | Вычислить и присвоить | d130=d130+1 |

#### Другие команды
| Команда | Описание |
|---------|----------|
| inputkill | Очистить наборную строку |
| next | Переход к следующему |
| execute | Выполнить |

---

## Примеры меню с командами

### Пример 1: Простое меню с переключением
{ menu 20 ; Режим Ручной
    { f1 { text "Шпиндель" } { pressed mm201 } }
    { f2 { text "СОЖ" } { pressed mm202 } }
    { f3 { text "Сброс" } { pressed s34 } }
    { f6 { text "Возврат" } { pressed mm } }
} menu

### Пример 2: Меню с изменением переменных
{ menu 201 ; Управление шпинделем
    { f1 { text "M3" } { pressed d130=3 } }
    { f2 { text "M4" } { pressed d130=4 } }
    { f3 { text "M5" } { pressed d130=5 } }
    { f6 { text "Возврат" } { pressed mm } }
} menu

### Пример 3: Меню с условиями ifok
{ menu 1070
    { f8
        { text "Пуск записи$" }
        { text "1" (fgr=CPARAM3) { if d404=0 fgr=MENU_FGR } }
        { text " 2" (fgr=CPARAM3) { if d404=1 fgr=MENU_FGR } }
        { text " 3" (fgr=CPARAM3) { if d404=2 fgr=MENU_FGR } }
        { pressed %=d404#0 ifok 3 "1" start_log 0
                  %=d404#1 ifok 3 "2" start_log 0
                  %=d404#2 ifok 3 "3" start_log 0 }
    }
} menu

### Пример 4: Меню с удержанием (pressed/released)
{ menu 80
    { f1 
        { text "Перемещение-" { if (d318#0)&&(d318#8) fgr=WARNING } } 
        { pressed d318=d406 } 
        { released d318=0 }   
    }
    { f2 
        { text "Перемещение+" { if (d317#0)&&(d317#8) fgr=WARNING } } 
        { pressed d317=d406 } 
        { released d317=0 }   
    }  
    { f3 
        { text "Выбор оси" }
        { text " &0 " { if d406=1 fgr=WARNING } { if IND_KRD0=0 fgr=BG_MAIN } } 
        { text " &1 " { if d406=2 fgr=WARNING } { if IND_KRD1=0 fgr=BG_MAIN } } 
        { text " &2 " { if d406=4 fgr=WARNING } { if IND_KRD2=0 fgr=BG_MAIN } } 
        { text " &3 " { if d406=8 fgr=WARNING } { if IND_KRD3=0 fgr=BG_MAIN } } 
        { text " &4 " { if d406=16 fgr=WARNING } { if IND_KRD4=0 fgr=BG_MAIN } } 
        { text " &5 " { if d406=32 fgr=WARNING } { if IND_KRD5=0 fgr=BG_MAIN } } 			
        { pressed %=d406=4 ifok 2 d406=d406*2 0 d406=1 }
    }
} menu

---

## Команда grid_next

### Назначение
Переключение на следующую выбираемую область в сетке.

### Условия использования
- Область должна иметь параметр grid_selectable=1
- Переключение происходит в порядке возрастания grid_x, grid_y

### Пример
В сетке:
{ grid g_main (grid_rows=10 grid_cols=10)
    { view (grid_x=0 grid_y=0 grid_w=5 grid_h=5 grid_selectable=1)
        { item # () 0 0 yes yes 'Область 1' }
    } view
    { view (grid_x=5 grid_y=0 grid_w=5 grid_h=5 grid_selectable=1)
        { item # () 0 0 yes yes 'Область 2' }
    } view
} grid

В меню:
{ menu 100
    { f1 { text "Далее" } { pressed grid_next } }
} menu

---

## Команды в hotkey

### Синтаксис
{ hotkey имя_клавиши
    { pressed команда }
}

### Пример из реального проекта
{ hotkey KEY_ENTER
    { pressed d_set_ust }
}
{ hotkey KEY_CTRL_Q
    { pressed quit }
}
{ hotkey KEY_ALT_S
    { pressed %=(m33>1) ifok 2 g_access 0 g_mfuncs }
}
{ hotkey KEY_CTRL_G
    { pressed s17 g_graph_show ifok 1 mm1130 }
}

---

## Полный пример

### Определение команд (может быть в action.dat или в основном файле)
{ action dpl d65 = 1 }
{ action dmn d65 = 2 }
{ action inc_f d40 = d40 + 10 }
{ action dec_f d40 = d40 - 10 }
{ action I10 d317=1 }
{ action I20 d318=1 }
{ action StopIt d317=0 d318=0 }
{ action call_reset call reset_proc }
{action set_ust
  { F  %=(x<0)||(x>24000) ifok 3 %i23=x d23=d23&~2 0 
       "Неверное значение F"  errormsg }
}
{action d_set_ust c "<?ust>" cc parse ifok 3 "Ошибка!" errormsg 0 set_ust }

### Интерфейс
{ default
    xmax = 800
    ymax = 600
    fgr = MAIN_TEXT
    bgr = MAIN_BGR
} default

{ form 1 (menu=10, xmax=665, ymax=600)
    
    { view ()
        ; Элемент с изменением значения
        { item d65 (height=FONTSIZE_MID width=50)
            GAP_H GAP_V no yes 'Позиция [1]' +dpl -dmn }
        
        ; Элемент с инкрементом/декрементом скорости
        { item d40 (height=FONTSIZE_MID width=80)
            GAP_H GAP_V+HEIGHT_MID no yes 'F [4]' +inc_f -dec_f }
        
        ; Элемент с битовой командой
        { item d317 (height=FONTSIZE_MID)
            GAP_H GAP_V+HEIGHT_MID*2 no yes 'Ось X' +I10#StopIt -I20#StopIt }
        
        ; Элемент с вызовом подпрограммы
        { item # (height=FONTSIZE_MID)
            GAP_H GAP_V+HEIGHT_MID*3 no yes 'Сброс' +call_reset }
        
        ; Элемент с вводом из наборной строки
        { item i23 (height=FONTSIZE_MID width=80)
            GAP_H GAP_V+HEIGHT_MID*4 no yes 'F [4]' +df1 -df2 !"<ust>F"#"<ust>" }
    } view
    
} form

{ menu 10
    { f1 { text "Настройки" } { pressed mm20 } }
    { f2 { text "Диагностика" } { pressed mm30 } }
    { f6 { text "Возврат" } { pressed mm } }
} menu

{ menu 20
    { f1 { text "Скорость+" } { pressed d40 = d40 + 10 } }
    { f2 { text "Скорость-" } { pressed d40 = d40 - 10 } }
    { f6 { text "Назад" } { pressed mm } }
} menu

---

## Правила использования команд

1. Имена команд должны соответствовать: в item (+имя#стоп) и в action { action имя ... }
2. В item команды указываются после текста, перед условиями
3. pressed и released могут использоваться вместе или по отдельности
4. Команды в меню выполняются мгновенно при нажатии/отжатии
5. Команды в item требуют наличия курсора на элементе
6. Для передачи значения из наборной строки используется синтаксис !"<ust>имя_действия"#"<ust>"

---

## Типичные применения

| Ситуация | Решение |
|----------|---------|
| Изменение числового параметра | +inc -dec с шагом |
| Выбор режима из нескольких | +mode1 +mode2 с установкой значений |
| Переключение да/нет | +toggle с условием ifok |
| Управление осями (битовые маски) | +I10#StopNt -I20#StopNt |
| Вызов подпрограммы | +call с action |
| Навигация по страницам | pressed grid_next / grid_prev |
| Смена меню | pressed mmN |
| Очистка ввода | pressed inputkill |
| Ввод значения с клавиатуры | !"<ust>F"#"<ust>" |

---
