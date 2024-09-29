# outline-go-tun2socks

> [!WARNING]  
> This repository is no longer being maintained. The tun2socks source is now maintained in the [outline-apps repository](https://github.com/Jigsaw-Code/outline-apps/tree/master) and [Intra repository](https://github.com/Jigsaw-Code/Intra/tree/master/Android/app/src/go).

Go package for building [go-tun2socks](https://github.com/eycorsican/go-tun2socks)-based clients for [Outline](https://getoutline.org) and [Intra](https://getintra.org) (now with support for [Choir](https://github.com/Jigsaw-Code/choir) metrics).  For macOS, iOS, and Android, the output is a library; for Linux and Windows it is a command-line executable.

## Prerequisites

- macOS host (iOS, macOS)
- make
- Go >= 1.18
- A C compiler (e.g.: clang, gcc)

## Android

### Set up

- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager)
  1. Download the command line tools from https://developer.android.com/studio.
  1. Unzip the pacakge as `~/Android/Sdk/cmdline-tools/latest/`. Make sure `sdkmanager` is located at `~/Android/Sdk/cmdline-tools/latest/bin/sdkmanager`
- Android NDK 23
  1. Install the NDK with `~/Android/Sdk/cmdline-tools/latest/bin/sdkmanager "platforms;android-30" "ndk;23.1.7779620"` (platform from [outline-client](https://github.com/Jigsaw-Code/outline-client#building-the-android-app), exact NDK 23 version obtained from `sdkmanager --list`)
  1. Set up the environment variables:
     ```
     export ANDROID_NDK_HOME=~/Android/Sdk/ndk/23.1.7779620 ANDROID_HOME=~/Android/Sdk
     ```
- [gomobile](https://pkg.go.dev/golang.org/x/mobile/cmd/gobind) (installed as needed by `make`)

### Build

```bash
make clean && make android
```
This will create `build/android/{tun2socks.aar,tun2socks-sources.jar}`

If needed, you can extract the jni files into `build/android/jni` with:
```bash
unzip build/android/tun2socks.aar 'jni/*' -d build/android
```

## Apple (iOS and macOS)

### Set up

- Xcode
- [gomobile](https://pkg.go.dev/golang.org/x/mobile/cmd/gobind) (installed as needed by `make`)


### Build
```
make clean && make apple
```
This will create `build/apple/Tun2socks.xcframework`.

## Linux and Windows

We build binaries for Linux and Windows from source without any custom integrations. `xgo` and Docker are required to support cross-compilation.

### Set up

- [Docker](https://docs.docker.com/get-docker/) (for xgo)
- [xgo](https://github.com/crazy-max/xgo) (installed as needed by `make`)
- [ghcr.io/crazy-max/xgo Docker image](https://github.com/crazy-max/xgo/pkgs/container/xgo). This is pulled automatically by xgo and takes ~6.8 GB of disk space.

## Build

For Linux:
```
make clean && make linux
```
This will create `build/linux/tun2socks`.

For Windows:
```
make clean && make windows
```
This will create `build/windows/tun2socks.exe`.

## Intra (Android)

Same set up as for the Outline Android library.

Build with:

```bash
make clean && make intra
```
This will create `build/intra/{tun2socks.aar,tun2socks-sources.jar}`

## addtionly：

需要本地安装go环境， 以及gomobile， 通常仅仅是安装了xcode 命令行工具还不够，还需要 配置完整的命令行工具
安装好go环境之后，直接跑 make clean && make apple； makefile 会帮我们安装gomobile环境

### 如何修改Tun2socks.xcframework 的名字？

讲 Makefile 文件中的Tun2socks.xcframework 替换为你想要的名字 eg： MyAppTun2socks.xcframework
重新再跑命令打包即可

- [x] 制作swift package 或 cocoapods 