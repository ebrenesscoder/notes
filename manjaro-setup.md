# Manjaro Setup

teps to configure Manjaro

## Tools
install neo-vim
```
sudo pacman -S neovim
```
## ZSH
```
sudo -S zsh zsh-completions
```

Install Oh my Zsh, a tool to configure zsh shell easily
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install plugins
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Configure zsh
```
sudo vim ~/.zshrc
```

```
plugins=(
    git
    zsh-autosuggestions
    zsh-syntax-highlighting
)
```

Change theme
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`

```
source ~/.zshrc
```
Make zsh the default shell

```
chsh -s /bin/zsh
```

Restart computer

## Git
```
git config --global user.email "ebreness.projects@gmail.com"

git config --global user.name "Erick Brenes Solano"
```

## AWS
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

aws --version

aws configure --profile <profile-name>
```

Install aws-vault

```
sudo pacman -S aws-vault
```

