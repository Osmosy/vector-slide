---
name: vector-slide
description: Use when fine-tuning web design — padding, radius, shadows, typography, animation. Generates a live control panel (sliders) over the page instead of text-based back-and-forth.
---

# Vector Slide

Live-панель ползунков для тонкой настройки дизайна поверх страницы.

## Концепция

Текст — грубый инструмент для design-тюнинга. "Подвинь на 4px", "помягче тень", "больше скругление" — десять сообщений, полчаса, и всё равно не то. Vector Slide ставит панель с ползунками прямо поверх макета. Крутишь — видишь результат вживую. Поймал идеал — "Copy CSS" — вставил значения в стили.

## Когда использовать

- Тонкая настройка отступов, скруглений, теней, типографики
- Подбор скорости анимации и easing
- Интерактивный тюнинг любого web-дизайна
- Когда агент гоняется по кругу: "левее → много → верни → правее"

## Промпт для запуска

```
Дизайн не трогай. Собери временную панель управления поверх страницы.
Ползунки на: внешние и внутренние отступы, размер шрифта, высоту строки,
скругления, толщину и цвет границ, тень, скорость анимации.
Всё должно применяться вживую, без перезагрузки.
Рядом кнопка "скопировать текущие значения".
Когда скажу "готово" — вбей значения в стили, а панель удали.
```

## Шаги

1. **Не модифицируй существующий дизайн.** Панель — это overlay, не часть макета.
2. **Используй CSS custom properties** с префиксом `--vs-` (vector-slide), чтобы не конфликтовать:
   - `--vs-padding`, `--vs-padding-y`, `--vs-padding-x` — отступы
   - `--vs-margin`, `--vs-gap` — внешние отступы и зазоры
   - `--vs-font-size`, `--vs-line-height`, `--vs-letter-spacing`, `--vs-font-weight` — типографика
   - `--vs-radius`, `--vs-radius-sm`, `--vs-radius-lg` — скругления
   - `--vs-border-width`, `--vs-border-color` — границы
   - `--vs-shadow-x/y/blur/spread/opacity/color` — тень (6 параметров)
   - `--vs-transition-duration`, `--vs-transition-delay` — анимация
   - `--vs-max-width` — размеры
3. **Каждый ползунок меняет CSS-переменную через `document.documentElement.style.setProperty()`** — изменения применяются мгновенно, без перезагрузки.
4. **Кнопка "Copy CSS"** копирует в буфер готовый `:root { ... }` блок со всеми значениями.
5. **Кнопка "Done"** выводит CSS в консоль и удаляет панель.
6. **Кнопка "Reset"** возвращает значения по умолчанию.
7. **Панель можно перетаскивать** за header (drag) и **ресайзить** за левый край.
8. **Ctrl+Shift+V** — скрыть/показать панель.

## Шаблон

Готовый self-contained HTML-шаблон: `templates/vector-slide.html`

### Подключение к существующей странице

**Вариант A — инлайн:**
Вставь `<style>` и `<script>` блок из шаблона в `<head>` страницы.
Добавь `<div id="vs-panel">...</div>` перед закрывающим `</body>`.

**Вариант B — внешняя загрузка:**
```html
<script>
fetch('vector-slide.html').then(r => r.text()).then(html => {
  const div = document.createElement('div');
  div.innerHTML = html;
  document.body.appendChild(div);
});
</script>
```

### Интеграция с CSS

Назначь CSS-переменные элементам:
```css
.card {
  padding: var(--vs-padding-y) var(--vs-padding-x);
  border-radius: var(--vs-radius);
  border: var(--vs-border-width) solid var(--vs-border-color);
  box-shadow:
    var(--vs-shadow-x) var(--vs-shadow-y)
    var(--vs-shadow-blur) var(--vs-shadow-spread)
    rgba(var(--vs-shadow-color), var(--vs-shadow-opacity));
  transition: all var(--vs-transition-duration) var(--vs-easing);
}
```

### Расширение списка ползунков

Добавь новый ползунок в HTML:
```html
<div class="vs-control">
  <div class="vs-control-row">
    <span class="vs-label">My param</span>
    <span class="vs-value" id="val-my-param">10px</span>
  </div>
  <input type="range" class="vs-slider" id="my-param"
    min="0" max="100" value="10" data-unit="px" data-var="--vs-my-param">
</div>
```

Ползунок автоматически подхватится JS-инициализацией (querySelectorAll('.vs-slider')).

## Источник

Идея: "Designing With AI? Make a Jig." — every.to/p/pardon-the-disruption
Термин "jig" из столярного дела: направляющая-шаблон для повторения точных настроек.

## Палитра

Цвет акцента: #F97316 (orange-500). Логотип Vector — звезда→зеркало.
Фон панели: #1A1A2E. Текст: #E0E0E0. Values: #FBBF24 (amber-400).