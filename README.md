# :zap:Znap!
**Znap** is the light-weight plugin manager for Zsh that's easy to grok.

Consisting of just ~16 kilobytes of source code (9 KB when zipped), Znap does everything you need
from a plugin manager, but without any bloat.

Znap is in fact so simple, that if you ever have a question about it, the fastest way to get an
answer is probably to [read the code](functions). Because good software shouldn't be hard to
understand.

Oh, and of course, Znap needs exactly zero configuration. Just `git clone` the repo into the
right place, `source` the plugin file, and you're good to go.

* [Installation](#installation)
* [Getting Started](#getting-started)
* [Author](#author)
* [License](#license)

# Installation
1. `cd` into the dir where you already keep your plugins or where you want to keep your plugins. If
   you don't have such a dir yet, you'll have to first make one. I personally use the following:
   ```zsh
   mkdir ~/.zsh
   cd ~/.zsh
   ```
1. `git clone` this repo. You'll end up with dir called `zsh-snap` inside your plugins dir. This
   example uses `https` because you don't need a GitHub account for it, but if you do have one,
   you can use `ssh` instead:
   ```zsh
   git clone https://github.com/marlonrichert/zsh-snap.git
   ```
   * _(optional)_ If you want to install Znap elsewhere, you'll need to tell it where to find your
     plugins dir (which you can also change on the fly):
     ```zsh
     zstyle ':znap:*' plugins-dir ~/.zsh
     ```
1. Add this line _near the top_ of your `~/.zshrc` file —or at least early enough that Znap is
   `source`d before any plugins that you want to manage:
   ```zsh
   source ~/.zsh/zsh-snap/znap.zsh
   ```
1. **Restart your shell.**

## Getting Started
Here's a list of things that Znap can help you do more easily and more efficiently.

### Clone Repos in Parallel
If you haven't done so yet, `znap clone` the plugins you want to use. If you pass it multiple Git
remote URLs, it will clone them all _in parallel,_ which can be much faster than cloning them one
by one:
```zsh
znap clone \
   git@github.com:ekalinin/github-markdown-toc.git \
   git@github.com:marlonrichert/zsh-autocomplete.git \
   git@github.com:marlonrichert/zsh-hist.git \
   git@github.com:marlonrichert/zsh-snap.git \
   git@github.com:MichaelAquilina/zsh-autoswitch-virtualenv.git \
   git@github.com:sindresorhus/pure.git \
   git@github.com:trapd00r/LS_COLORS.git \
   git@github.com:zsh-users/zsh-syntax-highlighting.git
```

### Quickly Re-install Your Plugins
Znap saves the URL of each remote you clone. You can quickly re-clone all remotes by running
`znap clone`, without arguments.

### Update Repos in Parallel
To update all of your repos _in parallel,_ just run `znap pull`.

### Instant Prompt, Simple Config
Here's how you can use Znap to optimize your `~/.zshrc` file:
```zsh
#!/bin/zsh

# Source Znap at the start of your .zshrc file.
source ~/.zsh/zsh-snap/znap.zsh

# Use Znap to make your prompt appear *instantly.*
# You can start typing right away!
znap prompt agnoster  # Just an example. Works with any (normal) theme.

# For OhMyZsh themes, use this syntax:
# znap prompt ohmyzsh robbyrussell

# Then, while you are typing...

# Use Znap to source your plugins:

znap source zsh-autocomplete

bindkey '^[Q' push-line-or-edit
znap source zsh-hist

export ZSH_HIGHLIGHT_HIGHLIGHTERS=( main brackets )
znap source zsh-syntax-highlighting

znap source zsh-autoswitch-virtualenv

# Use Znap to add plugins to your $path:
typeset -gU PATH path=(
  $(znap path github-markdown-toc)
  $path
  .
)

# Use Znap to cache and compile the output of slow `eval` commands:

# This runs inside the LS_COLORS repo.
znap eval LS_COLORS 'gdircolors -b LS_COLORS'
zstyle ":completion:*" list-colors "${(s.:.)LS_COLORS}"

# These don't have a repo, but the first arg will be used to name the cache file.
znap eval brew-shellenv 'brew shellenv'
znap eval pyenv-init 'pyenv init -'
znap eval pipenv-completion 'pipenv --completion'
```

Again, always **restart your terminal** for changes to take effect.

### Asynchronous Compilation
While you are using your shell, Znap will compile your scripts and functions in the background,whenever the Zsh Line Editor is idle. This way, you Zsh will start up even faster next time!

Should you not want this feature, you can _disable_ it with `zstyle ':znap:*' auto-compile no`.
You can then run it manually at any time with `znap compile`.

## Author
© 2020 [Marlon Richert](https://github.com/marlonrichert)

## License
This project is licensed under the MIT License. See the
[LICENSE](LICENSE) file for details.
