# Mac Setup

## No Internal Speaker

reset PRAM - hold option + command + P + R during startup.

{% embed url="https://support.apple.com/en-us/HT204063" %}

## Software Dev

```text
# Command Line Tools for Xcode
xcode-select --install
# iTerms 2 font
# https://github.com/powerline/fonts
# VSCode font
# https://github.com/tonsky/FiraCode

# VS Code
# download Settings Sync extension
# shift + alt + d to download settings
```

### Homebrew

```text
brew search xxx     # 模糊搜索某个软件
brew install xxx    # 安装某个软件
brew upgrade xxx    # 升级某个软件
brew uninstall xxx  # 卸载某个软件
brew list xxx       # 列出某个软件的所有相关文件

# CLI Apps
mackup
hugo
```

### Homebrew cask \(for GUI apps\)

```text
# install homebrew cask
brew tap homebrew/cask    # caskroom主仓库
brew tap homebrew/cask-fonts   # caskroom字体仓库

# how to use cask
brew cask search xxx     # 模糊搜索某个软件
brew cask install xxx    # 安装某个软件
brew cask uninstall xxx  # 卸载某个软件
brew cask reinstall      # 重新安装某个软件
brew cask cleanup        # 清理缓存
brew cask install font-source-code-pro # 安装字体

# GUI Apps
google-chrome
iina # player
the-unarchiver
iterms2
vscode
free-download-manager

## quicklook plugins
qlcolorcode: 预览源码时加上语法高亮
qlstephen: 预览无后缀的纯文本文件，比如README, HISTORY等
qlmarkdown: 预览渲染后的markdown文件
```

## Other Apps

* Screensaver: [https://github.com/JohnCoates/Aerial](https://github.com/JohnCoates/Aerial)
* PDF process: cpdf
* Video transcoder: [https://handbrake.fr/](https://handbrake.fr/)
* Remove app completely: [AppCleaner](https://freemacsoft.net/appcleaner/)
* homebrew UI: [https://www.cakebrew.com/](https://www.cakebrew.com/)
* screenshot:
  * command + shift + 3 \(fullscreen\)
  * command + shift + 4 \(drag\)
  * command+ shift + 5 \(drag, new UI\)
* screen recording: quicktime \(with or without sound\)
  * show key strokes: [https://github.com/keycastr/keycastr](https://github.com/keycastr/keycastr)

