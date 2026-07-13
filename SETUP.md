# Как поставить баннер в профиль GitHub

Баннер живёт в **специальном репозитории, имя которого = твой ник**: `reitar0/reitar0`.
GitHub показывает `README.md` такого репозитория прямо в шапке профиля.

## Что в этой папке

```
reitar0/
├─ README.md          # вставляет баннер через <picture> (авто-тема)
├─ assets/
│  ├─ dark.svg        # тёмная тема GitHub
│  └─ light.svg       # светлая тема GitHub
└─ SETUP.md           # этот файл
```

`preview.html`, `plain.html` — временные файлы для локального просмотра, в репозиторий их заливать не нужно (они уже в `.gitignore`).

## Вариант A — через сайт GitHub (проще всего, без консоли)

1. Зайди на https://github.com/new
2. **Repository name** введи ровно `reitar0` (совпадает с ником). Появится зелёная плашка
   *"You found a secret! reitar0/reitar0 is a special repository…"* — значит всё верно.
3. Поставь **Public**, галочку **Add a README** можно НЕ ставить.
4. Create repository.
5. На странице репозитория: **Add file → Upload files**. Перетащи `README.md` и папку `assets`
   (с `dark.svg` и `light.svg`). Commit changes.
6. Открой https://github.com/reitar0 — баннер уже в шапке. Переключи системную тему
   (светлая/тёмная) и обнови страницу — увидишь обе версии.

## Вариант B — через git (из этой папки)

```bash
cd D:/Projects/reitar0
git init
git add README.md assets
git branch -M main
git commit -m "Add animated profile banner"
git remote add origin https://github.com/reitar0/reitar0.git
git push -u origin main
```

> Если репозиторий `reitar0/reitar0` ещё не создан — сначала создай его на
> https://github.com/new (шаги 1–4 из варианта A), затем выполняй push.

## Как менять содержимое потом

Текст (роли, город, вуз, focus, email, навыки, соцсети) захардкожен в **обоих** SVG.
Правь одни и те же строки в `assets/dark.svg` и `assets/light.svg`, коммить, пуш —
профиль обновится (GitHub кэширует картинки через camo, обновление занимает
до нескольких минут; можно обновить страницу с Ctrl+F5).

Ключевые места в SVG:
- **роли** (печатающаяся строка) — блок `<!-- typing roles -->` + ширины в `clipPath id="tp0..tp4"`
  (если меняешь длину фразы — поправь и `width` в соответствующем `tp*`, иначе обрежется/останется зазор);
- **навыки** — блок `<!-- pills -->` (каждая пилюля = свой `<g>`);
- **город / вуз / focus / email** — блок `<!-- info rows -->`;
- **соцсети** — блок `<!-- socials -->`.

## Важно про анимации на GitHub

- Анимации сделаны на **SMIL** (`<animate>`), без JavaScript — GitHub вырезает JS,
  но SMIL проигрывает. В шапке профиля баннер будет анимированным.
- **Наведение мышью не работает** в README: GitHub вставляет SVG как картинку `<img>`,
  а картинка не получает событий мыши. Поэтому «увеличение пилюль при наведении»
  заменено на постоянный мягкий пульс подсветки — он виден без наведения.
