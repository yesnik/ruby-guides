# Ruby Installation

Official Docs: https://www.ruby-lang.org/en/documentation/installation/

## Ubuntu

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

### Snap

Snap is a package manager developed by Canonical. It is available out-of-the-box on Ubuntu.

```bash
sudo snap install ruby --classic

ruby -v
# ruby 3.0.2p107 (2021-07-07 revision 0db68f0233) [x86_64-linux]

gem -v
# 3.2.22
```

**Note** At Sep 2021 we have errors with ruby installed that way: `gem install eventmachine`. See issue: https://github.com/eventmachine/eventmachine/issues/881
