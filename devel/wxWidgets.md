# wxWidgets

## 3.1.4

```bash
wget https://github.com/wxWidgets/wxWidgets/releases/download/v3.1.4/wxWidgets-3.1.4.tar.bz2
```

```bash
tar -C ~/workspace/devel/ -xjf wxWidgets-3.1.4.tar.bz2
cd ~/workspace/devel/wxWidgets-3.1.4
```

### Build

#### macOS Catalina

##### Debug

```bash
mkdir build-cocoa-debug
cd build-cocoa-debug
../configure --enable-debug
make
```

##### Release

```bash
mkdir build-cocoa-release
cd build-cocoa-release
../configure
make
```

#### MSYS2

##### Debug

```bash
mkdir build-x86_64-msw-debug
cd build-x86_64-msw-debug
../configure --with-msw --enable-debug --disable-precomp-headers
make
```

##### Static Release

```bash
mkdir build-x86_64-msw-static-release
cd build-x86_64-msw-static-release
../configure --with-msw --disable-shared --disable-precomp-headers
make
```
