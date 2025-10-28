# Program 4

## Описание

Программа автоматизации резервного копирования файлов. Скрипт автоматически создает копии указанных файлов или директорий в заданную папку для резервного копирования с добавлением временной метки к имени файла.

## Код

```python
import os
import shutil
from datetime import datetime

def backup_files(source_path, backup_dir):
    """
    Создает резервную копию файла или директории
    
    Args:
        source_path: Путь к файлу или директории для копирования
        backup_dir: Директория для хранения резервных копий
    """
    # Создаем директорию для бэкапов, если её нет
    if not os.path.exists(backup_dir):
        os.makedirs(backup_dir)
    
    # Получаем текущее время для имени файла
    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
    
    # Получаем имя файла/директории
    item_name = os.path.basename(source_path)
    
    # Формируем имя для резервной копии
    backup_name = f"{item_name}_{timestamp}"
    backup_path = os.path.join(backup_dir, backup_name)
    
    try:
        if os.path.isfile(source_path):
            # Копируем файл
            shutil.copy2(source_path, backup_path)
            print(f"✓ Файл {item_name} успешно скопирован в {backup_path}")
        elif os.path.isdir(source_path):
            # Копируем директорию
            shutil.copytree(source_path, backup_path)
            print(f"✓ Директория {item_name} успешно скопирована в {backup_path}")
        else:
            print(f"✗ Путь {source_path} не найден")
            return False
        
        return True
    except Exception as e:
        print(f"✗ Ошибка при копировании: {e}")
        return False

# Пример использования
if __name__ == "__main__":
    # Укажите путь к файлу/директории для бэкапа
    source = "C:/Documents/important_file.txt"
    
    # Укажите директорию для хранения бэкапов
    backup_directory = "C:/Backups"
    
    # Выполняем резервное копирование
    backup_files(source, backup_directory)
```

[← Назад на главную](README.md)
