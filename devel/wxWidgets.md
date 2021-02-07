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
mkdir build-x86_64-darwin-debug
cd build-x86_64-darwin-debug
../configure --enable-debug
make
```

##### Release

```bash
mkdir build-x86_64-darwin-release
cd build-x86_64-darwin-release
../configure
make
```

#### MSYS2

##### Debug

```bash
mkdir build-x86_64-mingw-debug
cd build-x86_64-mingw-debug
../configure --with-msw --enable-debug --disable-precomp-headers
make
```

##### Release

```bash
mkdir build-x86_64-mingw-release
cd build-x86_64-mingw-release
../configure --with-msw --disable-precomp-headers
make
```
