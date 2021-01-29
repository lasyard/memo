# macOS

## Catalina

### homebrew

```bash
brew tap
brew update
brew upgrade
brew install ...
brew info ...
brew remove ...
brew cleanup -s
```

#### set repo

```bash
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles
```

### xcode-select

```bash
xcode-select -p
xcode-select --install
```

### pkgutil

```bash
pkgutil --pkgs
pkgutil --pkg-info com.apple.pkg.CLTools_Executables
```
