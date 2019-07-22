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

Now to set it up in a project that has a Pipfile with a requires block for `python_version` 3.6, I need to make sure that in the project directory I run:

```
pyenv local 3.6.8
pipenv install --dev
```

Oh yeah, I haven't installed `pipenv` in 3.6.8 yet.

```
pip install pipenv
```

Okay, that installed, but I'm still not able to load the environment correctly. Do some more digging... Found [this article](https://click.palletsprojects.com/en/7.x/python3/) that says if you are using Click with Python3 need to set LC_ALL and LANG exports, so added this to dotfiles:

```
export LC_ALL=en_US.utf-8
export LANG=en_US.utf-8
```

:thumbsup: *for now*

## 2019-07-22
#### On a MacBook Pro (15 inch, 2017) Mojave 10.14.5

<details><summary>I was trying to install from a homebrew tap and ran into issues where homebrew complained about my pyenv config files:</summary>
	<p>

```
==> Tapping aws/tap
Cloning into '/usr/local/Homebrew/Library/Taps/aws/homebrew-tap'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 4 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
Tapped 1 formula (32 files, 42.1KB).
~
❯ brew install aws-sam-cli                                       3.6.8 10.12.0
==> Installing aws-sam-cli from aws/tap
==> Installing dependencies for aws/tap/aws-sam-cli: openssl, sqlite and python
==> Installing aws/tap/aws-sam-cli dependency: openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2s.mojave.bottl
==> Downloading from https://akamai.bintray.com/c4/c4a762d719c2be74ac686f1aafabb
######################################################################## 100.0%
==> Pouring openssl-1.0.2s.mojave.bottle.tar.gz
==> Caveats
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

openssl is keg-only, which means it was not symlinked into /usr/local,
because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.

If you need to have openssl first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.zshrc

For compilers to find openssl you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl/include"

For pkg-config to find openssl you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"

==> Summary
🍺  /usr/local/Cellar/openssl/1.0.2s: 1,795 files, 12.0MB
==> Installing aws/tap/aws-sam-cli dependency: sqlite
==> Downloading https://homebrew.bintray.com/bottles/sqlite-3.29.0.mojave.bottle
==> Downloading from https://akamai.bintray.com/5f/5f2f8f36a8d13733b0374ac39bdcd
######################################################################## 100.0%
==> Pouring sqlite-3.29.0.mojave.bottle.tar.gz
==> Caveats
sqlite is keg-only, which means it was not symlinked into /usr/local,
because macOS provides an older sqlite3.

If you need to have sqlite first in your PATH run:
  echo 'export PATH="/usr/local/opt/sqlite/bin:$PATH"' >> ~/.zshrc

For compilers to find sqlite you may need to set:
  export LDFLAGS="-L/usr/local/opt/sqlite/lib"
  export CPPFLAGS="-I/usr/local/opt/sqlite/include"

For pkg-config to find sqlite you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/sqlite/lib/pkgconfig"

==> Summary
🍺  /usr/local/Cellar/sqlite/3.29.0: 11 files, 3.9MB
==> Installing aws/tap/aws-sam-cli dependency: python
==> Downloading https://homebrew.bintray.com/bottles/python-3.7.4.mojave.bottle.
==> Downloading from https://akamai.bintray.com/81/81fc6e5914a16387bd09387ce08e9
######################################################################## 100.0%
==> Pouring python-3.7.4.mojave.bottle.tar.gz
Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/local
Directory not empty @ dir_s_rmdir - /usr/local/opt/python
Error: Directory not empty @ dir_s_rmdir - /usr/local/opt/python
~
❯ brew doctor                                                  ⏎ 3.6.8 10.12.0
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: "config" scripts exist outside your system or Homebrew directories.
`./configure` scripts often look for *-config scripts to determine if
software packages are installed, and which additional flags to use when
compiling and linking.

Having additional scripts in your path can confuse software installed via
Homebrew if the config script overrides a system or Homebrew-provided
script of the same name. We found the following "config" scripts:
  /Users/brock/.pyenv/shims/python3.6m-config
  /Users/brock/.pyenv/shims/python3.7-config
  /Users/brock/.pyenv/shims/python3.7m-config
  /Users/brock/.pyenv/shims/python-config
  /Users/brock/.pyenv/shims/python3-config
  /Users/brock/.pyenv/shims/python3.6-config
  /usr/local/opt/pkg-config
  /Library/Frameworks/GDAL.framework/Programs/gdal-config

Warning: You have unlinked kegs in your Cellar.
Leaving kegs unlinked can lead to build-trouble and cause brews that depend on
those kegs to fail to run properly once built. Run `brew link` on these:
  python
~
```
</p>
</details>

This led me to a solution that I think more people need to be aware of:

* Only `brew install` when `pyenv global` is set to `system`
* Run `pyenv global` and if it isn't set to `system`, just run `pyenv global system`.
* You can switch it back to whatever global you had when you're done and things continue to work

This immediately solved my problem. I found that on [this article](https://amaral.northwestern.edu/blog/troubleshooting-pyenv). 


