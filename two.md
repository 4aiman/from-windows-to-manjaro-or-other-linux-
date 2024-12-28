## Прошлый файл был "цивильным". Этот будет "горящим". 
(И на русском, потому что всем в каноникле и минте насрать - `wontfix`ы у них там)

Сразу скажу, что я ставил и удалял кучу всяких DE после того как поставил систему.<br>
Поэтому у меня остались следы и проблемы от Gnome (без него вообще хер что работает), UKUI, Unity.<br>
Остановился на Cinnamon<br>

- ### Переключение языков не работает
  
  Стандартная панель цитрамона показывает говно. Оно вседа EN независимо от настроек.<br>
  Всякие "умники", которые переустанавливали шиндовс в нулевых, скажут, что это следствие кучи DE.<br>
  Только вот чинится всё добавилением апплета с флажком раскладки, который называется `Клавиатура (keyboard@cinnamon.org)`.<br>
  А нерабочая иконка - от iBus 1.5.29-rc2. Зачем оно там, если не работает - хз, но проблема общая для Deepin и Bazzit.
  Скорее всего разрабы тупо используют английский как елинственный язык. FT.

- ### Опять скриншоты, теперь в Cinnamon 

    Во-первых, это говно всё равно юзает gnome-screenshot, несмотря на "свою" DE.<br>
    Чинить так:
    - открываем "блокнот" и пишем туда вот так
    ```
    gnome-screenshot -a -c -f ~/Изображения/Screenshots/Screenshot_"$(date -d "today" +"%d.%m.%y - %H_%M_%S").png"
    ```
    - сохраняем `где-нибудь` и даём права на выполнение (можно chmod, можно через гуй)
    - ищём в меню `клавиатура`/`keyboard`
    - идём в доп. комбинации клавиш
    - вешаем на Win/Super+Shift+S команду выполнения файла со скриптом (тот, что лежить в `где-нибудь`)
    
    Почему так?
    - Потому что хрень, которая вызывает `gnome-screenhot`, не умеет работать с переменными так же, как это умеет аналогичная хрень в xfce.
    - Твики `/org/gnome/gnome-screenshot/auto-save-directory` становятся бесполнезными, если вам надо и скрин сделать и в буере его поиметь.
    - А это потому, что dfonf-editor (или как там его) не даёт сохранить скриншоты, которые идут в буфер обмена. С окнами и цеоым столом работает.
  
- ### Codium & Chrome
    Ставим из deb пакетов в обход snap. Говно этот ваш снап.

- ### Эмули
  Ставим AppImage'ы и не дуем в ус. Касается PS2/PS3 и дебильного недогуя RetroArch

- ### Steam
  Оно просто работает.
  Гавное, чтобы игры были не на NTFS томе.
  Стим-Твикер-как-его-там для запуска SpecialK *не нужен* (он в принципе не нужен).

  ***ВНИМАНИЕ***<br>
  Папка Стима по-молчанию находится в `~/.local/share/steam`, но после обновления ставится snap версия, и всё переезжает в `~/snap/steam/common/.local/share/Steam`<br>
  Игры при этом проложают работать, включая перенос сейвов и SpecialK.

