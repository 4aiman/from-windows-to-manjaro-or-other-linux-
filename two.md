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

  Если стим стал прозрачным, значит какая-то херня с opengl. Отлкючайте GPU. `steam --disable-gpu %u` и потом в настройках

  ***ВНИМАНИЕ***<br>
  Папка Стима по-молчанию находится в `~/.local/share/steam`, но после обновления ставится snap версия, и всё переезжает в `~/snap/steam/common/.local/share/Steam`<br> Лучше отключить snap до того как начинаешь ставить всякое, иначе потом начнётся неразбериха откуда что ставить. Не убунтой единой. 
  
  Игры при этом проложают работать, включая перенос сейвов и SpecialK.

- ### Кстати, о SpecialK
  - Качаем себе [`SpecialK.7z`](https://github.com/SpecialKO/SpecialK/releases) из релизов
  - выбираем dllку в зависимости от разрядности игры
  - распаковываем её в папку с игрой (где экзэшник)
  - перименовываем dllку в `dxgi.dll` или что-нибудь ещё (см. репу SpecialK)
  - После первого запуска, в папке с игрой появится SK_Res - туда можно кидать текстуры для подмены
  - *А ещё это говно заставляет игры заикаться раз в секунду-другую, что на вине, что не линуксах. Кайф*

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

  Если ругается на `libfreetype6-dev`, ставьте просто `ibfreetype-dev`; а если не находит ффмпег, то проверяйте `ffmpeg -v`. Если оно 6.1, то смело удалите требования к версии в сборочных файлах (просто стираем выделенное).
  
  ![изображение](https://github.com/user-attachments/assets/0b45a89b-d194-4de4-9052-1a4dbe8628b0)



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

  DataChannel тоже не ставится из пакетов
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

  Не ссыте, систему не запорет (если все те пакеты и драйверы выше не запороли) - сборка портабельная.

  От сборки на арче/манжаро отличается тем, что бинарник один (а может это 31ая версия ОБС подосрала), так что нет больше в папке `bin` папки `64bit`. 

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
  Если не нужны дополнения типа reactor, deepboru и инструменты типа получения инфы по пнг - можно поставить [EasyDiffusion](https://easydiffusion.github.io/docs/installation/).<br>
  На самом деле, это был поломан сам stable diffusion. На момент написания всё норм. Только питон ставьте из репы "dead snakes" (гугл в помощь).

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
  ~~Я ***не*** знаю, как это исправить нормально~~.<br>
  *НЕФИГ ставить пароли в liveCD окружении, а потом менять их.
  Разблокировать keyring можно введя изначальный пароль, который задавали юзеру при установке ОС.*

  Если память уже не та, то проще всего убрать автологин в lightDM.<br>  
  И, конечно же, интерфейс цитрамона не работает на удаление, т.к. кнопку `сохранить` выпилили за ненадобностью.<br>
  Давайте руками, чо:
  ```
  sudo nano /etc/lightdm/lightdm.conf
  ```
  В конфиге буквально 4 строчки, найдите что нужно и удалите.
  

- ### В Firefox пропали диалоги для открытия и сохранения файлов.
  Удаляем нахрен snap и ставим нормальный deb пакет из репы с оф. сайта мозиллы
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


- ### electron пеерстал сохранять позицию окна (или не реагирует на неё)
  https://github.com/electron/electron/issues/526
  ```
    mainWindow.setBounds({
    width: 500,
    height: 1440,
	  x: 2060,
	  y: 0})
  ```

- ### Расширения cinnamon не работают после перезагрузки
  Нужно поменять список расширений

  1. заглянуть в `~/.local/share/cinnamon/extensions/`
  2. найти там `metadata.json` и посмотреть uuid
  3. поправить список uuid'шников в dconf по пути `/org/cinnamon/enabled-extensions`
  ![изображение](https://github.com/user-attachments/assets/2c9f465c-f34b-49bd-95f8-b6378eb346d0)

- ### Ya.Disk на линуксах в 2025
  - ```
    sudo apt install rclone
    rclone config
    ```
  - Выбрать 43 (яндекс)
  - пропустить вопросы про `client_id` и `client_secret`
  - дополнительно ничего не редактировать, согласиться на значения по-умолчанию
  - смонтировать куда-нибудь `rclone mount yandexdisk: $HOME/YaDisk --daemon`
  - пока хз как оно синхронизирует, но, похоже, не автоматом

- ### архиватор не спрашивает пароля у 7z
  Скачай подмени бинарник engrampa в /bin: https://ftp5.gwdg.de/pub/linux/archlinux/extra/os/x86_64/engrampa-1.28.2-2-x86_64.pkg.tar.zst
  
- ### Яндекс.Диск
  https://yandex.ru/support/yandex-360/customers/disk/desktop/linux/ru/installation + https://launchpad.net/~slytomcat/+archive/ubuntu/ppa

  Ставил deb (без репы) и настраиваем через консоль - попросит открыть страницу устройств в браузере, где уже есть авторизация яндекса и ввести там код в течении 5 минут <br>
  ![Screenshot_09 01 25 - 18_18_05](https://github.com/user-attachments/assets/863457f3-8029-4e84-9877-ea7d6c163a72)
  

- ### nemo работает с file-roller вместо engrampa, хоть и написан на прошлом гноме
  Ставим древний плагин из https://github.com/juanfgs/nemo-engrampa ([форк](https://github.com/4aiman/nemo-engrampa)), собираем (клонируем + `./autoconf` + `./configure` + `make` + `sudo make install`) и радуемся.

  ```
  Libraries have been installed in:
     /usr/lib/x86_64-linux-gnu/nemo/extensions-3.0
  
  If you ever happen to want to link against installed libraries
  in a given directory, LIBDIR, you must either use libtool, and
  specify the full pathname of the library, or use the '-LLIBDIR'
  flag during linking and do at least one of the following:
     - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
       during execution
     - add LIBDIR to the 'LD_RUN_PATH' environment variable
       during linking
     - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
     - have your system administrator add LIBDIR to '/etc/ld.so.conf'
  
  See any operating system documentation about shared libraries for
  more information, such as the ld(1) and ld.so(8) manual pages.
  ----------------------------------------------------------------------
  ```

  - ### Big Picture Mode в Steam лагает
    - зайти в режим big picture
    - опции - дисплей - отключить чёрный список GPU
      
    можно ещё попробовать включить аппаратное ускорение в настройках стима в режиме big picture (не помню где, но оно выключено на убунте)

  - ### Цветовая индикация температуры GPU
    Для Ubuntu 24.04 и Palit RTX 4070 нужно поставить *как минимум* экспериментальный openrgb (`OpenRGB Pipeline (Experimental)`) [отсюда]([https://openrgb.org/index.html](https://gitlab.com/CalcProgrammer1/OpenRGB/-/jobs/artifacts/master/download?job=Linux+amd64+.deb+%28Debian+Bookworm%29)) (релиз для Debian Bookworm).<br>
    Экспериментальный релиз только потому, что стабильный 0.9 вышел ещё в 2023 и с тех пор всем ссыкотно "ломать" 4 и 5 линейки карточек от нвидии.
    
    [Там же](https://openrgb.org/releases/plugins/hardwaresync/release_0.9/OpenRGBHardwareSyncPlugin_0.9_Bookworm_64_dcefdd5.so) (ну, почти) надо раздобыть плагин `OpenRGB Hardware Sync Plugin`, тоже под Debian Bookworm.<br>
    Плагин ставится прямо из OpenRGB, если ходили по ссылкам, то проблем быть не должно.

    В теории, openrgb собирается без особой головной боли, но лучше брать готовое, если такой вариант есть.

    Для настройки трогать вкладку "Устройства" не нужно. Там можно просто настроить один цвет, но нам это ни к чему.

    У Palit RTX 4070 обнаруживается два региона светодиодов.<br>
    Но по факту мы видим лишь RGBW, а SingleColor не используется.

    Учитывая "потолок" в 93 градуса, вот скрин примерных настроек для того, чтобы:
      - при 40 градусах цвет сменялся на жёлтый
      - при 63 на красный
      - при 75 на фиолетовый
 
    Переходы плавные, а яркость настраивается.
        
    ![изображение](https://github.com/user-attachments/assets/ac32a5a0-a55b-4543-9937-b4910786059f)

    Для автозагрузки можно потыкать тут

    ![изображение](https://github.com/user-attachments/assets/24b270fc-01c9-4d15-bf58-0f943ecadb8f)


- ### Подстройка вентиляторов под температуру GPU
  Клон репы [тут](https://github.com/4aiman/nvidia_fan_control_linux).

  Читаем ридми и делаем.

  Выглядеть должно вот так:
  ```
	============================================================
	Driver Version: 550.120
	GPU 0: NVIDIA GeForce RTX 4070
	Fan Count: 2
	============================================================
	Temperature: 37°C
	Total Curve Point: 4
	Current Curve Point: 0
	Previous_Curve_Point: 0
	Next_Curve_Point: 1
	Fan_Speed: 38%
	============================================================
	Temperature_Delta: 40
	Fan_Speed_Delta: 30
	Temperature_Increment: 37
	Fan_Speed_Increment: 27.75
	Previous_Temperature: 37°C
	Step_Down_Temperature: 32
	============================================================
   ```
   Если интересно, то моя кривая выглядит так (я подправил значения в скрипте):
   ![изображение](https://github.com/user-attachments/assets/3f1de690-20e7-4276-aa31-ebc2e93a1a9f)

   Результат:
  	- 34 градуса на GPU "в простое", когда открыт только браузер да что-то по работе, вентиляторы почти не шумят
  	- 64 градуса при бенчмарке FurrMark в QHD на вулкане и ревущими на 90% вентиляторами
   
   Кому-то, может, и покадется шумным, а мне главное не перегреть карточку, особенно летом, когда за окном и 40 бывает.

- ### Engrampa не видит настройку, которую не создавала
  ```
  (engrampa:178307): GLib-GIO-ERROR **: 11:31:20.978: Settings schema 'org.mate.engrampa.dialogs.extract' does not contain a key named 'create-subdirectory'
  Ловушка трассировки/останова (образ памяти сброшен на диск)
  ```
  Ничего не помогало, пока не собрал engrampa руками.
  Локализации нет, ну и хер с ней.
  ![изображение](https://github.com/user-attachments/assets/274a282c-9050-4297-8987-bf14a4f42825)

- ### Нет заголовков для libsdl2
  ХЗ как так, но libsdl2-dev зависит от lib-decor-0. Сам декор ставится нормально, но libsdl2-dev всё равно ломается из-за libgbm1.

  *** НЕ ДАЙ ГРАБЛИ *** ВАМ УДАЛИТЬ ПРОБЛЕМНЫЙ libgbm1 через какой-нибудь `sudo dpkg --force-all -P libgbm1`!!!

  Сломается хром, кодиум, стим, вэйлэнд (и половина гуя, включая цитрамон, если попытаетесь решить всё через пакетный менеджер).

  Помогает установка sdl2 из сырцов.
  ```
  git clone https://git.launchpad.net/ubuntu/+source/libsdl2
  cd libsdl2
  ./autogem.sh
  ./configure
  make -j12
  sudo make install
  ```
  Если libgbm1 всё-таки потёрли, то возьмите из реп убуинты что-то типа `libgbm1_24.0.9-0ubuntu0.3_amd64.deb`
  
- ### Задолбали обновления дискорда

  Я решил вопрос [скриптом](https://github.com/4aiman/discord_updater)

  Скрипт берёт текущую версию дискорда из файла `version.txt` (на момент написания 93), добавляет 1, и пытается скачать обновку.<br>
  Если получилось - обновка распаковывается и копируется с заменой на место предыдущей.<br>
  Если не получилось - скачаннго файла не будет, `version.txt` не обновится, дискорд запустится старый (текущей версии).

  Как добавить в автозагрузку - личное дело каждого.

- ### локальная почта с TLS
  https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-20-04 

- ### фризы и дёрганье в играх
   1. Качаем dxvk и кладём в папку с игрой https://github.com/doitsujin/dxvk/releases
   2. Идём в настройки нвидии и ставим принудительные галки:
      ![изображение](https://github.com/user-attachments/assets/0c09b600-19e3-425c-ac87-094697e82f39)
   3. Ставим максимальный режим производительности в настройках nvidia
   4. Закрываем плавающие окна браузеров с видео, в которых источник в значительно меньшем разрешении, чем монитор (360p тормозит всё на 2к экране)
   5. Ставим low-latency ядро для nvidia + общие модули (смотри по номеру рядом со словом nvidia)
      ![изображение](https://github.com/user-attachments/assets/ea196760-4d04-4004-a527-9a81dfc42dc6)
   6. Удаляем апплет погоды
   7. ВСЁ РАВНО ИМЕЕМ ФРИЗЫ!!!
   8. Сносим цитрамон, ставим xfce - не помогает
   9. удаляем dxvk из папки с игрой + отключаем VSYNC - О ЧУДО!!!-


- ### расшифровка токенов
  По долгу службы приходится иметь с этим дело.<br>
  Решение - закинуть команду ниже в текстовый файл, дать права на выполнение, и просто дёргать `путь/tokendecode TOKEN`
  ```
  jq -R 'split(".") | .[1] | @base64d | fromjson | 
    . as $original | 
    { 
        iat: (try ($original.iat | todate) // null), 
        exp: (try ($original.exp | todate) // null), 
        nbf: (try ($original.nbf | todate) // null), 
        auth_time: (try ($original.auth_time | todate) // null) 
    } + ($original | del(.iat, .exp, .nbf, .auth_time))' <<< "$1"
  ```
  Конвертит JWT в джейсон + форматит даты в нормальный вид из штмапов.
  
- ### Кривой курсор в телеге ни с того ни с сего.

  Выглядит сие убожество так:<br>
  ![изображение](https://github.com/user-attachments/assets/e438a280-02f3-488e-84be-61df72a78489)

  Мне помогло создание скрипта для запуска с вот таким экспортом перед вызовом телеги:
 ```
 export XCURSOR_PATH=$RUNTIME/usr/share/icons
 ```
 Кроме того, телега сама добавляется в автозапуск *напрямую*, так что придётся её оттуда убрать и поместить в автозагрузку вручную написанный скрипт
 
- ### Прозрачный Steam
  
  Спасёт вас только  `steam -cef-disable-gpu`б причём оно работало нормально до последнего апдейта, числа десятого июня 2025

- ### Починить симлинки после перемещения папки
  
  `find . -type l -exec sh -c 'lnk="{}"; target="$(readlink '{}' | sed 's#/ОТКУДА#/КУДА#')"; unlink "${lnk}"; ln -s "${target}" "${lnk}" ' \;`

- ### cargo can't fetch a file that's accessible https://index.crates.io/ta/ur/tauri-plugin-dialog

  https://github.com/rust-lang/cargo/issues/13457#issuecomment-2012259159

  > If someone is still going through this issue on Windows, please try by adding registries.crates-io.protocol = "git" to /.cargo/config.toml. It worked for me.
  
  YES, it's EMPTY by default.
  
- ### долбанный экран вырубает, хоть и сказано НЕ выубаться

  https://wiki.archlinux.org/title/Display_Power_Management_Signaling

- ### Не играют видосы в играх Ys I, II, IV, VI

  1. Ставим gstreamer плагины для 32х  
    ```
	sudo apt install gstreamer1.0-libav:i386
	sudo apt install gstreamer1.0-plugins-bad:i386
	sudo apt install gstreamer1.0-plugins-base:i386
	sudo apt install gstreamer1.0-plugins-good:i386
    ```
  2. Удаляем gstreamer-vaapi
  3. ставим CCCPack кодеков
  
- ### emoji xfce
  
  https://github.com/efck-chat-keyboard/efck/releases
  
- ### Система теряет аудиокарту (usb) при запуске и если долго нет звука

  Симптомы:
    - звук воспроизводится только на других картах
    - звук воспроизводится только если выключен микрофон (аналоговое стерео, вместо стерео + микрофон)
    - не грузится ютуб и вообще любые видео и аудио (грузятся, просто не могут воспроизвестись
    - `journalctl --user-unit=pulseaudio` выдаёт `PulseAudio information vanished from X11!`
    - `pulseaudio -k` говорит, что "нет такой ноты"
    - `sudo killall pulseaudio` говорит, что "нет такой ноты"
    - `pulseaudio --check` ни к чему не приводит
    - `pulseaudio --start` ни к чему не приводит
    - игры с конфигом пульса, группами и udev правилами ни к чему не приводят

  Причина: у тебя завёлся ***pipewire***!!!

  Решение: Ставь `ubuntustudio-pulseaudio-config`
  ```
  grep -e " install " -e " remove " /var/log/dpkg.log | awk '{print $3,$4}'
  ```
  ```
  remove gstreamer1.0-pipewire:amd64
  remove libpipewire-0.3-common:all
  remove libpipewire-0.3-dev:amd64
  remove pipewire-pulse:amd64
  remove wireplumber:amd64
  remove pipewire:amd64
  remove pipewire-bin:amd64
  remove libpipewire-0.3-modules:amd64
  remove libwireplumber-0.4-0:amd64
  install pulseaudio-module-jack:amd64
  install pulseaudio-module-bluetooth:amd64
  install a2jmidid:amd64
  install libzita-alsa-pcmi0t64:amd64
  install libzita-resampler1:amd64
  install jackd2:amd64
  install python3-ply:all
  install python3-pycparser:all
  install python3-cffi:all
  install python3-jack-client:all
  install zita-ajbridge:amd64
  install qastools-common:all
  install qasmixer:amd64
  install python3-alsaaudio:amd64
  install polkitd-pkla:amd64
  install studio-controls:all
  install ubuntustudio-pulseaudio-config:all
  install jackd:all
  install libconfig++9v5:amd64
  install libxml++2.6-2v5:amd64
  install libffado2:amd64
  install jackd2-firewire:amd64
  install pulsemixer:all
  install qjackctl:amd64
  install qmidinet:amd64
  install zita-njbridge:amd64
  ```

- ### Белые окна в Хроме - куча ctrl+n иногда (50/50) приводят к открытия окна с неверным режимом blending

  Это какая-то херня с дровами нвидии + Хромом. В других приложениях OpebGL нормально себя ведёт. Переключи Хром на вулкан.
  ![изображение](https://github.com/user-attachments/assets/f735b681-389e-41f9-bc09-c9f32e310915)

- ### Не работает плагин погоды в xfce
	#### устанавливаем пререквизиты
	`sudo apt build-dep xfce4-weather-plugin`
	
	#### качаем исходники (должно быть включено в источниках)
	`apt source xfce4-weather-plugin
	cd xfce4-weather-plugin-0.11.2`
	
	#### меняем урл апи с api.met.no на нужный aa062reffgwvo1efa.api.met.no (в 0.11.3 такой же)
	`sed 's#https://api.met.no#https://aa062reffgwvo1efa.api.met.no#' -i panel-plugin/weather.c
	EDITOR=true dpkg-source --commit . backport_changed_api_url.patch`
	
	#### собираем пакет
	`dpkg-buildpackage -us -uc -ui`

	#### готовый пакет доступен **на уровень выше**, чем папка, где проделывались манипуляции по сборке
  	`ddeb` - то отладочный пакет, рядом есть нормальный `deb`

- ### Выставить конкретный размер окна по горячей клавише

  1. Берём команду
     ```sh
     wmctrl -r ":ACTIVE:" -e "0,$(xdotool getactivewindow getwindowgeometry|egrep -o '[0-9]+,[^ ]+'),1920,1080"
     ```
     и пихаем её в файл
  2. Добавляем шорткат
  3. В цитрамоне будет постоянно сбрасываться, говно. Придётся хаходить в настройки и перерегистрировать шорткат. ХЗ почему так.

- ### Nemo перстал открывать скрипты в xfce4-terminal

  ![изображение](https://github.com/user-attachments/assets/0259d5d8-e0e9-41f9-9f6b-5d535933bcf9)

  Прчём, если сделать
  ```
  gsettings set org.cinnamon.desktop.default-applications.terminal exec 'xfce4-terminal'
  ```
  то или ничего вообще не происходит, или открыввается xterm (он же открывается, если указать TERMINAL)

  Решение:
  ```
  gsettings get org.cinnamon.desktop.default-applications.terminal exec
  gsettings get org.cinnamon.desktop.default-applications.terminal exec-arg
  ```
  и теперь всё ок!
  
- ### Le Potato fsck
  
  1. Set fsck to scan on every boot:
     ```tune2fs -c 1 /dev/mmcblk0p1```
  2. Add bellow line to `/boot/armbianEnv.txt`
     ```extraargs=fsck.mode=force fsck.repair=yes```

- ### Пропали смайлики в xfce

  Как бы странно ни было, решается это через `qt5ct`.<br>
  Надо выставить в нём `Noto Sans` и нажать `Создать font.conf`
  <img width="740" height="176" alt="изображение" src="https://github.com/user-attachments/assets/870ae02a-f865-4517-ae04-a9bc8c95811b" />

  После этого смайлы вернутся в xfce4, включая панели, терминал, редакторы, адресную браузера - везде.
  
- ### свежий xfce
  Убунта имеет в репах версию 4.18, а в хубунте есть уже 4.20 - 4.23 (в зависимости от компонента). Так что почему бы и нет?

  Из плюсов: рабочий лагин погоды, компактная шапка в Тунаре, дерево папок в детальном посмотре (как в Nemo)<br>
  `sudo add-apt-repository ppa:xubuntu-dev/staging`
