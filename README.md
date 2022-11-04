# ifishy-term

Power up the plain o'boring terminal in MacOS with iTerm and fish shell.

## Preview 👀

![complete setup of iTerm2+fish shell](/assests/images/iterm-preview.png)

## Tools 🔨

We are going to use these powerful tools to make our terminal on steroid!

- [iTerm2](https://iterm2.com/) - a new terminal to replace the boring default terminal.  
- [Homebrew](https://brew.sh/) - package manager to install missing stuffs for our shell.  
- [fish shell](https://fishshell.com/) - a smart and user-friendly command line shell.  
- [Fisher](https://github.com/jorgebucaran/fisher) - power up fish with plugins with this plugin manager.  
- [Powerline fonts](https://github.com/powerline/fonts) - [THEMES](#themes)!! But before that we need some fonts.  

## Installation 💻

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
# Apple Silicon
which brew
/opt/homebrew/bin/brew
```

```bash
# macOS Intel
which brew
/usr/local/bin/brew
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

![Font Book app showing Powerline fonts installed](/assests/images/font-book.png)

## Configurations 📝

🎉 Congratulations! We have done all the required installations, now it's time for some configurations.  
> These are just some recommendations, feel free to mix and match to your own needs.

### config.fish

fish shell configurations is located in `~/.config/fish/config.fish`  
This is equivalent to `~/.zprofile` or `~/.bash_profile` but it's for fish.

> If the file does not exist  
> `touch ~/.config/fish/config.fish`

```bash
# add the lines below in the config.fish file
source ~/.fish_aliases
source ~/.fish_variables
```

Here we will source these two files everytime when fish shell logins.  
By doing so, now we can have access to the configurations directly on the user folder level.  
These are the recommended contents for both files.

```bash
# ~/.fish_aliases
# command aliases to be used daily
# directories
alias l "ls -alF"
alias .. "cd .."
# git
alias gst "git status"
alias gco "git checkout"
alias gcl "git clone"
alias gc "git commit"
alias gps "git push"
alias gpl "git pull"
alias gb "git branch"
alias gl "git log"
alias gad "git add"
alias grm "git rm"
alias grst-h "git reset --hard HEAD"
alias gprune "git branch -vv | grep ': gone]' | grep -v '\*' | awk '{ print $1; }' | xargs git branch -D"
# others
alias v "vim"
alias npml "npm list --depth=0"
alias gr grunt
alias ns "npm start"
alias py3 python3
```

```bash
# ~/.fish_variables
# set the shell variables in a fish way
# https://fishshell.com/docs/current/cmds/set.html
# node version using `n` package
set -x N_PREFIX $HOME/.local
set -x PATH $N_PREFIX/bin:$PATH
# default JAVA_HOME with specific Java version
set -x JAVA_HOME (/usr/libexec/java_home -v 1.8.0)
# Docker default platform arch for Apple Silicon
set -x DOCKER_DEFAULT_PLATFORM linux/amd64
```

> **NOTE:** remember to run `source ~/.config/fish/config.fish` after changing these configurations.

### Themes

There are lots of themes available for fish shell, check out this [list](https://github.com/oh-my-fish/oh-my-fish/blob/master/docs/Themes.md) to choose one most suitable for yourself.  
We'll choose [bobthefish](https://github.com/oh-my-fish/theme-bobthefish) in this case for its clean sleek git-aware feature.

![bobthefish in action](https://cloud.githubusercontent.com/assets/53660/18028510/f16f6b2c-6c35-11e6-8eb9-9f23ea3cce2e.gif)

First we need to install [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish), _another_ package manager but this time for fish themes.

```bash
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish
```

Run this to install the theme.

```bash
omf install bobthefish
```

There might be some weird symbols appearing on the terminal, don't worry we will fix it [here](#weird-symbols-after-using-themes).

### fish_config

Run the command below:

```bash
fish_config
```

It will open a new browser page, here we can

- set the color scheme and prompt
- view all functions and variables
- view shell history
- etc.

![fish_config](/assests/images/fish_config.png)

> Remember to `^C` to end the config session when you're done.

## Troubleshooting 🛠

### `brew` not found

Most commonly happened after the session has been restarted, this is due to the `brew` executable path is not sourced correctly when the shell logins.
Run this command to patch it:

```bash
eval "$(/opt/homebrew/bin/brew shellenv)" >> ~/.zprofile
```

> If prefer using `.bash_profile`
>
> ```bash
> eval "$(/opt/homebrew/bin/brew shellenv)" >> ~/.bash_profile
> ```

Then restart the terminal session and try again.

### Weird symbols after using themes

![Weird symbols](/assests/images/incorrect-font.png)

When you set a new theme, you might see some weird symbols like this if your font is not set correctly.  
Here's how we fix it, in your **iTerm2** > **Preferences** > **Profiles** > **Text** > **Font**, select any [Powerline font](#powerline-fonts-optional) that we have installed previously.

![Setting font to powerline](/assests/images/choose-powerline.png)

## Credits 💰

Original tutorial: [fish-shell-setup-osx](https://github.com/ellerbrock/fish-shell-setup-osx) by [ellerbrock](https://github.com/ellerbrock)
