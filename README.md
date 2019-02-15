# pyenv-headaches

I seem to never quite get into the zone when it comes to installing multiple versions of python on a Mac. Don't get me wrong, I'll get something working, but then another project will come along that requires a *slightly* different version of python and things will fall apart. It seems to happen months apart, long after I remembered what it took to get it working the last time. My zsh history looks like this every few months:

```
pyenv versions
pyenv
pyenv help versions
pyenv help install
pyenv install -l
pyenv update
cd ~/.pyenv
git pull
ls
history | grep pyenv
```

I get it. There are a lot of moving parts. But it also warrants a *huge* shoutout to [nvm](https://github.com/creationix/nvm), which consistently works for switching easily between versions of Node.js. I haven't used many version managers, just nvm, rvm, and one or two here and there for other languages, but nvm is hands down my favorite. There is no learning curve, no feeling like I need to orient to some new way of thinking about things that I just don't want to think about. NVM is actively and carefully managed and I just have a lot of respect for the entire initiative there.

The python world has not been that easy for me, so I'm keeping track of what I had to do on each machine to get things squared away.

## 2019-02-14 
#### MacBook Pro (Retina, 13-inch, Late 2013) Mojave 10.14.2
```
brew update
brew upgrade pyenv
pyenv update
pyenv install --list
```
Now I can install the versions of python I need

```
pyenv install 3.6.8
```
Oops, wait. No I can't. I don't have zlib apparently.

```
xcode-select --install
```

Nope, that didn't work. After some digging, this happened: https://github.com/pyenv/pyenv/issues/1219

```
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
pyenv install --list
```

Fingers crossed...

```
pyenv install 3.6.8
pyenv versions
```

:thumbsup:

```
pyenv install 3.7.2
pyenv versions
```

:thumbsup: *for now*