# brew
Go to brew.sh

# git
```
brew install git
```

# zsh
```
brew install zsh
```

# oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

# oh-my-zsh themes
```
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```
Then edit your `~/.zshrc` and set `ZSH_THEME="powerlevel10k/powerlevel10k"`. Once you do so, when you start a new terminal session, the Powerlevel10 configure wizard will be launched to set your prompt, beware, there are many many options!

# oh-my-zsh plugins
```
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Then edit your `~/.zshrc` and set `plugins=(git zsh-completions zsh-syntax-highlighting zsh-autosuggestions)`

# docker

# kubectl

# helm


