### brew
Go to brew.sh

### tools
```sh
brew install xz
```

```sh
xcode-select --install
```

To show hidden files:
```
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

### git
```
brew install git
```

### zsh
```sh
brew install zsh
```

### oh-my-zsh
```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### oh-my-zsh themes
```sh
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```
Then edit your `~/.zshrc` and set `ZSH_THEME="powerlevel10k/powerlevel10k"`. Once you do so, when you start a new terminal session, the Powerlevel10 configure wizard will be launched to set your prompt, beware, there are many many options!

### oh-my-zsh plugins
```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Then edit your `~/.zshrc` and set `plugins=(git zsh-completions zsh-syntax-highlighting zsh-autosuggestions)`

### docker
Go to https://docs.docker.com/docker-for-mac/install/

### kubectl
```sh
brew install kubectl
```

### helm
```sh
brew install helm
```

### terraform
```
brew install terraform
```

### aws-cli
```sh
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### aws-vault
```sh
brew install --cask aws-vault
```

### python 3
```bash
brew install pyenv
pyenv install 3.9.4
pyenv global 3.9.4
pyenv version
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

After that command, our dotfile (.zshrc for zsh or .bash_profile for Bash) should include these lines:

```bash
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```
