# cryptopp

## 8.4

```bash
wget https://www.cryptopp.com/cryptopp840.zip
wget https://www.cryptopp.com/cryptopp840.zip.sig
```

### Build

#### macOS Catalina

##### Release

```bash
unzip -d ~/workspace/devel/cryptopp840-x86_64-darwin-release cryptopp840.zip
cd ~/workspace/devel/cryptopp840-x86_64-darwin-release
```

```bash
make all
# Fix dylib id
install_name_tool -id $(pwd)/libcryptopp.dylib libcryptopp.dylib
```

#### MSYS2

##### Release

```bash
unzip -d ~/workspace/devel/cryptopp840-x86_64-mingw-release cryptopp840.zip
cd ~/workspace/devel/cryptopp840-x86_64-mingw-release
```

```bash
make all
```
