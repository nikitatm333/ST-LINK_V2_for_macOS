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

Установка программ:

```
sudo make install
```




