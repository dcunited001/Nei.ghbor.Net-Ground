Nei.ghbor.Net Ground
====================

Ground is the fundamental network for the Nei.ghbor.Net project. A Ground Node consists of two parts:

* A CJDNS daemon - provides encrypted routing over local mesh networks and tunneling over the existing IP infrastructure.
* A CCNx daemon - provides content-based routing and caching for efficient distrobution of data.

To install this protocol stack, you would be well served to be running Linux or OSX

for CJDNS installation instructions, see that projects Readme

for CCNx, see http://blog.rungeek.com/post/1711470902/project-ccnx-how-to (ubuntu)

We are working on brew scripts for MacOS and apt repos for ubuntu to ease this process.

Once your Ground stack is up and running, you'll want to find some Neighbors to connect with... see Nei.ghbor.Net-Keystone


Install
=======

Nei.ghbor.Net Ground has a few dependencies to install:

#### On OSX - Use Brew

Use this [Gist](https://gist.github.com/dcunited001/5626815) to Install CCNx & CJDNS

- Please report any problems you have with this gist.
- Or compile CCNx & CJDNS from source
- TODO: update gist to include the correct branch of CJDNS

#### On Linux - Build From Source

1. Install CJDNS Dependencies
  - `sudo apt-get install cmake git build-essential`
  - [NACL - Networking and Cryptography Library](http://nacl.cr.yp.to/install.html) is required - `sudo apt-get libnacl-dev`?
1. Install CCNx Dependencies (install the following packages with apt-get)
  - ant1.8
  - autoconf
  - libssl-dev
  - libexpat-dev
  - libpcap-dev
  - libecryptfs0
  - libxml2-utils
  - automake
  - gawk
  - gcc
  - g++
  - git-core
  - pkg-config
  - libpcre3-dev
  - openjdk-6-jre-lib
1. Download CCNx Source
  - Download latest stable here - [CCNx 0.7.2 Tarball](http://www.ccnx.org/releases/ccnx-0.7.2.tar.gz) - 5/20/13 - [SHA1](http://www.ccnx.org/releases/ccnx-0.7.2.tar.gz.SHA1)
  - Or Download 0.7.2 tag with `git clone git@github.com:ProjectCCNx/ccnx --branch ccnx-0.7.2 ccnx-0.7.2` (then checkout a new local branch, since you will be in a detached HEAD)
  - Or Download latest updates with `git clone git@github.com:ProjectCCNx/ccnx.git` (not recommended)
1. Build CCNx
  - `cd ccnx`
  - `make`
  - `make install`
  - Don't forget to update $PATH in your shell profile with the CCNx "bin" directory.
1. Download CJDNS Source
  - Download latest updates with `git clone git@github.com:cjdelisle/cjdns`
  - OSX Users must download the [named-pipes](https://github.com/cjdelisle/cjdns/tree/named-pipes) branch with `git clone git@github.com:cjdelisle/cjdns --branch named-pipes`
1. Build CJDNS
  - `cd cjdns`
  - `./do`
  - Don't forget to update $PATH in your shell profile with the CJDNS "bin" directory.

#### On Debian Wheezy (Raspbian)

- Build CCNx/CJDNS from source
- While you wait, ask a friend about cross-compilers.
- TODO: build instructions

Configure CJDNS
===============

#### Setup CJDNS Routes Conf

- Setup CJD Routes File with `cjdroute --genconf > cjdroute.conf`
- Refer to cjdroute.conf.example (cjdroute.conf is ignored in the project)

#### Ensure [TUN/TAP](https://github.com/cjdelisle/cjdns/tree/named-pipes#0-make-sure-youve-got-the-stuff) virtual network devices are available

Check for TUN device:

```
cat /dev/net/tun # If it says, "cat: /dev/net/tun: File descriptor in bad state", move on.
```

Otherwise, if /dev/net/tun doesn't exist, make one:

```
sudo mkdir /dev/net ; sudo mknod /dev/net/tun c 10 200 && sudo chmod 0666 /dev/net/tun
```

If you run into any problems, refer to [this section](https://github.com/cjdelisle/cjdns/tree/named-pipes#0-make-sure-youve-got-the-stuff) in the CJDNS repo.

Configure CCNX
==============

#### Initialize CCNx Repo

TODO: Initializing CCNx Repo

Nei.ghbor.Net Ground - Services
===============================

#### CCNx

- `ccnd` - runs CCNx in the foreground
- `ccndstart` - starts CCNx in the background

#### CJDNS

- `sudo cjdroute` - run without options for help
- `sudo cjdroute < cjdroute.conf` - start cjd, specifying config file
- `sudo cjdroute < config/cjdroute.conf > log/cjdroute.log` - with log (won't work with foreman, i don't think)

To start `cjdroute` as a non-root user, read [this](https://github.com/cjdelisle/cjdns/tree/named-pipes#start-cjdroute-as-non-root-user)

Starting Services
=================

Different ways to start services:

- Install Ruby/Bundler/Foreman & `foreman start`
- Configure rc.d (Raspbian Wheezy)
- Configure systemd (Raspbian Arch)

#### With Foreman

Foreman is by far the easiest route to take, but may not be ideal for production.

All Foreman processes must run in the foreground.  Fore example, this means you must use `ccnd` instead of `ccndstart`.

1. `ruby -v; gem -v`
1. `gem install bundler`
1. `bundle install`
1. `cp Procfile.example Procfile`
1. `foreman start`

#### With rc.d

TODO: configuring CCNx/CJDNS with rc.d

#### With systemd

TODO: configuring CCNx/CJDNS with systemd
