# 📦 Установка и запуск проекта DeepFake Detector for Music

## 1. Клонирование репозитория
```bash
# Если репозиторий ещё не склонирован
git clone https://github.com/your-org/deepfake-detector-rus.git
cd deepfake-detector-rus
```

## 2. Настройка Python‑окружения
Рекомендуется использовать **virtualenv** или **conda**.
```bash
# С помощью virtualenv
python3 -m venv venv
source venv/bin/activate

# С помощью conda (если предпочтительно)
conda create -n deepproject python=3.11 -y
conda activate deepproject
```

## 3. Установка зависимостей
```bash
pip install -r requirements.txt
```
> Если `pip` выдаёт ошибки из‑за версии Python, убедитесь, что используете Python 3.11‑3.12.

## 4. Подготовка предобученных моделей
Для работы детектора нужны два внешних репозитория:
- **Musika** – автоэнкодер для музыкального сигнала.
- **LAC** – автоэнкодер из проекта **VampNet**.
```bash
mkdir -p pretrained
cd pretrained
# Musika
git clone https://github.com/marcoppasini/musika.git
# LAC (VampNet weights)
git clone https://github.com/hugofloresgarcia/lac.git
cd lac
# Скачайте предобученные веса VampNet (см. README проекта LAC)
# Обычно они находятся в папке `weights/` или доступны через release.
cd ../../..
```
> После клонирования убедитесь, что в `musika` и `lac` есть папки с модельными весами.

## 5. Правка скрипта `utils_encode.py`
В `musika` необходимо добавить функцию `encode_audio`, возвращающую латентный вектор модели.
1. Откройте `musika/utils_encode.py`.
2. Скопируйте метод `compress_whole_files` и замените сохранение `np.save` на `return lat`.
3. Сохраните изменения.
```bash
# Пример (открыть в вашем редакторе)
vim musika/utils_encode.py
# Добавьте функцию и сохраните.
```

## 6. Обучение модели
```bash
python scripts/train.py --config conf/train_config.yaml
```
*Параметры конфигурации можно менять в `conf/`.

## 7. Оценка модели
```bash
python scripts/eval.py --config conf/eval_config.yaml
```

## 8. Калибровка (опционально)
Для калибровки детектора запустите:
```bash
python scripts/calibration.py --config conf/calibration.yaml
```

## 9. Запуск детектора на новых аудио‑файлах
```bash
python scripts/predict.py --input path/to/audio.wav --output results.json
```

## 10. Доступные аудио‑примеры
Все примеры находятся в папке `audio_examples/`. Вы можете протестировать их, указав путь к файлу.

---
**⚡️ Быстрый старт**
```bash
# Всё в один блок (предполагая, что у вас уже установлен git и Python)
git clone https://github.com/your-org/deepfake-detector-rus.git && cd deepfake-detector-rus
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
mkdir -p pretrained && cd pretrained && git clone https://github.com/marcoppasini/musika.git && git clone https://github.com/hugofloresgarcia/lac.git && cd ..
# Не забудьте поправить utils_encode.py в Musika (см. пункт 5)
python scripts/train.py --config conf/train_config.yaml
```

---
**Поддержка**
Если возникнут вопросы, откройте Issue в репозитории или пишите по адресу: [research.deezer.com/deepfake-detector/](https://research.deezer.com/deepfake-detector/).
