# Ubuntu zsh

### 查看当前Shell

`echo $SHELL`

### 安装zsh {id="zsh_1"}

sudo apt install zsh

### 切换zsh

`chsh -s /bin/zsh`

`sudo reboot`

### 安装 Oh my zsh

`sh -c "$(wget -O-` [`https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh`](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)`)”`

国内：

`sh -c "$(wget -O-` [`https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh`](https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)`)"`

### zsh-syntax-highlighting插件

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# 国内源
git clone https://gitee.com/ziyaoxie/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### zsh-autosuggestions插件

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
#国内源
git clone https://gitee.com/funckzs/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### 启用插件

```bash
# ~/.zshrc
plugins=(
  git zsh-autosuggestions zsh-syntax-highlighting
)
```

`source ~/.zshrc`

