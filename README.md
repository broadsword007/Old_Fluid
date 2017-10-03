Fluid
=====
Fluid is a collection of cross-platform QtQuick components for building fluid and dynamic applications. This is an old backup of Fluid. You can find the latest Fluid repo * [here](* [qtbase](http://code.qt.io/cgit/qt/qtbase.git))

## Dependencies

Qt >= 5.8.0 with at least the following modules is required:

 * [qtbase](http://code.qt.io/cgit/qt/qtbase.git)
 * [qtdeclarative](http://code.qt.io/cgit/qt/qtdeclarative.git)
 * [qtquickcontrols2](http://code.qt.io/cgit/qt/qtquickcontrols2.git)
 * [qtgraphicaleffects](http://code.qt.io/cgit/qt/qtgraphicaleffects.git)
 * [qtsvg](http://code.qt.io/cgit/qt/qtsvg.git)

## System-wide installation

### Build with QMake

From the root of the repository, run:

```sh
mkdir build; cd build
qmake ../fluid.pro
make
make install # use sudo if necessary
```

On the `qmake` line, you can specify additional configuration parameters:

 * `LIRI_INSTALL_PREFIX=/path/to/install` (for example `/opt/liri` or `/usr`)
 * `CONFIG+=debug` if you want a debug build
 * `CONFIG+=install_under_qt` to install plugins and QML modules inside Qt

Use `make distclean` from inside your `build` directory to clean up.
You need to do this before rerunning `qmake` with different options.

### Notes on installation

A system-wide installation with `LIRI_INSTALL_PREFIX=/usr` is usually performed
by Linux distro packages.

In order to avoid potential conflicts we recommend installing under `/opt/liri`,
but this requires setting some environment variables up.

First build and install:

```sh
mkdir build; cd build
qmake LIRI_INSTALL_PREFIX=/opt/liri ../fluid.pro
make
sudo make install
```

Then create a file with the environment variables as `~/lenv` with the following contents:

```sh
LIRIDIR=/opt/liri

export LD_LIBRARY_PATH=$LIRIDIR/lib:$LD_LIBRARY_PATH
export XDG_DATA_DIRS=$LIRIDIR/share:/usr/local/share:/usr/share:~/.local/share:~/.local/share/flatpak/exports/share
export XDG_CONFIG_DIRS=$LIRIDIR/etc/xdg:/etc/xdg
export QT_PLUGIN_PATH=$LIRIDIR/lib/plugins
export QML2_IMPORT_PATH=$LIRIDIR/lib/qml:$QML2_IMPORT_PATH
export PATH=$LIRIDIR/bin:$PATH
```

Source the file (we are assuming a bash shell here):

```sh
source ~/lenv
```

And run `fluid-demo` to test:

```sh
fluid-demo
```

## Per-project installation using QMake

You can embed Fluid in your project and build it along your app.

First, clone this repository.

In your project file, include the `fluid.pri` file:  
```qmake
include(path/to/fluid.pri)
```

Then, in your `main.cpp` file or wherever you set up a `QQmlApplicationEngine`:
* add this include directive:
```cpp
#include "iconsimageprovider.h"
#include "iconthemeimageprovider.h"
```
* add the resources files to your `QQmlApplicationEngine` import paths:
```cpp
engine.addImportPath(QLatin1String("qrc:/"));
```
* register fluid's image providers to the engine:
```cpp
engine.addImageProvider(QLatin1String("fluidicons"), new IconsImageProvider());
engine.addImageProvider(QLatin1String("fluidicontheme"), new IconThemeImageProvider());
```
* and after that you can load your qml file:  
```cpp
engine.load(QUrl(QLatin1String("qrc:/main.qml")));
```

## Documentation

Build the HTML documentation from the `build` directory created earlied:

```sh
cd build
make html_docs_fluid
```

Then open up `doc/fluid/html/index.html` with a browser.

## Licensing

Licensed under the terms of the Mozilla Public License version 2.0.
