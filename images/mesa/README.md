<h1 align="center">
	Mesa Package & Software Rendering
</h1>


Mesa introduction blablablablabla

due to lacking GPU rendering power, I investigated online about software rendering. With the instruction from the technician, I have gained the knowledge of Mesa package, LLVM software rendering pipeline with SCon compilation and obtained a very detailed instruction noting the ways tackling all compilation conflicts during construction of software render environment. Other than that, I learned writing Cmakefiles from scratch in order to customise the compilation process.

</br>

## Fetching Mesa/OpenGL Version Information

Firstly, before installing any package, we need an overview of the environment and version information of existing dependencies. These implementations are included in the mesa version management package `mesa-utils`, install it by:

```bash
$ sudo apt install mesa-utils
```
After installing the `mesa-utils` package, use the terminal command `glxinfo` to detect existing OpenGL and Mesa and fetch their version information.

> using `grep` to filter results, `-i` tells the program to ignore cases when filtering results

```bash
$ glxinfo | grep -i opengl
```
Without manually installing latest version of Mesa and related dependencies, the terminal should return some raw information similar to:

```bash
server glx version string: 1.4
client glx version string: 1.4
GLX version: 1.4
    Max core profile version: 3.3
    Max compat profile version: 3.0
    Max GLES1 profile version: 1.1
    Max GLES[23] profile version: 3.0
OpenGL version string: 3.0 Mesa 17.2.8
OpenGL shading language version string: 1.30
```

If in the `OpenGL renderer string`: you see `llvmpipe`, that means your system doesn't use the GPU but the CPU software renderer instead to render the computer graphics. (In this guidance, we are considering cases only using ubuntu 16.04 CPU server without graphic card presenting)

> The command `glxinfo` will be frequently used throughout the environment building process to check for version information after each step.

</br>

## Getting Mesa

Download various versions of Mesa packages from <https://mesa.freedesktop.org/archive/>

> check <https://www.mesa3d.org/relnotes/> for release notes of each version. In this guidance, we are referring to the latest stable release version at the moment: 18.1.6

> When a new release is coming, release candidates (betas) may be found in the same directory, and are recognisable by the `mesa-Y.N.P-rcX.tar.gz` filename

### Unpacking

Mesa releases are available in two formats: `.tar.xz` and `.tar.gz`. To unpack the tarball:

```
$ tar xf mesa-Y.N.P.tar.xz
```
or

```
$ tar xf mesa-Y.N.P.tar.gz
```

## Prerequisites for Building

### Compilers

The following compilers are known to work at the moment, you can use other compilers maintain support.

#### GCC

GCC needs version 4.2.0 or later (some parts of Mesa may require later versions). Check the GCC version by:

```bash
$ gcc --version
```
which should return something similar to:

```
gcc (Ubuntu 5.4.0-6ubuntu1~16.04.10) 5.4.0 20160609
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

#### Clang/LLVM

Ubuntu only provides a relatively old clang 3.8 (with llvm-3.8). If you run:

```bash
sudo apt-get install clang
```
installs version 3.8 wich is a really old version Ubuntu 16.04 ships with. Luckily, Apple creates deb packages and maintains a server for all llvm/clang releases and most Ubuntu distros.

> We can check check which LLVM version is currently in use by typing the command:
>
> ```bash
> $ llvm-config --prefix --version
> ```
> which should return the following at the moment:
>
> ```
> /usr/lib/llvm-3.8
> 3.8.0
> ```

> This guidance mainly follows the instruction from this website for <https://blog.kowalczyk.info/article/k/how-to-install-latest-clang-6.0-on-ubuntu-16.04-xenial-wsl.html>

`apt` queries package servers to get a list of available deb packages. Default Ubuntu installation knows about official Ubuntu servers. To install latest stable version (currently 6.0 when writing this guidance) we need to run a series of commands to customise our own server to provide additional packages.

List of servers is in `/etc/apt/sources.list`. Here’s how it looks by default on Ubuntu 16.04:

```bash
$ cat /etc/apt/sources.list
```
which returns:

```
deb http://archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse
```

For security, packages are signed with private keys. You need public key to verify package signature.

```bash
$ wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | sudo apt-key add -
```
This command downloads llvm’s server public key.

> If the network keeps `connecting` state without any actual response, you might probably not have access to the server, adding proxies for your local server and for `apt` command:
>
> > replace all the `[W3_account]:[W3_password]` below with real accounts and passwords
>
> ```bash
> $ export http_proxy=http://[W3_account]:[W3_password]@proxyhk.huawei.com:8080
> $ export https_proxy=http://[W3_account]:[W3_password]@proxyhk.huawei.com:8080
> ```
> > remember to export proxy everytime you open a new window of server
>
> #### `apt` proxy setting:
>
> ```bash
> $ gedit /etc/apt/apt.conf
> ```
> and add the following lines into the file:
> 
> ```bash
> Acquire::http::Proxy "http://[W3_account]:[W3_password]@proxyhk.huawei.com:8080";
> Acquire::https::Proxy "http://[W3_account]:[W3_password]@proxyhk.huawei.com:8080";
> Acquire::ftp::Proxy "http://[W3_account]:[W3_password]@proxyhk.huawei.com:8080";
> Acquire::https::download.docker.com::Verify-peer "false";
> ```
>
> You might potentially encounter a verification problem similar to the below:
> ```
> ERROR: cannot verify apt.llvm.org's certificate, issued by ‘CN=Huawei Web Secure Internet Gateway CA,OU=IT,O=Huawei,L=Shenzhen,ST=GuangDong,C=cn’:
>  Unable to locally verify the issuer's authority.
> To connect to apt.llvm.org insecurely, use `--no-check-certificate'.
> ```
> Just follow the instruction and type:
>
> ```
> $ wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key --no-check-certificate | sudo apt-key add -
> ```

