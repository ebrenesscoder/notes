# Windows Setup
- Display Seetings - Scale 100%
- Downloas Google Chrome
- Install terminal
- - under defaults:
- 
```json
"colorScheme": "Nord"
```

```json
Nord Theme Colors:
{
    "name": "Nord",
    "foreground": "#D8DEE9",
    "background": "#2E3440",
    "black": "#3B4252",
    "red": "#BF616A",
    "green": "#A3BE8C",
    "yellow": "#EBCB8B",
    "blue": "#81A1C1",
    "purple": "#B48EAD",
    "cyan": "#88C0D0",
    "white": "#E5E9F0",
    "brightBlack": "#4C566A",
    "brightRed": "#BF616A",
    "brightGreen": "#A3BE8C",
    "brightYellow": "#EBCB8B",
    "brightBlue": "#81A1C1",
    "brightPurple": "#B48EAD",
    "brightCyan": "#88C0D0",
    "brightWhite": "#E5E9F0"
}

```

- WSL Install

- - as administrator run:
```sh
wsl --install
```

restart

- Set ubuntu as default in the terminal

- install zsh
```sh
sudo apt-get update
```

```sh
sudo apt-get install zsh
```

```sh
zsh --version
```

```sh
chsh -s `which zsh`
```

restart terminal
select 2. Populate your `~/.zshrc` with the configuration recommended

- install git

```sh
sudo apt install git
```

- install oh-my-zsh

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

```sh
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```


- install plugins
```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
```

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Then edit your `~/.zshrc` and set `plugins=(git zsh-completions zsh-syntax-highlighting zsh-autosuggestions)`
