---
content_title: "1.2: Before You Begin"
link_text: "1.2: Before You Begin"
---

## Step 1: Install binaries
To get started as quickly as possible we recommend using pre-built binaries. Building from source is a more advanced option but will set you back an hour or more and you may encounter build errors.

[[info]]
| You can find how to build Antelope from source [here](https://developers.eos.io/manuals/eos/latest/install/build-from-source/).

The below commands will download binaries for respective operating systems.

### Mac OS X Brew Install:
```shell
brew tap eosio/eosio
brew install eosio
```

[[info]]
| If you don't have Brew installed, follow the installation instructions on the <a href="https://brew.sh/" target="_blank">official Brew website</a>.

### Ubuntu 18.04 Debian Package Install:
```shell
wget https://github.com/AntelopeIO/leap/releases/download/v2.0.0/eosio_2.0.0-1-ubuntu-18.04_amd64.deb
sudo apt install ./eosio_2.0.0-1-ubuntu-18.04_amd64.deb
```
### Ubuntu 16.04 Debian Package Install:
```shell
wget https://github.com/AntelopeIO/leap/releases/download/v2.0.0/eosio_2.0.0-1-ubuntu-16.04_amd64.deb
sudo apt install ./eosio_2.0.0-1-ubuntu-16.04_amd64.deb
```
### CentOS RPM Package Install:
```shell
wget https://github.com/AntelopeIO/leap/releases/download/v2.0.0/eosio-2.0.0-1.el7.x86_64.rpm
sudo yum install ./eosio-2.0.0-1.el7.x86_64.rpm
```

[[warning]]
| If you have previous versions of eosio installed on your system, please uninstall before proceeding. For detailed instructions, see [here](https://github.com/AntelopeIO/leap/blob/master/README.md).

## Step 2: Setup a development directory, stick to it.
You're going to need to pick a directory to work from, it's suggested to create a `contracts` directory somewhere on your local drive.
```shell
mkdir contracts
cd contracts
```

## Step 3: Enter your local directory below.
Get the path of that directory and save it for later, as you're going to need it, you can use the following command to get your absolute path.
```
pwd
```

Enter the absolute path to your contract directory below, and it will be inserted throughout the documentation to make your life a bit easier. _This functionality requires cookies_

![cli](../images/cli_2.2.2.gif)

<div class="eosio-helper-box">
    <form id="CONTRACTS_DIR">
        <label>Absolute Path to Contract Directory</label>
        <input class="helper-cookie" name="CONTRACTS_DIR" type="text" />
        <input type="submit" />
        <span></span>
    </form>
</div>

## What's Next?
- [Install the CDT](./04_install-the-CDT.md): Steps to install the Antelope Contract Development Toolkit (CDT) on your system.