After that, type the following command:

```bash
$ sudo apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-6.0 main"
```
adds `llvm`’s server for Ubuntu 16.04 to `/etc/apt/sources.list`.

```bash
$ sudo apt-get update 
```
downloads the latest list of packages from all servers, including the one we just added.

```bash
$ sudo apt-get install -y clang-6.0 lld-6.0
```
> Flag -y disables confirmation prompt.

Lastly, reconfigure the system so that clang refers to clang 6:

```bash
$ update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-3.8 100
$ update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-6.0 1000
$ update-alternatives --install /usr/bin/clang++ clang /usr/bin/clang-3.8 100
$ update-alternatives --install /usr/bin/clang clang /usr/bin/clang-3.8 100
$ update-alternatives --install /usr/bin/clang clang /usr/bin/clang-6.0 1000
$ update-alternatives --config clang
$ update-alternatives --config clang++
```
Installing clang is not yet done for using, in order to add it into the environment path, we first find the location of various versions fo LLVM:

```bash
$ dpkg -L clang-6.0
$ ls /usr/lib/ | grep llvm
```
and it is VERY IMPORTANT to export the related path to makes `llvm-6.0` the default llvm package, this is VERY VERY IMPORTANT for later command `./configure` when using `autoconf` or otherwise a problem of `r600` requires `llvm` version higher than 3.9 will be raised.

```bash
$ export PATH="/usr/lib/llvm-6.0/bin:$PATH"
```
and then `echo $PATH` to check `/usr/lib/llvm-6.0/bin` presenting.

After this, type the following command again:

```bash
$ llvm-config --prefix --version
```
This time, instead of 3.8 version:

```
/usr/lib/llvm-6.0
6.0.1
```
will be displayed.

<br>

## Building with Autoconf (Linux/Unix/X11)

The primary method to build Mesa on Unix systems is with autoconf. The general approach is the standard:

```bash
$ ./configure
```
> You might encounter error code of 
> 
> ```
> error: /home/[username]/anaconda2/lib/libreadline.so.6: undefined symbol: PC
> ```
> This problem has been caused by the conflict between the configuration of anaconda python and the system python if you have anaconda installed. One brute-force solution is:
> 
> ```bash
> $ cp -p /lib/x86_64-linux-gnu/libreadline.so.6 ~/anaconda2/lib/libreadline.so.6
> ```
> for python 3 on various machines:
> 
> ```bash
> $ cp /lib64/libreadline.so.6 ~/anaconda3/lib/libreadline.so.6
> ```

