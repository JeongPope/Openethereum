# Openethereum

Build ethereum node using 'Openethereum'

## Table of Contents
1. [Description](#chapter-001)
2. [Building](#chapter-002)<br>
  2.1 [Building Dependencies](#chapter-0021)<br>
  2.2 [Building from Source Code](#chapter-0022)<br>
  2.3 [Starting OpenEthereum](#chapter-0023)
3. [Need to change](#chapter-003)
4. [Reference](#chapter-004)
5. [Example](#chapter-005)<br>
  5.1 [New address](#chapter-0051)<br>
  5.2 [Import Metamask](#chapter-0052)

## 1. Description <a id="chapter-001"></a>

I made it for the purpose of studying through 'Openethereum'.

#### Environment
- Ubuntu 20.04 LTS
- AWS Instance : c5.2xlarge, 150GB

## 2. Building <a id="chapter-002"></a>

### 2.1 Build Dependencies <a id="chapter-0021"></a>

OpenEthereum requires **latest stable Rust version** to build.

OpenEthereum recommend installing Rust through [rustup](https://www.rustup.rs/). 
If you don't already have `rustup`, you can install it like this:

- Linux:
  ```bash
  $ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```

  OpenEthereum also requires `clang` (>= 9.0), `clang++`, `pkg-config`, `file`, `make`, and `cmake` packages to be installed.
  ```bash
  $ sudo apt-get install build-essential cmake libudev-dev      // libudev
  $ sudo bash -c "$(wget -O - https://apt.llvm.org/llvm.sh)"    // clang
  $ sudo apt-get install -y pkg-config                          // pkg-config
  $ sudo apt-get install -y file                                // file
  $ sudo apt-get install -y make                                // make
  $ sudo apt-get install -y cmake                               // cmake
  ```

Once you have `rustup` installed, then you need to install:
* [Perl](https://www.perl.org)
* [Yasm](https://yasm.tortall.net)

#### Important
Make sure that these binaries are in your `PATH`. After that, you should be able to build OpenEthereum from source.

if `PATH` == $HOME/Openethereum,
```bash  
  wget https://www.tortall.net/project/yasm/releases/yasm-1.2.0.tar.gz
  tar xzvf yasm-1.2.0.tar.gz
  cd yasm-1.2.0
  ./configure --prefix="$Home/openethereum" --bindir="$Home/bin"
  make
  make install
  make distclean
```

### 2.2 Build from Source Code <a id="chapter-0022"></a>

```bash
# download OpenEthereum code
$ git clone https://github.com/openethereum/openethereum
$ cd openethereum

# build in release mode
$ cargo build --release --features final
```

This produces an executable in the `./target/release` subdirectory.

Note: if cargo fails to parse manifest try:

```bash
$ ~/.cargo/bin/cargo build --release
```

Note, when compiling a crate and you receive errors, it's in most cases your outdated version of Rust, or some of your crates have to be recompiled. Cleaning the repository will most likely solve the issue if you are on the latest stable version of Rust, try:

```bash
$ cargo clean
```

This always compiles the latest nightly builds. If you want to build stable, do a

```bash
$ git checkout stable
```

### 2.3 Starting OpenEthereum <a id="chapter-0023"></a>

#### Custom
To start OpenEthereum with config,
```bash
$ ./syncNode.sh
```

#### Manually

To start OpenEthereum manually, just run

```bash
$ ./target/release/openethereum
```

so OpenEthereum begins syncing the Ethereum blockchain.

#### Using `systemd` service file

To start OpenEthereum as a regular user using `systemd` init:

1. Copy `./scripts/openethereum.service` to your
`systemd` user directory (usually `~/.config/systemd/user`).
2. Copy release to bin folder, write `sudo install ./target/release/openethereum /usr/bin/openethereum`
3. To configure OpenEthereum, see [our old wiki](https://openethereum.github.io/wiki/index) for details.

## 3. Need to change <a id="chapter-003"></a>

- openethereum/config.toml : chain, base_path, db_path, keys_path
- openethereum/syncNode.sh : config.toml path

## 4. Reference <a id="chapter-004"></a>

- [Openethereum](https://github.com/openethereum/openethereum)
- [Openethereum old wiki](https://openethereum.github.io/wiki/index)

## 5. Example <a id="chapter-005"></a>

### 5.1 New address <a id="chapter-0051"></a>
```bash
$ curl -H "Content-Type: application/json" -X POST localhost:8545 --data '{"method":"personal_newAccount","params":["YOURPASSWORD"],"id":1,"jsonrpc":"2.0"}'
```

### 5.2 Import Metamask <a id="chapter-0052"></a>
```bash
$ curl --data '{"method":"parity_exportAccount","params":["YOUR ADDRESS","YOUR PASSWORD"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545
```

Use the following three values as JSON among the results.
- result
- id
- version

You must follow the format of V3 Wallet.
