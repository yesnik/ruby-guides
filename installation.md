# Ruby Installation

Official Docs: https://www.ruby-lang.org/en/documentation/installation/

## Ubuntu

### Rbenv

This [instruction](https://gorails.com/setup/ubuntu/20.04) will help to install Ruby, Rails on Ubuntu 20:

```bash
sudo apt install curl
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update
sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn
```

Next we're going to be installing Ruby using a version manager called Rbenv.

Installing with rbenv is a simple two step process. First you install rbenv, and then ruby-build:

```bash
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL
```
To install Ruby and set the default version, we'll run the following commands:

```bash
# Show available versions
rbenv install -l

rbenv install 3.1.1
rbenv global 3.1.1
```

Confirm the default Ruby version matches the version you just installed.

```bash
ruby -v
```

The last step is to install Bundler

```bash
gem install bundler
```

`rbenv` users need to run `rbenv rehash` after installing bundler.

Install Rails:

```bash
gem install rails
```

### Apt

```
sudo apt install ruby-full
```

### Source

Get the link to [stable release](https://www.ruby-lang.org/en/downloads/)

```
wget https://cache.ruby-lang.org/pub/ruby/3.1/ruby-3.1.1.tar.gz
tar -xzvf ruby-3.1.1.tar.gz
cd ruby-3.1.1/
./configure
make
sudo make install

# Close terminal, open it again
ruby -v
```

**Note** At February 2022 command `make` finished with error:

```
./tool/file2lastrev.rb:6:in `require': cannot load such file -- optparse (LoadError)
	from ./tool/file2lastrev.rb:6:in `<main>'
```

### Snap

Snap is a package manager developed by Canonical. It is available out-of-the-box on Ubuntu.

```bash
sudo snap install ruby --classic

ruby -v
# ruby 3.0.2p107 (2021-07-07 revision 0db68f0233) [x86_64-linux]

gem -v
# 3.2.22
```

**Note** We got errors for command: `gem install rails`:
- At Feb 2022: `gem install –verbose nio4r`
- At Sep 2021: `gem install eventmachine`. See issue: https://github.com/eventmachine/eventmachine/issues/881