> You might also encounter error code of
>
> ```
> configure: error: r600 requires libelf when using llvm
> ```
> This implies that `elfutils` dependency is needed by r600 with `llvm` support and radeonSI. 
>
> ```bash
> $ sudo apt-get install elfutils
> ```
> 
> lastly, let the program search for the path that contains the header file `libelf.h` to finally tackle the r600 driver problem
> 
> sometimes `libelf.h` not automatically installed side with elfutils, manually install:
>
> ```bash
> $ sudo apt install libelf-dev`
> ```
> to install the package and 
>
> ```bash
> $ sudo find / -name libelf.h
> ``` 
> looking for the path of `libelf`, in this case returned: `/usr/include/libelf.h`
> 
> ```bash
> $ export PATH="/usr/include:$PATH"
> ```
> > **Extended reading**: check elfutils-libelf isn’t libelf! <https://nanxiao.me/en/elfutils-libelf-isnt-libelf/>

After successfully running the `./configure` command, proceed to the `make` procedure by:

```bash
$ make -j $(nproc)
$ sudo make install
```

Finally, when installation is done, in order to let the program find the crucial `libGL.so`, type the command:

```bash
$ sudo find / -name libGL.so
``` 
and export the path, in this case:

```bash
$ export PATH="/usr/lib/x86_64-linux-gnu:$PATH"
$ export PATH="/usr/lib/x86_64-linux-gnu/mesa:$PATH"
```
and check the version information again by:

```bash
$ glxinfo | grep -i opengl
```
which returns:

```
OpenGL vendor string: VMware, Inc.
OpenGL renderer string: llvmpipe (LLVM 6.0, 256 bits)
OpenGL version string: 3.1 Mesa 18.2.0-rc2
OpenGL shading language version string: 1.40
OpenGL context flags: (none)
OpenGL extensions:
```

## SCons

navigate to the mesa directory and do:

```bash
$ scons
```
> You might probably get the error code:
>
> ```
> Traceback (most recent call last):
>   File "src/compiler/glsl/ir_expression_operation.py", line 23, in <module>
>     import mako.template
> ImportError: No module named mako.template
> ```
> This can be solved installing `Mako`
>
> ```bash
> $ pip install Mako
> ```
> > in this case `mako` template has been installed into python `dist-package` directory, in order to let the program find the package, type the following command:
>
> ```bash
> $ sudo gedit ~/.bashrc
> ```
> and add the following lines into the file:
>
> ```
> # let the program find mako template
> export PATH="/usr/local/lib/python2.7/dist-packages:$PATH"
> ```

> You might also get the error:
>
> ```
> ...
> sh: 1: lex: not found
> ...
> sh: 1: yacc: not found
> ...
> ```
> solve the above problem by:
> 
> ```bash
> $ sudo apt-get install bison flex
> ```

Finally `scons` finished with:

```
...
  Linking build/linux-x86_64-debug/gallium/targets/libgl-xlib/libGL.so.1.5 ...
ln -sf libGL.so.1.5 build/linux-x86_64-debug/gallium/targets/libgl-xlib/libGL.so.1
ln -sf libGL.so.1 build/linux-x86_64-debug/gallium/targets/libgl-xlib/libGL.so
```
On Linux, building will create a drop-in alternative for `libGL.so` into the above directory

eventually set the `LD_LIBRARY_PATH` environment variable accordingly 


```bash
$ export LD_LIBRARY_PATH="/home/hrk/pkgs/mesa-18.2.0-rc2/build/linux-x86_64-debug/gallium/targets/libgl-xlib:$LD_LIBRARY_PATH"
```
> place the command lines into `~/.bashrc` and run `source ~/.bashrc` instead of typing all the cumbersome commands every time opening a new terminal 

and see the miracle happens:

```
glxinfo | grep -i opengl
```

returned:

```
OpenGL vendor string: VMware, Inc.
OpenGL renderer string: llvmpipe (LLVM 3.8, 256 bits)
OpenGL core profile version string: 3.3 (Core Profile) Mesa 18.2.0-rc2
OpenGL core profile shading language version string: 3.30
OpenGL core profile context flags: (none)
OpenGL core profile profile mask: core profile
OpenGL core profile extensions:
OpenGL version string: 3.1 Mesa 18.2.0-rc2
OpenGL shading language version string: 1.40
OpenGL context flags: (none)
OpenGL extensions:
OpenGL ES profile version string: OpenGL ES 3.0 Mesa 18.2.0-rc2
OpenGL ES profile shading language version string: OpenGL ES GLSL ES 3.00
OpenGL ES profile extensions:
```

left the `llvmpipe`(LLVM 3.8) problem unsolved









