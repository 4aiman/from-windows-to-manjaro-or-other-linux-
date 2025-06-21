# build obs from source
Install:
```
linux-headers (I have linux61-headers-6.1.60-1)
v4l2loopback-dkms (v4l2loopback-dkms-0.12.7-2 for me)
ninja cmake curl ccache zsh qrcodegencpp-cmake hiredis pkgconf libdatachannel libjuice rav1e-git dav1d-git decklink
```
Download and unpack CEF (for browse source) + clone and build obs in portable mode:

```
clear && cd "$HOME" && rm -rf "$HOME/portable_obs_build_dir" && mkdir -p "$HOME/portable_obs_build_dir/" && cd "$HOME/portable_obs_build_dir" && wget https://cdn-fastly.obsproject.com/downloads/cef_binary_5060_linux64.tar.bz2 && tar -xjf ./cef_binary_5060_linux64.tar.bz2 && git clone https://github.com/obsproject/obs-studio --recursive && cd obs-studio && rm -rf build && rm -rf "$HOME/portable_obs_build_dir/release" && mkdir build && cd build && cmake -DLINUX_PORTABLE=ON -DCMAKE_INSTALL_PREFIX="$HOME/portable_obs_build_dir/release/" -DENABLE_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_5060_linux64" -DENABLE_AJA=ON -DTWITCH_HASH=0 -DTWITCH_CLIENTID=selj7uigdty0j5ijt41glcce29ehb4 -DOAUTH_BASE_URL=https://auth.obsproject.com/ -DENABLE_BROWSER_PANELS=ON -DENABLE_DECKLINK=ON -DENABLE_FREETYPE=ON -DENABLE_HEVC=ON -DENABLE_JACK=ON -DENABLE_LIBFDK=ON -DENABLE_NEW_MPEGTS_OUTPUT=ON -DENABLE_PIPEWIRE=ON -DENABLE_PLUGINS=ON -DENABLE_PULSEAUDIO=ON -DENABLE_QSV11=ON -DENABLE_RNNOISE=ON -DENABLE_SCRIPTING=ON -DENABLE_SCRIPTING_LUA=ON -DENABLE_SCRIPTING_PYTHON=ON -DENABLE_SERVICE_UPDATES=ON -DENABLE_SNDIO=ON -DENABLE_SPEEXDSP=ON -DENABLE_UI=ON -DENABLE_V4L2=ON -DENABLE_VLC=ON -DENABLE_VST=ON -DENABLE_WAYLAND=ON -DENABLE_VST_BUNDLED_HEADERS=ON -DENABLE_WEBRTC=ON -DENABLE_WEBSOCKET=ON -DBUILD_TESTS:BOOL="1" -DENABLE_JACK:BOOL="1" -DENABLE_SNDIO:BOOL="1" -DENABLE_RTMPS:STRING="ON" -DCMAKE_BUILD_TYPE:STRING="RelWithDebugInfo" -DCALM_DEPRECATION:BOOL="1" -DENABLE_LIBFDK:BOOL="1" -DENABLE_UDEV=ON .. && make -j12 && make install && cd "$HOME/portable_obs_build_dir/release/bin/64bit/" && ./obs
```

