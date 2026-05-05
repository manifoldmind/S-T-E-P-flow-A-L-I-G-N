## 🗺️ Текущая карта проекта

В репозитории три типа контента:

**Зрелые, стабильные артефакты:**
- `DONE.ipynb` — основной рабочий ноутбук с финальными результатами
- `ALIGN.md` — методология с «огранкой» терминов
- `STEP.md` — полный отчёт по лабораторной работе
- `curr_state.md` — #CHILLOUT-анализ и план дальнейших шагов

**Учебные / теоретические заметки:**
- `DEEP — Теория по accuracy.md` — объяснение метрики
- `DEEP — Теория по кросс-валидации.md` — объяснение кросс-валидации
- `DEEP — Алгоритм псевдослучайных чисел.md` — заметки о random_state

**Экспериментальная / временная зона:**
- `__sandbox.ipynb` — черновик-песочница (1516 строк)
- `tmp.md` — временные заметки по инструментам для Task 3

Плюс техника: `.obsidian/`, `.vscode/`, `requirements.txt`.

Проект находится в **активной точке развития**: Task 1 и 2 выполнены, Task 3 (кросс-валидация) запланирована, но не реализована. Ветки помогут войти в эту задачу без риска сломать уже работающий код.

---

## 🔒 Шаг 0: Страховка (тег)
Создаём «точку возврата», чтобы в любой момент можно было восстановить сегодняшнее состояние **точь-в-точь**.

```bash
git checkout main
git tag backup-before-mindground-2026-05-05
git push origin backup-before-mindground-2026-05-05
```

Теперь точка сохранена на GitHub. Вернуться можно командой:
```bash
git checkout backup-before-mindground-2026-05-05
```

---

## 🌱 Шаг 1: Создаём `mindground` (полная копия текущего main)
Эта ветка будет твоей «живой картографией» — все эксперименты, черновики и временные файлы останутся здесь навсегда.

```bash
git checkout main
git checkout -b mindground
git push -u origin mindground
```

**Состояние `mindground`:** абсолютно всё, что есть в `main` сейчас (`__sandbox.ipynb`, `tmp.md`, `DEEP-файлы`, `curr state.md`, `DONE.ipynb`, `ALIGN.md`, `STEP.md` и пр.).

---

## 🛠️ Шаг 2: Создаём `made` (стабильный прототип)
Ветка для проверенных, но ещё не финальных артефактов + теоретическая база, которая помогает дорабатывать проект.

```bash
git checkout main
git checkout -b made
```

Теперь удалим из неё чисто черновые файлы, которые не относятся к стабильному состоянию:

```bash
git rm __sandbox.ipynb
git rm tmp.md
git rm "Pasted image 20260426211115.png"   # если он есть в репозитории
git commit -m "clean: удалить черновики, оставить стабильные артефакты в made"
git push -u origin made
```

**Состояние `made`:**  
- Остаются: `DONE.ipynb`, `ALIGN.md`, `STEP.md`, `README.md`, `requirements.txt`, а также три `DEEP — ...`, `curr state.md`, `.obsidian/`, `.vscode/`.  
- Удалены: `__sandbox.ipynb`, `tmp.md`, картинка (если была).  

Это соответствует статусу `#MAKED` — реализовано, работает, но может дорабатываться.

---

## 🚀 Шаг 3: Превращаем `main` в `proddone` и очищаем до релиза
Ветка `proddone` — только готовый продукт, который не стыдно показать. Сначала переименовываем текущий `main`.

```bash
git checkout main
git branch -m main proddone
git push -u origin proddone
```

Теперь удаляем **все** файлы, которые не являются финальным результатом:

```bash
git rm __sandbox.ipynb
git rm tmp.md
git rm "Pasted image 20260426211115.png"
git rm "curr state.md"
git rm "DEEP — Теория по accuracy.md"
git rm "DEEP — Теория по кросс-валидации.md"
git rm "DEEP — Алгоритм псевдослучайных чисел.md"
git commit -m "clean: оставить только релизный код в proddone"
git push
```

**Состояние `proddone`:** только `DONE.ipynb`, `STEP.md`, `ALIGN.md`, `README.md`, `requirements.txt`, `.obsidian/`, `.vscode/`. Это статус `#DONE`.

---

## ⚙️ Шаг 4: Меняем ветку по умолчанию на GitHub
Чтобы `proddone` стала основной веткой репозитория:

1. Зайди на страницу репозитория на GitHub.
2. **Settings** → **Branches** (в левом меню).
3. В разделе «Default branch» нажми на значок карандаша, выбери `proddone` и подтверди.
4. (Опционально) Удали старую ветку `main` на GitHub:  
   ```bash
   git push origin --delete main
   ```

Теперь при клонировании репозитория по умолчанию будет выгружаться `proddone`.

---

## 📊 Итоговая структура

| Ветка | Содержит | Маркер готовности |
|---|---|---|
| `mindground` | **Всё** как сейчас: `__sandbox.ipynb`, `tmp.md`, DEEP-файлы, `DONE.ipynb`, `curr state.md` | `#PROC`, `#EXPERIMENT` |
| `made` | Стабильные артефакты + теория + `curr state.md` | `#MAKED` |
| `proddone` | Только финальный код отчёта | `#DONE` |

---

## 🔄 Как работать дальше

- **Эксперименты (Task 3, кросс-валидация и т.п.)**  
  `git checkout mindground` → работаешь в `__sandbox.ipynb` или `tmp.md`, пробуешь, ошибаешься, фиксируешь маркерами `#PROC`, `#FIXME`.  
- **Перенос зрелых блоков в стабильный прототип**  
  Когда решение готово, переключаешься в `made`, копируешь нужные ячейки/функции в `DONE.ipynb`, коммитишь с якорем:  
  ```
  feat: add cross-validation results (Task 3)
  Разработано в mindground (__sandbox.ipynb, ячейки 12–18)
  ```
- **Релиз**  
  Когда весь проект в `made` протестирован и готов:  
  ```bash
  git checkout proddone
  git merge made
  git push
  ```
  И `proddone` обновляется, оставаясь чистым.

Твоя триада `mindground` → `made` → `proddone` полностью отражает эволюцию любого проекта, а маркеры внутри файлов подскажут, на каком этапе находится каждый фрагмент кода.