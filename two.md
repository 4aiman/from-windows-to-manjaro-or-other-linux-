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
  Скорее всего разрабы тупо используют английский как единственный язык. FT.

- ### иконки для .exe файлов
  ```
  sudo ap install icoextract-thumbnailer
  ```

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
  Папка Стима по-молчанию находится в `~/.local/share/steam`, но после обновления ставится snap версия, и всё переезжает в `~/snap/steam/common/.local/share/Steam`<br> Лучше отключить snap до того как начинаешь ставить всякое, иначе потом начнётся неразбериха откуда что ставить. Не убунтой единой. 
  
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

  Для сборки нам будет нужен сам OBS, поэтому клонируем его в `~/portable_obs_build_dir/obs-studio/`. <br>
  Официальная репа тут https://github.com/obsproject/obs-studio <br>
  Если в падлу потом маяться ещё и с патчем, то можно склонировать это: https://github.com/4aiman/obs-studio/commit/89fd4cb46112b37142f85f88bd96b73971b0073b#diff-bf68fe13e5406abc066eade5979836ab91ba9d914b895a18566dfce715810872R1107

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

  А вообще, текущая версия не находит то луа, то либу фронта. Даже есть делать непортабельную сборку. <br>
  Раньше работало сразу, даже либы локальные переопределяли те, что были поставлены из пакета.

  Короче, запускать так:
  ```
  cd ~/portable_obs_build_dir/release/bin && export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/portable_obs_build_dir/release/lib && ./obs
  ```
  Ну и... раз уж мы тут столько всего вытерпели, то вот вам нутро для desktop файла
  ```
  [Desktop Entry]
  Version=1.0
  Name=OBS Studio Portable
  GenericName=Streaming/Recording Software
  Comment=Free and Open Source Streaming/Recording Software
  Exec=bash -c "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/portable_obs_build_dir/release/lib && cd ~/portable_obs_build_dir/release/bin && ./obs --remote-debugging-port=9222 --remote-allow-origins=http://localhost:9222"
  Icon=com.obsproject.Studio
  Terminal=false
  Type=Application
  Categories=AudioVideo;Recorder;
  StartupNotify=true
  StartupWMClass=obs
  GenericName[ru_RU]=Приложение для потокового вещания и видеозаписи
  Comment[ru_RU]=Свободное и открытое ПО для потокового вещания и видеозаписи
  ```