Remove libva-vdpau-driver (segfaults obs on load don't know why):
```sudo pamac remove libva-vdpau-driver```

In the end rav1e-git and dav1d-git will probably complain about ffmpeg depending on them, but all three work just fine.
I've used a combination of pacman, pamac and pamac GUI to install all the needed things, cause none seect one would onlieg and do what I wanted it to do. 


# Get back the ability to free webcams/capture cards when not in active use

As of 4th of December 2023 OBS on Linux (v 30.0!!!) still has no way to disable v4l2 devices when those are not in active use. <br>
The most baffling thing is that the patch was made by [julijane](https://github.com/obsproject/obs-studio/pull/3159/files) almost ***four years ago***.
Insane, right?
 
Anyway, this patch is for OBS 30.0 (more specifically the master branch) and all it does is gets in Julijane's changes.
Clone OBS, apply the [patch](files/Add%20deactivate%20button%20to%20v4l2%20devices%20--%20pr%233159%20--%20patch%20for%20OBS%2030.0%20with%20changes%20from%202020%20by%20julijane.patch)
via git and build. Onec done, you'll have that sweet "Disable when not showing" checkbox you love so much back.

In case you have no idea how to apply patches:

```
# Switch to the branch where the changes should be applied
$ git checkout master

# Apply the patch
$ git am bugfix.patch

# Check what has happened in the commit log
$ git log 
```


# remove flatpak support
removing flatpaks from manjaro be like:

`$ flatpak list`

Remove a named app
`$ flatpak uninstall io.lbry.lbry-app`

Uninstall unused flatpaks
`$ flatpak uninstall --unused`

Uninstalling apps may not remove everything - so how much is left?
`du -ch /var/lib/flatpak`

If the total is more than a few MB you may want to cleanup leftovers
`sudo flatpak repair`

Disable flatpak support
`$ sudo nano /etc/pamac.conf`

Modify the last lines to look like below and save the changes (you will also find snap support) but as this is about flatpak leave as is.
  ```#CheckFlatpakUpdates
  #EnableFlatpak
```

Then remove flatpak packages using pacman
`$ sudo pacman -Rns flatpak libpamac-flatpak-plugin`

Then update your system
`$ sudo pacman -Syu`

Finally restart your system


# fix icons/emoji in discord's channel names
https://fonts.google.com/noto/specimen/Noto+Color+Emoji


# windows-10/11-like screenshots
put this into an sh file, and call using a kbd shortcut
```xfce4-screenshooter --region --clipboard --save ~/screenshots/"Screenshot $(date -d "today" +"%d.%m.%y - %H_%M_%S").png"```


# fix back/forward button for your mouse in Thunar:
Open up windows manager, in kbd settings find back/forward, edit those.
When aked to press a button - use your mouse


# fix your windows fonts
just copy font from the ex~ c:/windows/fonts and c:/Users/<your username>/AppData/Local/Microsoft/Windows/Fonts/ into ~./fonts
in my case some fonts were locked as read-only. Just chmod 777 -R . in your ~./fonts


# fix vscode build:
`create writable ~/.cache/yarn/`

# am I on x11 or wayland?
`echo $XDG_SESSION_TYPE `


# build nvidia vfx on Manjaro
***USELESS***
- get all packages (vfx, afx, sdk, cuda, cudnn, tensor, opencv) https://catalog.ngc.nvidia.com/orgs/nvidia/teams/maxine/collections/maxine

libcuda, libvinfer, libcdnn, libVideoFX
export LD_LIBRARY_PATH=/opt/cuda-11.7/lib64:/usr/local/TensorRT-8.6.1.6/lib:/usr/lib64:/usr/local/VideoFX/lib:/opt/cuda/lib64:/usr/local/TensorRT-8.6.1.6/lib:/usr/lib64:/usr/local/VideoFX/lib:/usr/local/VideoFX/lib/models:/usr/local/VideoFX/share:/usr/include/opencv4/opencv2$LD_LIBRARY_PATH
export _VFX_MODELS=/usr/local/VideoFX/lib/models
export _VFX_SHARE=/usr/local/VideoFX/share
export _OPEN_CV4=/usr/include/opencv4/opencv2
export CUDA_INCLUDE_DIRS 

export PATH=`pwd`:/opt/cuda-11.7/bin:/usr/local/TensorRT-8.6.1.6:/usr/local/VideoFX:/opt/cuda/bin:/usr/local/TensorRT-8.6.1.6:/opt/cuda/bin:/usr/local/TensorRT-8.6.1.6/bin:/usr/local/VideoFX/bin:$PATH

pacman -Sy vtk lvtk
pamac install catch2 - required by h5cppp
pamac install h5cpp - failed
pamac install h5utils-git - worked
pamac install llm-cublas-git -- failed on rust
pamac install rustup - removed rust, replaced with rustup - failed
installed pamac install llm-cublas-git after making ~./rustup 777 
sudo pamac install cuda-11.7

need opencv with cuda support, not the plain one


# fix rust
failed to create directory `/.cargo/registry/cache/index.crates.io-6f17d22bba15001f`
means don't do it under root, stupid!
create the folder ~./cargo and use sudo on whichever command you're trying to issue.
If you're inside sudo mc - it'll freak out like that, demanding a place in / #oof



# fix GPU fan running at 50C by manually controling fans
- install nvfancontrol
- setup it in ./config/nvfancontrol.conf with "temp speed" values
- add this thing to /home/USER/.config/autostart/
[Desktop Entry]
Type=Application
Name=NV fan control
Comment=Enables windows-like GPU fan control. ~./config/nvfancontrol.conf has temp-speed settings.
Exec=nvfancontrol
Terminal=false
Categories=System


# fix telegram-desctop needing protobuf v25:
sudo pacman -Syu
sudo pacman-mirrors -f10
sudo pacman -Syyu


#fix bright as shit pamac-gui
install pamak-gtk3, that uses gtk3 themes 


# windirtree replacement
qdirtree

# fix brightness/contrast:
make ~.nvidia-settings-rc read-only

# fix stuttering in games:
- get mangohud and goverlay
- in goverlay:
    - disable vsync
    - set framecap to match your monitor
    - remove/apply dxvk

#nvidia led 
install that shit if you have nvidia LOGO on your GPU.
Else hope that OpenRGB works.
Their support is meh at best, tho

sudo make install


# No object for D-Bus interface

NTFS partitions can be that way even with no errors on them

`systemctl --user restart gvfs-udisks2-volume-monitor`

Also, 
- First, go to Dash (for Ubuntu) or run gparted using superuser, preferrably gparted-pkexec.
- Right click the partition, choose New UUID.
- Click the Apply button.

Update: Some people claimed that just by having gparted refreshing information, the problems are solved. You should try that first, as refreshing UUID screws up fstab.



# no scrollbar buttons anywhere!
fnid your theme in usr/share/themes and search for something like this in main.rc or main.css:

```
GtkScrollbar::has-backward-stepper = 1 (change value from 0 to 1)
GtkScrollbar::has-forward-stepper = 1 (change value from 0 to 1)
```

If you encounter any imports from a resource:// schema, go there in Thunar and search for "stepper" to know what to add.

On gtk2 it looks like described above.
On gtk3 it looks like this:
 
```
* { -GtkScrollbar-has-backward-stepper: true; -GtkScrollbar-has-forward-stepper: true; }
```

Experiments are your friends now.


# system-wide noise cancellation
install `pulse-effects` (pulse) or `easyeffects` (pipewire)
This helped me: [noise gate + RRNOISE + maximizer.json](files/noise%20gate%20+%20RRNOISE%20+%20maximizer.json)


# Clicking scrollbar makes it jump there instead of page up/down

Edit (or create) the file:

~/.config/gtk-3.0/settings.ini

And add the following:

[Settings]
gtk-primary-button-warps-slider = false


.

# my controller isn't working in games (but works in menus and/or Steam)
https://old.reddit.com/r/LinuxCrackSupport/comments/wnt7lh/proper_method_to_get_spiderman_remastered_running/
