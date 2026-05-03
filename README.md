# Meshtastic Ollama Bridge

Python-мост между Meshtastic LoRa mesh-сетью и локальной LLM через Ollama.

## Схема работы

```text
Meshtastic node #2
        ↓ LoRa
Meshtastic node #1
        ↓ USB Serial
Python script
        ↓ HTTP
Ollama + local LLM
        ↓
Python script
        ↓ USB Serial
Meshtastic node #1
        ↓ LoRa
Meshtastic node #2
```

## Что делает скрипт

Скрипт слушает входящие сообщения Meshtastic.

Если сообщение начинается с `/ai`, текст отправляется в локальную модель Ollama. Ответ модели ограничивается по длине и отправляется обратно через Meshtastic.

## Проверено на

- Heltec ESP32 LoRa 32 V4
- Windows
- Ollama
- `huihui_ai/qwen3.5-abliterated:35b`
- Meshtastic Python API

## Установка

```bash
pip install -r requirements.txt
```

## Настройка

В файле `mesh_ai_bridge.py` укажите COM-порт Meshtastic-ноды:

```python
MESHTASTIC_PORT = "COM8"
```

И модель Ollama:

```python
MODEL = "huihui_ai/qwen3.5-abliterated:35b"
```

При необходимости можно изменить триггер:

```python
TRIGGER = "/ai"
```

## Запуск

```bash
python mesh_ai_bridge.py
```

После запуска отправьте с другой Meshtastic-ноды сообщение:

```text
/ai привет, кто ты?
```

Если всё работает, Python-скрипт примет сообщение, отправит его в Ollama, получит короткий ответ и отправит его обратно через Meshtastic.

## Важно

Meshtastic/LoRa не предназначен для длинных текстов. Ответы модели нужно держать короткими, чтобы не забивать радиоэфир.

Также соблюдайте местные правила использования радиочастот, мощности передатчика и duty cycle.

## License

MIT
