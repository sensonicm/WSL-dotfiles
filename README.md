My dot files for `~/.config` directory — nvim, tmux, alacritty etc.

Install:

```bash
1 сначала устанавливаем wsl (power shell или windows terminal)

windows terminal можно скачать в магазине виндовс

wsl --install -d Debian

проверяем, что дистро стоит коммандой

wsl -l -v

командой wsl в терминале или по вкладке в windows terminal открываем дистрибутив Debian
и задаем пользователя

Обновляем репо и ставим софт

sudo apt update

sudo apt install -y git tree zsh gpg pass zip unzip curl wget tmux gcc bsdmainutils htop fzf bat ripgrep build-essential



2  приступаем к настройке Git

в терминале wsl и консоли git-bash:

git config --global user.name "Max Afanasyev"
git config --global user.email "cptx@yandex.ru"
git config --global core.quotepath false

устанавливаем git-csm
https://git-scm.com/


Подключаем SSH и GPG

SSH:
https://docs.github.com/ru/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

создаем папку .ssh и копируем ключи туда, либо создаем новые

sudo mkdir -p .ssh

в терминале wsl нужно установить права на файлы:

sudo chmod 600 .ssh/sshkeyfile
sudo chmod 644 .ssh/sshkeyfile.pub

проверяем соединение по ssh:

ssh -T git@github.com

Полученный ответ должен содержать ваш ник:

Hi sensonicm! You've successfully authenticated, but GitHub does not provide shell access.

GPG:
https://docs.github.com/ru/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys?platform=linux

gpg --import public.gpg
gpg --import secret.gpg

проверяем наличие ключей

gpg --list-secret-keys --keyid-format=long

и выводим ключ командой с keyid из предыдущей

gpg --armor --export 3AA5C34371567BD2

добавляем в git информацию о ключе

git config --global user.signingkey 3AA5C34371567BD2



3 Настраиваем терминал и окружение


Копируем репо с конфигурацией:

git clone git@github.com:sensonicm/WSL-dotfiles.git .config


-- Alacritty

само приложение на офф сайте
https://alacritty.org/

скачиваем шрифт: MesloLG Nerd Font Mono
https://www.nerdfonts.com/font-downloads

качаем репо с темами:
git clone https://github.com/alacritty/alacritty-theme.git

создаем директорию под винду

mkdir -p /mnt/c/Users/senso/AppData/Roaming/alacritty

копируем туда конфиг и темы

cp .config/alacritty/alacritty.toml /mnt/c/Users/senso/AppData/Roaming/alacritty/
mv alacritty-theme/ /mnt/c/Users/senso/AppData/Roaming/alacritty/

Терминал сразу открывается в машине wsl из-за настройки в alacritty.toml:

[terminal]
shell = { program = "wsl", args = [ "-d", "Debian", "--cd", "~" ] }



-- ZSH

устанавливаем OhMyZSH

команда для установки на сайте https://ohmyz.sh/#install

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

подкдючаем конфиги к .zshrc 

echo "source \$HOME/.config/zsh/env.zsh" >> ~/.zshrc
echo "source \$HOME/.config/zsh/aliases.zsh" >> ~/.zshrc

ставим powerlevel 10k

git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"

в конфиге .zshrc меняем тему:

ZSH_THEME="powerlevel10k/powerlevel10k"

и перегружаем zsh:

exec zsh


Добавляем плагины в файл .zshrc

plugins=(git z docker fzf history)

Перезагружаем zsh

exec zsh


Настраиваем сохран истории, добавим к концу файла:

# history setup
HISTFILE=$HOME/.zhistory
SAVEHIST=1000
HISTSIZE=999
setopt share_history
setopt hist_expire_dups_first
setopt hist_ignore_dups
setopt hist_verify







4 устанавливаем дополнительный софт

-- google chrome


-- obsidian
https://obsidian.md/

скачиваем репозиторий с заметками:

git clone https://github.com/sensonicm/knowledge-base.git

добавляем папку как Vault

в настройках Hotkeys - Git: Commit-and-Sync устанавливаем 'Ctrl + P'




5 правим команду bat на тубе 38:17













sudo apt install -y git curl wget tmux gcc ripgrep fzf build-essential

# Alacritty themes
mkdir -p ~/.config/alacritty/themes
git clone https://github.com/alacritty/alacritty-theme ~/.config/alacritty/themes

# zsh settings
echo "source \$HOME/.config/zsh/env.zsh" >> ~/.zshrc
echo "source \$HOME/.config/zsh/aliases.zsh" >> ~/.zshrc

# Install nodejs and pyright LSP server for Python
# https://nodejs.org/en/download/package-manager
# choose linux and nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
export NVM_DIR="$HOME/.nvm"
nvm install 23
node -v
npm -v

npm i -g npm
npm i -g pyright

# Install typescript LSP server
npm install -g typescript-language-server typescript

# Packer for nvim
mkdir -p ~/.local/share/nvim/site/pack/packer/start/

git clone --depth 1 https://github.com/wbthomason/packer.nvim \
    ~/.local/share/nvim/site/pack/packer/start/packer.nvim

# Build telescope for nvim
cd ~/.local/share/nvim/site/pack/packer/start/telescope-fzf-native.nvim
make
```