- ### Кстати, о SpecialK
  - Качаем себе [`SpecialK.7z`](https://github.com/SpecialKO/SpecialK/releases) из релизов
  - выбираем dllку в зависимости от разрядности игры
  - распаковываем её в папку с игрой (где экзэшник)
  - перименовываем dllку в `dxgi.dll` или что-нибудь ещё (см. репу SpecialK)
  - После первого запуска, в папке с игрой появится SK_Res - туда можно кидать текстуры для подмены

- ### Пароли и keyring
  Моё любимое.<br>
  Когда челу говорят "поиграйся с live системой, пока не решил поставить", он может попробовать много чего.<br>
  И это "много чего" сохранится в уже установленной системе.<br>
  Одна беда - ПАРОЛЬ У LIVE ЮЗЕРА мы не знаем.<br>
  Выход - `passwd` в консоли. <br>
  Прчём, в live сессии оно даст поставить что угодно, даже пресловутые `123`, но вот в уже поставленной среде такое не прокатит. Там надо из-под root/sudo это делать.<br>

  Отдельный прикол с `passwd` в том, что долбанное "Пароли и Ключи" (на самом деле `seahorse` - вот где локализация мешает!) не знает ничего о том, что пароль сменился.<br>
  И, если вы забыли старый пароль, то хз. Пересоздавайте себе брелок и/или ключницу.<br>

  Почему это важно?<br>
  Потому, что iBus не работает "из коробки" (см. выше). Либо в систему не войти, потому что раскладку с русского не переключить, либо потом вот такие приколы.<br>

- ### W: Загрузка выполняется от лица суперпользователя без ограничений песочницы, так как файл «/root/.synaptic/tmp//tmp_sh» недоступен для пользователя «_apt». - pkgAcquire::Run (13: Отказано в доступе)
  Тянется с 2018 года. Можно забить, тк. это ***w***arning, а не ошибка. <br>
  Если больно - можно проверить, что пользователь _apt существует и каталог /root/.synaptic/ имеет владельца _apt и права 0700:
  ```
  sudo chown _apt /root/.synaptic/
  ```
  0700 - это `drwx------`, проверить можно через `stat -c '%a %n' /root/.synaptic`
  
- ### MAXINE SDK
  Есть [это](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/maxine/resources/maxine_linux_vfx_sdk_ga/files), но оно платное и для энтерпрайза.<br>
  Простым васянам нужно что-то из 2022, но не убунта и не центось, и чтобы там была 11ая CUDA. <br>
  Собственно, пока `0.7.5.0_GA` за бабки, в ОБС нихера не будет работать ни один фильтр от NVidia.  <br>
  А и правильно, а и пошли мы нахер :/

- ### FFZ
  Расширения для огнелиса могут не работать точно так же, как и на винде. А тут ещё и CDN забанен.
  Пользуемся [заменой](https://cdn2.frankerfacez.com/script/ffz_injector.user.js)

- ### StreamFX ушёл ~~в зад~~ на патреон
  Сырцы забрал с собой. Есть форки и счастливчики, которые успели успели собрать v30 "до того как".
  Сборка Manjaro, которую я делал год назад не работает на современной бубунте.

- ### OBS всё так же держит камеру...
  Ну что, пиздос, конечно.<br>
  Берём исходники обс и собираем - примерно как написано в [one.md](one.md)<br>

  Проблема в том, что на убунте нет aur'а и придётся ставить руками ебучую AJA, libdatachannel,  и т.п.
  Проще гуглить, но вот что-то:
  - заголовки под ядро:
  ```  
  sudo apt install -y linux-headers-$(uname -r) linux-tools-$(uname -r)
  ```
  - туева хуча зависимостей (удивительно, как это не сломало цитрамор и не привело к удалению нужных пакетов):  
  ```
  sudo apt install cmake ninja-build pkg-config clang clang-format build-essential curl ccache git zsh libavcodec-dev libavdevice-dev libavfilter-dev libavformat-dev libavutil-dev libswresample-dev libswscale-dev libx264-dev libcurl4-openssl-dev libmbedtls-dev libgl1-mesa-dev libjansson-dev libluajit-5.1-dev python3-dev libx11-dev libxcb-randr0-dev libxcb-shm0-dev libxcb-xinerama0-dev libxcb-composite0-dev libxcomposite-dev libxinerama-dev libxcb1-dev libx11-xcb-dev libxcb-xfixes0-dev swig libcmocka-dev libxss-dev libglvnd-dev libgles2-mesa-dev libwayland-dev librist-dev libsrt-openssl-dev libpci-dev libpipewire-0.3-dev libqrcodegencpp-dev uthash-dev qt6-base-dev qt6-base-private-dev qt6-svg-dev qt6-wayland qt6-image-formats-plugins libasound2-dev libfdk-aac-dev libfontconfig-dev libfreetype6-dev libjack-jackd2-dev libpulse-dev libsndio-dev libspeexdsp-dev libudev-dev libv4l-dev libva-dev libvlc-dev libvpl-dev libdrm-dev nlohmann-json3-dev libwebsocketpp-dev libasio-dev libxcb-xinput-dev libffmpeg-nvenc-dev libsndfile1-dev libsoxr-dev libsox-dev
  ```
  Если тут не всё или что-то отсутствует - идите в жопу 😉

  Ещё понадобится хромиум без башки отсюда https://cdn-fastly.obsproject.com/downloads/cef_binary_6533_linux_x86_64.tar.xz. Распаковать через `tar -xvf` не получится, но у вас есть энгрампа, файл-роллер или фар2л. Если нет - печаль.

  Аджа сама не ставится, надо собирать.
  ```
  cd ~
  git clone https://github.com/aja-video/libajantv2
  cd libajantv2
  cmake -S . -B build
  cmake --build build
  cd driver/linux
  make clean && make -j12
  sudo make install  
  ```
  Чтобы не перезагружать, сразу херачим модуль в ядро
  ```
  sudo insmod ajantv2.ko
  ```
  Но всё-таки через DKMS ставим в систему
  ```
  make dkms-install
  ```

  DataChannel тже не ставится из пакетов
  ```
  cd ~
  git clone https://github.com/paullouisageneau/libdatachannel.git
  git submodule update --init --recursive --depth 1
  cmake -B build -DUSE_GNUTLS=0 -DUSE_NICE=0 -DCMAKE_BUILD_TYPE=Release
  cd build
  make -j12
  sudo make install
  ```
  RNNoise
  ```
  cd ~
  git clone https://github.com/sysprog21/rnnoise
  cd rnnoise
  make -j12
  sudo make install
  ```

  Вроде всё! Должно собраться.<br>
  Если хромиум распакован в `~/cef_binary_6533_linux_x86_64`, то что-то такое должно сработать, если ничего не поломали в апстриме (а они мастера):
  ```
  cd ~/portable_obs_build_dir/obs-studio/ && rm -rf build && rm -rf "$HOME/portable_obs_build_dir/release" && mkdir build && cd build && cmake -DLINUX_PORTABLE=ON -DENABLE_PORTABLE_CONFIG=ON -DCMAKE_INSTALL_PREFIX="$HOME/portable_obs_build_dir/release/" -DENABLE_BROWSER=ON -DCEF_ROOT_DIR="../../../cef_binary_6533_linux_x86_64" -DTWITCH_HASH=0 -DTWITCH_CLIENTID=selj7uigdty0j5ijt41glcce29ehb4 -DOAUTH_BASE_URL=https://auth.obsproject.com/ -DENABLE_BROWSER_PANELS=ON -DENABLE_DECKLINK=ON -DENABLE_FREETYPE=ON -DENABLE_HEVC=ON -DENABLE_JACK=ON -DENABLE_LIBFDK=ON -DENABLE_NEW_MPEGTS_OUTPUT=ON -DENABLE_PLUGINS=ON -DENABLE_PULSEAUDIO=ON -DENABLE_RNNOISE=ON -DENABLE_SCRIPTING=ON -DENABLE_SCRIPTING_LUA=ON -DENABLE_SCRIPTING_PYTHON=ON -DENABLE_SERVICE_UPDATES=ON -DENABLE_SNDIO=ON -DENABLE_SPEEXDSP=ON -DENABLE_UI=ON -DENABLE_V4L2=ON -DENABLE_VLC=ON -DENABLE_VST=ON -DENABLE_WAYLAND=ON -DENABLE_VST_BUNDLED_HEADERS=ON -DENABLE_WEBRTC=ON -DENABLE_WEBSOCKET=ON -DBUILD_TESTS:BOOL="1" -DENABLE_JACK:BOOL="1" -DENABLE_SNDIO:BOOL="1" -DENABLE_RTMPS:STRING="ON" -DCMAKE_BUILD_TYPE:STRING="RelWithDebugInfo" -DCALM_DEPRECATION:BOOL="1" -DENABLE_LIBFDK:BOOL="1" -DENABLE_UDEV=ON .. && make -j12 && make install && mkdir -p ~/portable_obs_build_dir/release/lib/x86_64-linux-gnu  && cp ~/portable_obs_build_dir/release/lib/obs-scripting ~/portable_obs_build_dir/release/lib/x86_64-linux-gnu/ -r && cd "$HOME/portable_obs_build_dir/release/bin/" && ./obs

  ```

  Не ссыте, систему не запорет (если все те пакеты и драйверы выще не запороли) - сборка портабельная.

  От сборки на арче/манжаро отличается тем, что бинарник один (а может это 31ая версия ОБс подосрала), так что нет больше в папке `bin` папки `64bit`. 

  Если собралось и запустилось - значит работает. Значит, можно, наконец-то, патчить камеру.<br>

  ***Не забываем удалить обс, поставленный из пакета "до того как"!***<br>
  (Нет, можно, конечно, и оставить, но потом не спрашивайте, почему патч не работает.)
    
  Патч из one всё ещё может быть применён вручную, но не работает из коробки.
  Вот вам для версии 31 от 28.12.2024 (ветка мастер) [webcam.patch](files/webcam.patch)

  А вообще, текущая версия не находит то луа, то либу фронта. Даже есть делать непортабельную сборку. 
  
  
  
  
  
  

  
  
  
