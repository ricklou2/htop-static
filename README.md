# htop — Fully Static Build

**Languages:** [English](#english) | [简体中文](#简体中文) | [繁體中文](#繁體中文)

---

## English

A standalone, fully static build of **htop 3.3.0** for four Linux architectures, bundled with the upstream source code. The executables have no dynamic interpreter and no shared-library dependencies, so they run directly on Android phones, embedded devices, and minimal Linux systems.

**Upstream version:** 3.3.0

### Executables

| File | Size | MD5 |
|---|---:|---|
| htop-linux-x64 | 620,056 B | `6a08a80dad2a50a7234b2651bf98b6bf` |
| htop-linux-x86 | 582,012 B | `21290abeb96afc9056579fb6f44e1918` |
| htop-linux-arm64 | 592,488 B | `d6f752d56a1f261f3b1eefb8410b17b0` |
| htop-linux-armhf | 541,872 B | `38f56d4653c6ddc2f319084def1673fe` |

### Static verification

```bash
llvm-readelf -l htop-linux-x64 | grep -c INTERP   # 0
llvm-readelf -d htop-linux-x64 | grep -c NEEDED   # 0
llvm-readelf -l htop-linux-x64 | awk '/LOAD/{print $NF}' | sort -u   # 0x4000
```

All four files report `INTERP=0`, `NEEDED=0`, and `LOAD align=0x4000` — musl static executables.

### Install and run

```bash
# Linux
install -m755 htop-linux-x64 /usr/local/bin/htop
htop --help

# Android (via adb)
adb push htop-linux-arm64 /data/local/tmp/htop
adb shell "chmod 755 /data/local/tmp/htop"
adb shell "/data/local/tmp/htop --help"
```

### Repository layout

```
bins/   Four executables (linux-x64/x86/arm64/armhf)
src/    htop-3.3.0.tar.xz
```

### Offline build

The source is included under `src/` for fully offline reproduction. The toolchain uses zig (musl cross-compilation):

```bash
tar xf src/htop-3.3.0.tar.xz && cd <source dir>
CC="zig cc -target x86_64-linux-musl -static" ./configure --host=x86_64-linux-musl --disable-shared --enable-static
make -j4
```

Architecture triples:

| Target | Linux triple |
|---|---|
| x64 | x86_64-linux-musl |
| x86 | x86-linux-musl |
| arm64 | aarch64-linux-musl |
| armhf | arm-linux-musleabihf |

Use `llvm-strip` for stripping (zig has no `strip` subcommand); unstripped binaries are 4–5 times larger.

### Known limitations

- A small number of sub-features that rely on host tools may be unavailable on very restricted systems.

---

## 简体中文

**htop 3.3.0** 的独立全静态构建包，覆盖四个 Linux 架构，并附上游源码。可执行文件无动态解释器、无共享库依赖，可直接在安卓手机、嵌入式设备和精简 Linux 系统上运行。

**上游版本：**3.3.0

### 可执行文件

| 文件 | 体积 | MD5 |
|---|---:|---|
| htop-linux-x64 | 620,056 B | `6a08a80dad2a50a7234b2651bf98b6bf` |
| htop-linux-x86 | 582,012 B | `21290abeb96afc9056579fb6f44e1918` |
| htop-linux-arm64 | 592,488 B | `d6f752d56a1f261f3b1eefb8410b17b0` |
| htop-linux-armhf | 541,872 B | `38f56d4653c6ddc2f319084def1673fe` |

### 静态性核验

```bash
llvm-readelf -l htop-linux-x64 | grep -c INTERP   # 0
llvm-readelf -d htop-linux-x64 | grep -c NEEDED   # 0
llvm-readelf -l htop-linux-x64 | awk '/LOAD/{print $NF}' | sort -u   # 0x4000
```

四个文件全部为 `INTERP=0`、`NEEDED=0`、`LOAD align=0x4000`：musl 静态可执行文件。

### 安装与运行

```bash
# Linux
install -m755 htop-linux-x64 /usr/local/bin/htop
htop --help

# Android（通过 adb）
adb push htop-linux-arm64 /data/local/tmp/htop
adb shell "chmod 755 /data/local/tmp/htop"
adb shell "/data/local/tmp/htop --help"
```

### 仓库结构

```
bins/   四个可执行文件（linux-x64/x86/arm64/armhf）
src/    htop-3.3.0.tar.xz
```

### 离线编译

源码位于 `src/`，可完全离线复现。工具链使用 zig（musl 交叉编译）：

```bash
tar xf src/htop-3.3.0.tar.xz && cd <源码目录>
CC="zig cc -target x86_64-linux-musl -static" ./configure --host=x86_64-linux-musl --disable-shared --enable-static
make -j4
```

架构 triple：

| 目标 | Linux triple |
|---|---|
| x64 | x86_64-linux-musl |
| x86 | x86-linux-musl |
| arm64 | aarch64-linux-musl |
| armhf | arm-linux-musleabihf |

strip 使用 `llvm-strip`（zig 无 `strip` 子命令），未 strip 体积会大 4~5 倍。

### 已知限制

- 在受限严重的系统上，个别依赖宿主工具的子功能可能不可用。

---

## 繁體中文

**htop 3.3.0** 的獨立全靜態建構包，涵蓋四個 Linux 架構，並附上游原始碼。可執行檔無動態直譯器、無共用程式庫相依性，可直接在安卓手機、嵌入式裝置和精簡 Linux 系統上執行。

**上游版本：**3.3.0

### 可執行檔

| 檔案 | 體積 | MD5 |
|---|---:|---|
| htop-linux-x64 | 620,056 B | `6a08a80dad2a50a7234b2651bf98b6bf` |
| htop-linux-x86 | 582,012 B | `21290abeb96afc9056579fb6f44e1918` |
| htop-linux-arm64 | 592,488 B | `d6f752d56a1f261f3b1eefb8410b17b0` |
| htop-linux-armhf | 541,872 B | `38f56d4653c6ddc2f319084def1673fe` |

### 靜態性核驗

```bash
llvm-readelf -l htop-linux-x64 | grep -c INTERP   # 0
llvm-readelf -d htop-linux-x64 | grep -c NEEDED   # 0
llvm-readelf -l htop-linux-x64 | awk '/LOAD/{print $NF}' | sort -u   # 0x4000
```

四個檔案全部為 `INTERP=0`、`NEEDED=0`、`LOAD align=0x4000`：musl 靜態可執行檔。

### 安裝與執行

```bash
# Linux
install -m755 htop-linux-x64 /usr/local/bin/htop
htop --help

# Android（透過 adb）
adb push htop-linux-arm64 /data/local/tmp/htop
adb shell "chmod 755 /data/local/tmp/htop"
adb shell "/data/local/tmp/htop --help"
```

### 倉庫結構

```
bins/   四個可執行檔（linux-x64/x86/arm64/armhf）
src/    htop-3.3.0.tar.xz
```

### 離線建構

原始碼位於 `src/`，可完全離線重現。工具鏈使用 zig（musl 交叉編譯）：

```bash
tar xf src/htop-3.3.0.tar.xz && cd <原始碼目錄>
CC="zig cc -target x86_64-linux-musl -static" ./configure --host=x86_64-linux-musl --disable-shared --enable-static
make -j4
```

架構 triple：

| 目標 | Linux triple |
|---|---|
| x64 | x86_64-linux-musl |
| x86 | x86-linux-musl |
| arm64 | aarch64-linux-musl |
| armhf | arm-linux-musleabihf |

strip 使用 `llvm-strip`（zig 無 `strip` 子命令），未 strip 體積會大 4~5 倍。

### 已知限制

- 在受限嚴重的系統上，個別依賴宿主工具的子功能可能無法使用。
