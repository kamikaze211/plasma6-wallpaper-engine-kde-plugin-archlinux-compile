# plasma6-wallpaper-engine-kde-plugin-archlinux-compile
a compiling tutorial for wallpaper-engine-kde-plugin on archlinux

#安装依赖
```
sudo pacman -S extra-cmake-modules plasma-framework5 gst-libav base-devel mpv python-websockets qt5-declarative qt5-websockets qt5-webchannel vulkan-headers cmake
```

#删除以前编译安装过的资源文件（系统+家目录）
#之前某个名字打错的版本
```
rm -rf ~/.local/share/plasma/wallpapers/com.github.catsout.wallpaperEngineKde
rm -rf ~/.local/share/kpackage/generic/com.github.catsout.wallpaperEngineKde/
sudo rm -rf /usr/share/plasma/wallpapers/com.github.catsout.wallpaperEngineKde/
sudo rm -f /usr/lib/qt6/qml/com/github/catsout/wallpaperEngineKde/libWallpaperEngineKde.so
sudo rm -rf /usr/lib/qt6/qml/com/github/catsout/

rm -rf ~/.local/share/plasma/wallpapers/com.github.casout.wallpaperEngineKde
rm -rf ~/.local/share/kpackage/generic/com.github.casout.wallpaperEngineKde/
sudo rm -rf /usr/share/plasma/wallpapers/com.github.casout.wallpaperEngineKde/
sudo rm -f /usr/lib/qt6/qml/com/github/casout/wallpaperEngineKde/libWallpaperEngineKde.so
sudo rm -rf /usr/lib/qt6/qml/com/github/casout/
```

#删除后先重启下plasmashell，避免出现问题？
```
systemctl --user restart plasma-plasmashell.service
```


# 切换分支下载源码
```
git clone --branch=fix-qt6-mousehover https://github.com/Thadah/wallpaper-engine-kde-plugin
cd wallpaper-engine-kde-plugin
```

# 下载submodule
```
git submodule update --init --force --recursive
```

# 配置，构建，安装
# 'USE_PLASMAPKG=ON': using kpackagetool tool to install plugin
```
cmake -B build -S . -GNinja -DUSE_PLASMAPKG=ON
sudo cmake --build build
cmake --install build
```

# Install package (ignore if USE_PLASMAPKG=OFF for system-wide installation)
```
cmake --build build --target install_pkg
```

# 编译场景壁纸插件
```
cd src/backend_scene/standalone_view
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_QML=ON
make -j$nproc
```
#检查场景壁纸插件是否编译成功
```
./sceneviewer --help
```

#重启plasmashell，以显示在壁纸菜单中
```
systemctl --user restart plasma-plasmashell.service
```