- ### А можно OBS уже с патчем?
  [Репозиторий](https://github.com/4aiman/obs-studio.git) <br>
  [Сборка](https://github.com/4aiman/obs-studio/actions/runs/12528447589/artifacts/2368103516)
  
- ### Нерабочий Electron
  Даже если чистить пакеты nodejs, электрон не стартует, выдавая
  ```
  The SUID sandbox helper binary was found, but is not configured correctly. Rather than run without sandboxing I'm aborting now.
  ```
  А проблема в AppArmor. В каноникле решили поиздеваться и порезать права CEF на уровне ядра.<br>
  Чинится снятием тех ограничений, которые ввела убунта для AppImage'ей:
  ```
  sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
  ```
  На всякий случай, можно вернуть всё "взад", поставив единицу:
  ```
   sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=1
  ```
  Перманентно:
  ```
  echo 'kernel.apparmor_restrict_unprivileged_userns = 0' | sudo tee /etc/sysctl.d/20-apparmor-donotrestrict.conf
  ```

- ### Не монтируютя NTFS (выборочно)
  Криво выключили машину.
  Если из-под рута монтируется нахрапом, то можно попробовать отключить драйвер ядра ntfs вот так:
  ```
  sudo bash -c 'echo "blacklist ntfs3" > /etc/modprobe.d/disable-ntfs3.conf'  
  ```
  
- ### Наставил говна и всё поломал?
  Ну, хотя бы посмотри, что делал с в каком порядке, может и получится откатить
  ```
  grep " install " /var/log/dpkg.log
  ```

- ### Системный трей без иконок под цитрамоном?
  Хз как так (вот теперь *наверняка* из-за кучи DE), но вот это реально помогло:
  ```
  sudo apt remove indicator-application
  ```
  Походу, два приложения для показа индикаторов просто "дрались", кто первый, и побеждало не то.<br>
  Учитывая, что перезапуск цитрамона помогал, можно сделать вывод, что в нём используется далеко не самый шустрый и легковесный апплет (У меня цитрамон жрёт 800+ метров из 32Гб).

- ### Stable-Diffusion от Automatic не ставится
  Если не нужны дополнения типа reactor, deepboru и инструменты типа получения инфы по пнг - можно поставить [EasyDiffusion](https://easydiffusion.github.io/docs/installation/).

- ### Pipewire переодически отваливается
  Проблема в том, что ты сидишь и ВНЕЗАПНО звук перестаёт работать. Всякие интерфейсы для pipewire показывают, что источники на месте и даже связаны, но звука не.<br>
  Иногда помогает зайти в настройки звука и там поменять устройство на что-нибудь подключённое и обратно.<br>
  А иногда не помогает, и нужно отключить один из источников (поди угадай ещё какой именно) и заново "присоединить его "проводами". <br>
  
  В сети есть рекомендации поправить `50-alsa-config.lua` в папке с wireplumber или что-то шаманить в `/usr/share/pipewire/client.conf` (походу, устаревшая инфа), поставить там false в отключение node'ов. <br>
  Но так всё ещё быстрее засирается.

  В итоге я просто удалил pipewire и поставил pulse.

  Это поломало сеть.

- ### Поломанная сеть после установки pulse
  Задал IP вручную и перезагрузился.<br>
  При этом следтели настройки AppArmor (см. выше)

  Уж не знаю почему dhcp не работает, но `/usr/lib/systemd/system/NetworkManager-wait-online.service` ждёт, что `nm-online -s` выдаст нужный статус, и тогда запускается.
  <br>А nm-online ничего не выдаёт, т.к. не может подключиться.

  При этом `ip link` говорит, что всё UP, а демон dhcp установлен и запущен. Поди пойми.  
  
- ### OBS, Discord, и некоторые AppImage постоянно просят разблокировать keyring
  Я ***не*** знаю, как это исправить нормально, но проще всего убрать автологин в lightDM.<br>
  И, конечно же, интерфейс цитрамона не работает на удаление, т.к. кнопку `сохранить` выпилили за ненадобностью.<br>
  Давайте руками, чо:
  ```
  sudo nano /etc/lightdm/lightdm.conf
  ```
  В конфиге буквально 4 строчки, найдите что нужно и удалите.

- ### В Firefox пропали диалоги для открытия и сохранения файлов.
  Удаляем назхрен snap и ставим нормальный deb пакет из репы с оф. сайта мозиллы
  https://support.mozilla.org/en-US/kb/install-firefox-linux

- ### pulse начал "заикаться" (crackle)
  Похоже на постоянное прерывание звука в эмуляторе, когда тот не справляется, только по всей системе и даже когда нагрузки нет.

  1. Удаляем `speech-dispatcher` (убъёт TTS, в т.ч. конченный e-speaak и нормальный festvox)
  2. Копируем файл [default.pa](files/default.pa) по этому пути `/etc/pulse/default.pa` ([первоисточник](https://pastebin.com/raw/EJ2qvZvA), [вопрос на SO](https://askubuntu.com/questions/225444/how-to-make-pulseaudio-work-again) )

- ### не отключается заголовок у Firefox и VS-Codium
  1. Нужно удалить пакет который отключает `client-side borders gtk3+`. 
  2. Для огнелиса использовать стандартную настройку (ПКМ на закладках или месте с иконками расширений + "Настройка панели инструментов")
     ![изображение](https://github.com/user-attachments/assets/88192917-86b5-49db-aae4-5e5cf83ab34f)
  4. для кодиума перейти в настройки `File` -> `Preferences` -> `Settings` и поменять там пару параметров как на скрине:
     ![изображение](https://github.com/user-attachments/assets/8273ac69-2262-415e-a266-06b31438e71a)


- ### electron пеерстал сохранять позицию окна (или нреагировать на неё
  https://github.com/electron/electron/issues/526
  ```
    mainWindow.setBounds({
    width: 500,
    height: 1440,
	  x: 2060,
	  y: 0})
  ```
