# Manjaro linux setup 

## Enable AUR 

```bash
sudo sed --in-place "s/#EnableAUR/EnableAUR/" "/etc/pamac.conf"
sudo sed --in-place "s/#CheckAURUpdates/CheckAURUpdates/" "/etc/pamac.conf"
```

## Install Chrome

```bash
pamac install google-chrome
```

## Cleanup 

This is an optional step to remove unused software. Open "Add/Remove Software" from 
the start menu and select unneeded software to be removed. Don't forget to apply 
the changes. 

## Useful dependencies

Just install the following:
```bash
pamac install npm nodejs
```


## Editor setup 

Install [neovim](https://github.com/neovim/neovim) and create an alias
```bash
pamac install neovim-git
sudo ln -s /usr/bin/nvim /usr/bin/vim
```

Setup vim plug and configure neovim
```bash
mkdir .config/nvim/autoload
curl -fLo ~/.config/nvim/autoload/plug.vim     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
curl -fLo ~/.config/nvim/coc-settings.json https://gist.githubusercontent.com/dmytro-vynokurov/08f37354723fd5728df94cabfc0fec11/raw/f1d31657f91120b525d2c865811bfa7aa0ed04e4/coc-settings.json
wget -O ~/.config/nvim/init.vim https://gist.githubusercontent.com/dmytro-vynokurov/1ce203ef7c2fde1ed7f65cd5bcbe3688/raw/4265f1753ba4ec116b9f2782dc4bc5326e7a778a/init.vim
```

Then open nvim and run `:PlugInstall` or do the following:
```bash
vim +PlugInstall
```

## Install and configure zsh

Install oh my zsh by following the manual from [here](https://github.com/ohmyzsh/ohmyzsh)
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

In order to make it look nicer I need [my own theme](https://gist.github.com/dmytro-vynokurov/1ca5ea2ecb57351c135eec2ad31a1bcf) 
to be copied to ~/.oh-my-zsh/custom/themes/dvynokurov.zsh-theme and 
fish-style zsh [plugin](https://github.com/zsh-users/zsh-autosuggestions) to be installed. 

```bash
curl -o ~/.oh-my-zsh/custom/themes/dvynokurov.zsh-theme https://gist.githubusercontent.com/dmytro-vynokurov/1ca5ea2ecb57351c135eec2ad31a1bcf/raw/4f50a5ba6cfd3bd322e0c85fd0e4049606a7acab/dvynokurov.zsh-theme

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

After this is done `.zshrc` file has to be patched. Easiest way to do it would be to replace it with 
a version that can be found 
[here](https://gist.github.com/dmytro-vynokurov/7558fd0ec0535fd5876645d0ab86c2e2). This file already 
includes all the small configs for history, fix for sdkman and, potentially, some other useful stuff.

```bash
curl -o ~/.zshrc https://gist.githubusercontent.com/dmytro-vynokurov/7558fd0ec0535fd5876645d0ab86c2e2/raw/6f93cf9706a71e350687ca4b3d63face1729794a/.zshrc
```

Now it should be possible to switch to zsh:
```bash
zsh
```

## Configure git 

git expects user data to be set. Also, default editor has to be changed to vim (nvim)
```bash
git config --global user.email "dmytro.vynokurov@gmail.com"
git config --global user.name "Dmytro Vynokurov"
git config --global core.editor vim

## Sdkman and java 

This command should be used to install sdkman: 
```bash
curl -s "https://get.sdkman.io" | bash
```

If, for some reason, it doesn't work, try checking their documentation. 
Also, note that `.zshrc` file had to be patched in the previoius step, as otherwise `sdk` may not
work as expected. 

After sdkman gets installed `sdk` command will become available. One should use it to list 
available java versions and then to install the available openjdk version: 

```bash
sdk list java 
```

Command above should output multiple versions and among them should be something like 11.0.2-open
```bash
sudo sdk install <version> # here <version>=11.0.2.open
```

## Python stuff

Python 3 should already be in place, but pip and virtualenv still have to be installed
```bash
pamac install python-pip
pip install virtualenv
```

## Docker

```bash
sudo pacman -S docker
sudo systemctl enable docker.service
sudo pamac install docker-compose
```

