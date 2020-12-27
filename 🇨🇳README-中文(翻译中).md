<div align="center">
<p>
    <img width="80" src="https://raw.githubusercontent.com/donnisnoni95/v-logo/master/dist/v-logo.svg?sanitize=true">
</p>
<h1>V语言设计 -- 中文🇨🇳</h1>

[vlang.io](https://vlang.io) |
[Docs](https://github.com/vlang/v/blob/master/doc/docs.md) |
[Changelog](https://github.com/vlang/v/blob/master/CHANGELOG.md) |
[Speed](https://fast.vlang.io/) |
[Contributing & compiler design](https://github.com/vlang/v/blob/master/CONTRIBUTING.md)

</div>
<div align="center">

[![Build Status][WorkflowBadge]][WorkflowUrl]
[![Sponsor][SponsorBadge]][SponsorUrl]
[![Patreon][PatreonBadge]][PatreonUrl]
[![Discord][DiscordBadge]][DiscordUrl]
[![Twitter][TwitterUrl]][TwitterBadge]

</div>


### 译者的话（TRANSLATOR'S WORDS）：本文档疏漏甚多，请大家一起完善😃
## V的关键功能

- 简单: 一门学会用不了半时辰的语言
- 快速编译: ≈80k loc/s with a Clang backend,
    ≈1 million loc/s with x64 and tcc backends *(Intel i5-7500, SSD, no optimization)*
- 易于开发: V compiles itself in less than a second
- 实战:和C语言一样快(V语言的主要后端编译至人读得懂的C语言代码)
- 安全性: 无null, 无globals,无未定义行为, immutability by default
- 可以实现C语言到V语言的翻译
- 热重载
- [创新内存管理](https://vlang.io/#memory)
- [跨平台用户界面图书馆](https://github.com/vlang/ui)
- 内嵌图形图书馆
- 简单地交叉编译
- REPL
- [内嵌ORM](https://github.com/vlang/v/blob/master/doc/docs.md#orm)
- [内嵌网络框架](https://github.com/vlang/v/blob/master/vlib/vweb/README.md)
- C和JavaScript的后端

## 稳定性保证和未来变化

尽管处于早期开发阶段，V语言相对稳定，并且
向后兼容性保证，这意味着您今天编写的代码是保证的
工作一个月，一年，或五年后。

在 1.0 版本之前，仍有小语法更改，但将处理这些更改
自动通过`vfmt`，就像过去一样。

V 核心 API（主要是`os`模块）仍将有细微的更改，直到
它们在2020年稳定下来。当然，API 将增长之后，但不打破
现有代码。

与许多其他语言不同，V 不会总是更改，具有新功能
正在引入和修改旧功能。它总是会是一个小而简单的
语言，非常类似于它现在的方式。

## 下载V源码

### Linux, macOS, Windows, *BSD, Solaris, WSL, Android, Raspbian

```bash
git clone https://github.com/vlang/v
cd v
make
```

就是这样！现在，您有一个 V 可执行文件在 `[路径到 V 存储库/v`.
`[到 V 存储库的路径]可以在任何地方。

(Windows上`make`指运行`make.bat`,所以确保你用`cmd.exe`)

你可以试着`./v run examples/hello_world.v` (`v.exe` on Windows).

V is being constantly updated. To update V, simply run:

```bash
v up
```

### C compiler

It's recommended to use Clang or GCC or Visual Studio.
If you are doing development, you most likely already have one of those installed.

Otherwise, follow these instructions:

- [Installing a C compiler on Linux and macOS](https://github.com/vlang/v/wiki/Installing-a-C-compiler-on-Linux-and-macOS)

- [Installing a C compiler on Windows](https://github.com/vlang/v/wiki/Installing-a-C-compiler-on-Windows)

However, if none is found when running `make` on Linux or Windows,
TCC would be downloaded and set as an alternative C backend.
It's very lightweight (several MB) so this shouldn't take too long.

### Symlinking

NB: it is *highly recommended*, that you put V on your PATH. That saves
you the effort to type in the full path to your v executable every time.
V provides a convenience `v symlink` command to do that more easily.

On Unix systems, it creates a `/usr/local/bin/v` symlink to your
executable. To do that, run:

```bash
sudo ./v symlink
```

On Windows, start a new shell with administrative privileges, for
example by <kbd>Windows Key</kbd>, then type `cmd.exe`, right click on its menu
entry, and choose `Run as administrator`. In the new administrative
shell, cd to the path, where you have compiled v.exe, then type:

```bat
.\v.exe symlink
```

That will make v available everywhere, by adding it to your PATH.
Please restart your shell/editor after that, so that it can pick
the new PATH variable.

NB: there is no need to run `v symlink` more than once - v will
continue to be available, even after `v up`, restarts and so on.
You only need to run it again, if you decide to move the V repo
folder somewhere else.

### Docker

<details><summary>Expand Docker instructions</summary>

```bash
git clone https://github.com/vlang/v
cd v
docker build -t vlang .
docker run --rm -it vlang:latest
v
```

### Docker with Alpine/musl

```bash
git clone https://github.com/vlang/v
cd v
docker build -t vlang --file=Dockerfile.alpine .
docker run --rm -it vlang:latest
/usr/local/v/v
```

</details>

### Testing and running the examples

Make sure V can compile itself:

```bash
v self
```

```bash
$ v
V 0.2.x
Use Ctrl-C or `exit` to exit

>>> println('hello world')
hello world
>>>
```

```bash
cd examples
v hello_world.v && ./hello_world    # or simply
v run hello_world.v                 # this builds the program and runs it right away

v word_counter.v && ./word_counter cinderella.txt
v run news_fetcher.v
v run tetris/tetris.v
```

<img src='https://raw.githubusercontent.com/vlang/v/master/examples/tetris/screenshot.png' width=300>

NB: In order to build Tetris or 2048 (or anything else using `sokol` or  `gg` graphics modules)
on some Linux systems, you need to install `libxi-dev` and `libxcursor-dev` .

If you plan to use the http package, you also need to install OpenSSL on non-Windows systems.

```bash
macOS:
brew install openssl

Debian/Ubuntu:
sudo apt install libssl-dev

Arch/Manjaro:
openssl is installed by default

Fedora:
sudo dnf install openssl-devel
```

## V sync
V's `sync` module and channel implementation uses libatomic.
It is most likely already installed on your system, but if not,
you can install it, by doing the following:
```bash
MacOS: already installed

Debian/Ubuntu:
sudo apt install libatomic1

Fedora/CentOS/RH:
sudo dnf install libatomic-static
```

## V UI

<a href="https://github.com/vlang/ui">
<img src='https://raw.githubusercontent.com/vlang/ui/master/examples/screenshot.png' width=712>
</a>

https://github.com/vlang/ui

<!---
## JavaScript backend

[examples/hello_v_js.v](examples/hello_v_js.v):

```v
fn main() {
	for i in 0 .. 3 {
		println('Hello from V.js')
	}
}
```

```bash
v -o hi.js examples/hello_v_js.v && node hi.js
Hello from V.js
Hello from V.js
Hello from V.js
```
-->

## Developing web applications

Check out the [Building a simple web blog](https://github.com/vlang/v/blob/master/tutorials/building-a-simple-web-blog-with-vweb.md)
tutorial and Gitly, a light and fast alternative to GitHub/GitLab:

https://github.com/vlang/gitly

<img src="https://user-images.githubusercontent.com/687996/85933714-b195fe80-b8da-11ea-9ddd-09cadc2103e4.png">

## 抓捕问题

请看我们的[wiki界面](https://github.com/vlang/v/wiki)上的[抓捕问题](https://github.com/vlang/v/wiki/Troubleshooting)栏目 

[WorkflowBadge]: https://github.com/vlang/v/workflows/CI/badge.svg
[DiscordBadge]: https://img.shields.io/discord/592103645835821068?label=Discord&logo=discord&logoColor=white
[PatreonBadge]: https://img.shields.io/endpoint.svg?url=https%3A%2F%2Fshieldsio-patreon.vercel.app%2Fapi%3Fusername%3Dvlang%26type%3Dpledges
[SponsorBadge]: https://camo.githubusercontent.com/da8bc40db5ed31e4b12660245535b5db67aa03ce/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f6c6162656c3d53706f6e736f72266d6573736167653d254532253944254134266c6f676f3d476974487562
[TwitterBadge]: https://twitter.com/v_language

[WorkflowUrl]: https://github.com/vlang/v/commits/master
[DiscordUrl]: https://discord.gg/vlang
[PatreonUrl]: https://patreon.com/vlang
[SponsorUrl]: https://github.com/sponsors/medvednikov
[TwitterUrl]: https://img.shields.io/twitter/follow/v_language.svg?style=flatl&label=Follow&logo=twitter&logoColor=white&color=1da1f2
