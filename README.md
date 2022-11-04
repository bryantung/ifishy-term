# ifishy-term

Power up the plain o'boring terminal in MacOS with iTerm and fish shell.

## Preview

<img width="1024" alt="Screenshot 2022-11-03 at 22 53 42" src="https://user-images.githubusercontent.com/2121605/199754961-7ea2dec2-16f4-4b98-a792-5c3dde71015b.png">

## Tools

We are going to use these powerful tools to make our terminal on steroid!

- [iTerm2](https://iterm2.com/) - a new terminal to replace the boring default terminal.  
- [Homebrew](https://brew.sh/) - package manager to install missing stuffs for our shell.  
- [fish shell](https://fishshell.com/) - a smart and user-friendly command line shell.  
- [Fisher](https://github.com/jorgebucaran/fisher) - power up fish with plugins with this plugin manager.  
- [Powerline fonts](https://github.com/powerline/fonts) - THEMES!! But before that we need some fonts.  

## Installation

Now we will go through step-by-step the required installations.
> Remember, we're only installing the tools needed, configurations will come after this

### iTerm2 (Optional)

We strongly [recommend](https://iterm2.com/features.html) installing iTerm2 to replace the default terminal.  
Click [here](https://iterm2.com/downloads/stable/latest) to download and install.  
Following steps we will be using iTerm2 as our default terminal.  

### Homebrew

Install Homebrew by using the command below and follow the instructions:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```  

Quick check after the installation is done.

```bash
which brew
# macOS Intel
/usr/local/bin/brew
# Apple Silicon
/opt/homebrew/bin/brew
```

> If everything goes well, using `brew` **shouldn't** require any `sudo brew`, refer to this [documentation](https://docs.brew.sh/Installation) if something went wrong.

### fish shell

Next stop is the mighty fish shell, run the commands below to install and setup as default shell:

```bash
# install the latest fish
brew install fish
# add fish path to /etc/shells
echo $(which fish) | sudo tee -a /etc/shells
# make fish as your default shell
chsh -s $(which fish)
```

Nice! Now we have iTerm2 powered up with fish!

### Fisher

Once we have fish installed, it's time for some [plugins](https://github.com/jorgebucaran/awsm.fish) to make our life easier.  
Install Fisher, the plugin manager for fish:

```bash
curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
```

After it's done, let's get started with some useful plugins.

```bash
# update all plugins
fisher update
# autocompletion for most used directories
fisher install jethrokuan/z
# for better bash compatibility
fisher install edc/bass
# printf with colours
fisher install Markcial/cprintf
# docker completion
fisher install barnybug/docker-fish-completion
# fuzzing finder
brew install fzf; fisher install jethrokuan/fzf
# colourizer for terminal apps
brew install grc; fisher install oh-my-fish/plugin-grc
```

> If `brew install <plugin>` is not working, make sure that `which brew` is printing the [correct executable path](#homebrew), more info in the [troubleshooting](#brew-not-found) section

### Powerline fonts (Optional)

At last, fonts installation is optional, feel free to skip this step if not planning to setup any themes (but WHY NOT?!).

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/powerline/fonts/master/install.sh)"
```

Open the **Font Book** app to verify that the fonts are installed.
<img width="1024" alt="Screenshot 2022-11-04 at 02 35 26" src="https://user-images.githubusercontent.com/2121605/199806311-24c5456c-2f3f-43b5-8af4-70f21576be8b.png">

## Configurations

## Troubleshooting

### `brew` not found

Most commonly happened after the session has been restarted, this is due to the `brew` executable path is not sourced correctly when the shell logins.
Run this command to patch it:

```bash
eval "$(/opt/homebrew/bin/brew shellenv)" >> ~/.zprofile
# or if you prefer .bash_profile
eval "$(/opt/homebrew/bin/brew shellenv)" >> ~/.bash_profile
```

Then restart the terminal session and try again.

## Credit

Original tutorial
https://github.com/ellerbrock/fish-shell-setup-osx
