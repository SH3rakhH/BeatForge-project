# BeatForge Desktop

Нативный десктопный секвенсор на Python + PySide6. Работает на
Windows / macOS / Linux. Реальная синтезация через `sounddevice` +
`numpy`.

## Установка

```bash
pip install -r requirements.txt
```

Требуется Python **3.10+**.

## Запуск

```bash
python app.py
```

## Сборка в EXE

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name BeatForge app.py
```

Готовый `dist/BeatForge.exe` (~60 МБ) запускается как обычное приложение.

Для macOS / Linux аналогично, только `--windowed` заменить на `--noconsole`.

## Управление

- **PLAY** — старт/стоп (кнопка ▶ / ■)
- **ЛКМ по шагу** — включить/выключить
- **ПКМ по шагу** — меню: velocity, probability, ratchet, flam, pitch, note
- **M / S** — mute / solo трека
- **BPM / SWING / STEPS / KIT / VOL** — в верхней панели
- **PAT A/B/C/D** — переключение паттернов
- **Random / Fill / Clear / Copy / Paste** — инструменты
- **Save / Load** — проекты в JSON

## Архитектура

```
engine.py   AudioEngine + Sequencer + синтез + модель
app.py      PySide6 GUI + логика UI
```

- Синтез реализован на numpy (kick, snare, clap, hats, toms, bass, lead и т.д.)
- Реалтайм-микс через `sounddevice.OutputStream` с блоком 512 сэмплов
- Секвенсор работает в отдельном потоке с точным таймингом (`time.perf_counter`)
- Голоса складываются в lock-free очередь и микшируются в аудио-callback

## Расширение

- Новый трек: добавьте в `TRACK_DEFS` и функцию `synth_*`, зарегистрируйте в `render_track`
- Новый жанр Drummer: аналог веб-версии — добавьте таблицу паттернов
- Дополнительные эффекты: добавьте node в `AudioEngine._callback` (уже после микса)
