```
sudo dpkg-reconfigure dash
no
```

### bashrc
```
#!/usr/bin/bash
# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if [ -f /etc/bash_completion ] && ! shopt -oq posix; then
    . /etc/bash_completion
fi

# Set the PS1 prompt (with colors).
# Based on http://www-128.ibm.com/developerworks/linux/library/l-tip-prompt/
# And http://networking.ringofsaturn.com/Unix/Bash-prompts.php .
PS1="\[\e[36;1m\]\h:\[\e[32;1m\]\w$ \[\e[0m\]"

# Set the default editor to vim.
export EDITOR=vim

# Avoid succesive duplicates in the bash command history.
export HISTCONTROL=ignoredups

# Append commands to the bash command history file (~/.bash_history)
# instead of overwriting it.
shopt -s histappend

# Append commands to the history every time a prompt is shown,
# instead of after closing the session.
PROMPT_COMMAND='history -a'

# Add bash aliases.
if [ -f ~/.bash_aliases ]; then
    source ~/.bash_aliases
fi
```

### profile
```
# Add some more custom software to PATH.
PATH=$PATH:~/usr/bin
export PATH

# Make sure pkg-config can find self-compiled software
# and libraries (installed to ~/usr)
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:~/usr/lib/pkgconfig
export PKG_CONFIG_PATH

# Add custom compiled libraries to library search path.
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/usr/lib
export LD_LIBRARY_PATH

# Add custom compiled libraries to library run path.
LD_RUN_PATH=$LD_RUN_PATH:~/usr/lib
export LD_RUN_PATH

# Add custom Python modules to the Python path.
PYTHONPATH=$PYTHONPATH:~/slippenspythonlib
PYTHONPATH=$PYTHONPATH:~/usr/lib/python2.5/site-packages
export PYTHONPATH

# Java;s CLASSPATH customization
CLASSPATH=$CLASSPATH:~/foo/bar.jar
export CLASSPATH
```

### bash_aliases
```
# Make some possibly destructive commands more interactive.
alias rm='rm -i'
alias mv='mv -i'
alias cp='cp -i'

# Add some easy shortcuts for formatted directory listings and add a touch of color.
alias ll='ls -lF --color=auto'
alias la='ls -alF --color=auto'
alias ls='ls -F'

# Make grep more user friendly by highlighting matches
# and exclude grepping through .svn folders.
alias grep='grep --color=auto --exclude-dir=\.svn'

# Shortcut for using the Kdiff3 tool for svn diffs.
alias svnkdiff3='svn diff --diff-cmd kdiff3'
```

### screenrc
```
# Set the default window name to empty string instead of the arbitrary "bash"
shelltitle ''

# Set the window caption.
# I use caption instead of hardstatus, so it is available per split window too
# (hardstatus is only per complete screen).
caption always "%{= KW}%-Lw%{= wb}%n %t %{= KW}%+Lw %-=| ${USER}@%H | %M%d %c%{-}"
# Some decryption hints:
# %{= KW}     background light black (aka dark gray) with foreground light white
# %{= wb}     background dark white (ake light gray) with foreground dark blue
# %-Lw        all windows before the current window.
# %n%f %t     current window number, flags and title.
# %+Lw        all windows after the current window.
# %-=         pad remaining spaces.
# %H          hostname.
# %M%d %s     month and day (MmmDD) and current time (HH:MM).
```
