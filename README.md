# ST-LINK_V2_for_macOS
Попытка настроить macOS для использования программатора ST-LINK V2 

Для начала установим Homebrew [Homebrew](https://brew.sh/).  
Homebrew — это менеджер пакетов для macOS, который значительно упрощает процесс установки, обновления и удаления программного обеспечения через терминал. Он поможет в установке инструментов и библиотек, которых нет в стандартной поставке macOS.

Качаем его командой:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Далее нам понадобится несколько инструментов:
- autoconf – инструмент для автоматической генерации configure-скриптов, часто используется в сборке программ из исходников.
- automake – утилита для автоматического создания файлов Makefile.in на основе Makefile.am, используется в процессе сборки.
- pkg-config – утилита, которая помогает находить пути и флаги компиляции для библиотек (CFLAGS, LDFLAGS).
- libusb – библиотека для работы с USB-устройствами на низком уровне.
- libusb-compat – совместимая версия libusb для программ, ожидающих API старой версии libusb-0.1.
  
```
brew install autoconf automake pkg-config libusb libusb-compat"
```

Stlink — это open-source проект, исходники которого можно взять тут [https://github.com/texane/stlink.git].  
Перед клонированием репозитория создаем папку, в которую сохраним проект.  

```
mkdir stlink_v2 && cd stlink_v2
```

Клонируем репозиторий:

```
git clone https://github.com/texane/stlink.git && cd stlink/
```

Создаем папку для сборки:  

```
mkdir build && cd build
```

Установка CMake через Homebrew:

```
brew install cmake
```

Генерация файлов сборки с CMake:  
(Запускаем в родительской папке)

```
cmake ..
```

Компилируем проект:

```
make
```
(Если на этом моменте возникает ошибка, то в конце есть ее возможное решение)


Установка программ:

```
sudo make install
```


# Нюансы 
## Проблема с компиляцией на macOS
При выполнении команды make для сборки проекта может возникнуть следующая ошибка:

```
In file included from .../libusb_settings.h:49:26:
error: 'MINIMAL_API_VERSION' is not defined, evaluates to 0 [-Werror,-Wundef]
```
### Причина ошибки
Ошибка возникает потому, что в файле libusb_settings.h макрос MINIMAL_API_VERSION не определён для платформы macOS. В исходном коде проекта этот макрос задан только для систем FreeBSD, OpenBSD, Linux и Windows, а для macOS отсутствует соответствующее определение.

### Решение
Чтобы устранить ошибку, необходимо отредактировать файл libusb_settings.h и добавить определение MINIMAL_API_VERSION для macOS. Например, можно внести следующие изменения:

- Откройте файл по пути:
  ```
  stlink/src/stlink-lib/libusb_settings.h
  ```
- Найдите блок с определениями платформ:
  ```
  #if defined (__FreeBSD__)
    #define MINIMAL_API_VERSION 0x01000102 // v1.0.16
  #elif defined (__OpenBSD__)
      #define MINIMAL_API_VERSION 0x01000106 // v1.0.22
  #elif defined (__linux__)
      #define MINIMAL_API_VERSION 0x01000106 // v1.0.22
  #elif defined (_WIN32)
      #define MINIMAL_API_VERSION 0x01000109 // v1.0.25
  #endif
  ```
- Добавьте условие для macOS:
  ```
  #elif defined (__APPLE__)
    #define MINIMAL_API_VERSION 0x01000106 // v1.0.22
  ```





