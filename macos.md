# brew
Go to brew.sh
https://gist.github.com/kevin-smets/8568070
# zsh
```
brew install zsh
```

# oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

# zsh plugins
```
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```


```
brew install romkatv/powerlevel10k/powerlevel10k
echo "source $(brew --prefix)/opt/powerlevel10k/powerlevel10k.zsh-theme" >>~/.zshrc
```

Then edit your ~/.zshrc and set ZSH_THEME="powerlevel10k/powerlevel10k". Once you do so, when you start a new terminal session, the Powerlevel10 configure wizard will be launched to set your prompt, beware, there are many many options!

# git
```
brew install git
```

# docker

# kubectl

# helm


