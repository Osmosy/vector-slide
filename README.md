<p align="center">
  <img src="assets/vector-logo.png" width="240" alt="Vector Logo">
</p>

<h1 align="center">Vector Slide</h1>

> Live-панель ползунков для тонкой настройки web-дизайна. Вместо "подвинь левее → много → верни" — крутишь ползунок и видишь результат вживую.

![Vector Slide](https://img.shields.io/badge/Vector%20Slide-v1.0-F97316)

## Что это

Vector Slide — это "jig" (направляющая-шаблон) для AI-агентов и дизайнеров. Вместо десятков сообщений с текстовыми правками ("подвинь на 4px", "тень помягче", "скругление больше") — агент собирает панель с ползунками прямо поверх страницы. Крутишь, смотришь, ловишь идеал, нажимаешь "Copy CSS".

## Как работает

1. Агент подключает панель к странице (overlay, дизайн не трогает)
2. Ползунки меняют CSS custom properties (`--vs-*`) в реальном времени
3. Все элементы со `var(--vs-*)` обновляются мгновенно
4. "Copy CSS" — готовый `:root { ... }` блок в буфер
5. "Done" — CSS в консоль, панель удаляется

## Параметры

| Группа | Ползунки |
|--------|----------|
| Отступы | padding, padding-y, padding-x, margin, gap |
| Типографика | font-size, line-height, letter-spacing, font-weight |
| Форма | radius, radius-sm, radius-lg, border-width, border-color |
| Тень | offset-x/y, blur, spread, opacity, color |
| Анимация | duration, delay |
| Размеры | max-width |

## Быстрый старт

### Промпт для AI-агента

```
Дизайн не трогай. Собери временную панель управления поверх страницы.
Ползунки на: внешние и внутренние отступы, размер шрифта, высоту строки,
скругления, толщину и цвет границ, тень, скорость анимации.
Всё должно применяться вживую, без перезагрузки.
Рядом кнопка "скопировать текущие значения".
Когда скажу "готово" — вбей значения в стили, а панель удали.
```

### Подключение

```html
<!-- Вариант 1: инлайн — вставь <style> и <script> из templates/vector-slide.html -->

<!-- Вариант 2: внешняя загрузка -->
<script>
fetch('vector-slide.html').then(r => r.text()).then(html => {
  const div = document.createElement('div');
  div.innerHTML = html;
  document.body.appendChild(div);
});
</script>
```

### Использование в CSS

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

## Фичи

- All-CSS-variables — ползунки меняют `--vs-*`, не инлайн-стили
- Drag & resize — панель перетаскивается и меняет ширину
- Ctrl+Shift+V — скрыть/показать панель
- Copy CSS — готовый блок в буфер обмена
- Reset — вернуть значения по умолчанию
- Self-contained — один HTML-файл, без зависимостей
- z-index: 2147483647 — поверх всего

## Установка как Hermes Agent Skill

```bash
# Скопировать в ~/.hermes/skills/
cp -r vector-slide ~/.hermes/skills/
```

## Источник

Идея из статьи ["Designing With AI? Make a Jig."](https://every.to/p/designing-with-ai-make-a-jig) — every.to

Термин "jig" из столярного дела: направляющая-шаблон для повторения точных настроек.

## License

MIT