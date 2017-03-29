# Base Runtime
The base application runtime and hardware abstraction layer.

## Scope
A project closely linked to the Modularity Initiative, Base Runtime
is about the defining the common shared package and feature set of the
operating system, providing both the hardware enablement layer and the
minimal application runtime environment other modules can build upon.
Besides the Base Runtime module itself, the project also focuses on
delivery of additional low-level modular content, such as distribution
management and configuration utilities, basic system services or
infrastructure build environments.

With the Modularity infrastructure gradually becoming available, it is
time to focus on the modular content as well. The goal of the Base Runtime
project is to define the minimal shared core of the operating system
that would light up the hardware (where applicable) and provide runtime
environment for other, more complex and feature-rich modules.  The idea
is to deliver this functionality as a set of modules or a module stack.

Besides the Base Runtime module itself, the team is responsible for a
number of additional low level modules that define the platform, such
as software management and configuration tools or buildroots.

## Modules
The team currently focuses on development of two separate modules.

* `base-runtime`, which provides a fairly minimal set of runtime environment packages, suitable for creation of container base images as well as bootable bare metal installations for all supported architectures; the module also provides the minimal buildroot, although that might change in the future
* `bootstrap`, which provides a self-hosting build environment for the `base-runtime` module; a special-purpose, unsupported module, it shouldn't be used by anyone but the Base Runtime team

## API
This section documents all the source and binary packages included in
the Base Runtime along with brief reasoning why the particular component
was included or excluded.

The component list is based on the output of
[whatpkgs](https://github.com/fedora-modularity/whatpkgs) ran against
Fedora 26 dist-git branch point metadata.  The input for teh depsolver
can be found in the `toplevel-binary-packages.txt` file.

Decisions whether a particular subpackage should be included or not is
based on a small set of basic rules:

* if the package is in the toplevel set, it's included, obviously
* if the package is in the runtime dependency chain of a toplevel package, it's also included, also obviously
* if the package provides development environment for another package provided by the module, it should be included
* if including the package doesn't require any additional components to be added to the module, it should be included; avoid package splits and filtering where possible, even at the cost of adding new components to the set, provided the new components are small, stable, and have minimal dependency chains; use common sense
* no Python or other interpreted languages bindings unless necessary

Note the API list was mostly done by hand and may therefore contain errors.

Also keep in mind that having a package available in the Base Runtime set
doesn't mean it has to be installed on the system or be part of the Base
Runtime-based base container image.  It means the packages are available
in the repository, supported and can be installed the the user wishes
to do so.

* `acl`
    * `acl`, pulled in by the depsolver
    * `libacl`, pulled in by the depsolver
    * `libacl-devel`, since `libacl` is our API

* `attr`
    * `attr`, preventing a split
        * depends on `glibc`
        * depends on `libattr`
    * `libattr`, pulled in by the depsolver
    * `libattr-devel`, since `libattr` is our API

* `audit`
    * ~~`audispd-plugins`~~, requires `audit`
    * ~~`audispd-plugins-zos`~~, requires `audit`
    * ~~`audit`~~, requires `tcp_wrappers-libs`
    * `audit-libs`, pulled in by the depsolver
    * `audit-libs-devel`, since `audit-libs` is our API
    * ~~`audit-libs-python`~~, requires `python2`
    * ~~`audit-libs-python3`~~, requires `python3`
    * `audit-libs-static`, since `audit-libs` is our API

* `babeltrace`
    * `babeltrace`, preventing a split
        * depends on `elfutils-libelf`
        * depends on `elfutils-libs`
        * depends on `glib2`
        * depends on `glibc`
        * depends on `libbabeltrace`
        * depends on `libuuid`
        * depends on `popt`
    * `libbabeltrace`, pulled in by the depsolver
    * ~~`libbabeltrace-devel`~~, **temporarily removed**, needed to support `libbabeltrace` but requires `glib2-devel`
    * ~~`python3-babeltrace`~~, requires `python3`

* `basesystem`
    * `basesystem`, pulled in by the depsolver

* `bash`
    * `bash`, pulled in by the depsolver
    * `bash-doc`, preventing a split
        * depends on `bash`

* `binutils`
    * `binutils`, pulled in by the depsolver
    * `binuilts-devel`, since `binutils` is our API

* `byacc`
    * `byacc`, pulled in by the depsolver

* `bzip2`
    * `bzip2`, pulled in by the depsolver
    * `bzip2-devel`, since `bzip2` is our API
    * `bzip2-libs`, pulled in by the depsolver
    * `bzip2-static`, since `bzip2` is our API

* `ca-certificates`
    * `ca-certificates`, pulled in by the depsolver

* `chkconfig`
    * `chkconfig`, pulled in by the depsolver
    * ~~`ntsysv`~~, requires `newt`, deliberately unsupported

* `coreutils`
    * `coreutils`, pulled in by the depsolver
    * `coreutils-common`, pulled in by the depsolver
    * `coreutils-single`, preventing a split, possibly useful for minimized images

* `cpio`
    * `cpio`, pulled in by the depsolver

* `cracklib`
    * `cracklib`, pulled in by the depsolver
    * `cracklib-devel`, since `cracklib` is our API
    * `cracklib-dicts`, preventing a split
        * depends on `cracklib`
    * ~~`cracklib-python`~~, requires `python2`

* `crypto-policies`
    * `crypto-policies`, pulled in by the deplsover

* `cryptsetup`
    * `cryptsetup`, preventing a split
        * depends on `cryptsetup-libs`
        * depends on `glibc`
        * depends on `libpwquality`
        * depends on `popt`
    * `cryptsetup-devel`, since `cryptsetup` is our API
    * `cryptsetup-libs`, pulled in by the depsolver
    * ~~`cryptsetup-python`~~, requires `python2`
    * ~~`cryptsetup-python3`~~, requires `python3`
    * `cryptsetup-reencrypt`, preventing a split
        * depends on `cryptsetup-libs`
        * depends on `glibc`
        * depends on `libpwquality`
        * depends on `libuuid`
        * depends on `popt`
    * `veritysetup`, preventing a split
        * depends on `cryptsetup-libs`
        * depends on `glibc`
        * depends on `popt`

* `curl`
    * `curl`, pulled in by the depsolver
    * `libcurl`, pulled in by the depsolver
    * `libcurl-devel`, since `libcurl` is our API

* `cyrus-sasl`
    * `cyrus-sasl`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `krb5-libs`
        * depends on `libcom_err`
        * depends on `libcrypt`
        * depends on `libdb`
        * depends on `openldap`
        * depends on `openssl-libs`
        * depends on `pam`
        * depends on `shadow-utils`
        * depends on `systemd`
        * depends on `util-linux`
    * `cyrus-sasl-devel`, since `cyrus-sasl-lib` is our API
    * `cyrus-sasl-gs2`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `krb5-libs`
        * depends on `libcom_err`
        * depends on `libcrypt`
    * `cyrus-sasl-gssapi`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `krb5-libs`
        * depends on `libcom_err`
        * depends on `libcrypt`
    * `cyrus-sasl-ldap`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `libcrypt`
        * depends on `openldap`
    * `cyrus-sasl-lib`, pulled in by the depsolver
    * `cyrus-sasl-md5`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `libcrypt`
        * depends on `openssl-libs`
    * `cyrus-sasl-ntlm`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `libcrypt`
        * depends on `openssl-libs`
    * `cyrus-sasl-plain`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `libcrypt`
    * `cyrus-sasl-scram`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `libcrypt`
        * depends on `openssl-libs`
    * ~~`cyrus-sasl-sql`~~, requires `mariadb-libs` and `postgresql-libs`, deliberately unsupported

* `dbus`
    * `dbus`, pulled in by the depsolver
    * ~~`dbus-devel`~~, **temporarily removed**, requires `xml-common`
    * `dbus-doc`, preventing a split
        * depends on `coreutils`
        * depends on `dbus`
    * `dbus-libs`, pulled in by the depsolver
    * `dbus-tests`, preventing a split
        * depends on `bash`
        * depends on `dbus`
        * depends on `dbus-libs`
        * depends on `glib2`
        * depends on `glibc`
        * depends on `systemd-libs`
    * ~~`dbus-x11`~~, requires `libX11` and `xorg-x11-xinit`

* `diffutils`
    * `diffutils`, pulled in by the depsolver

* `dracut`
    * `dracut`, pulled in by the depsolver
    * `dracut-caps`, preventing a split
        * depends on `bash`
        * depends on `dracut`
        * depends on `libcap`
    * `dracut-config-generic`, preventing a split
        * depends on `dracut`
    * `dracut-config-rescue`, preventing a split
        * depends on `bash`
        * depends on `dracut`
    * ~~`dracut-fips`~~, no need for FIPS, requries `hmaccalc`
    * ~~`dracut-fips-aesni`~~, no need for FIPS, requires `dracut-fips`
    * ~~`dracut-live`~~, requires `dracut-network`
    * ~~`dracut-network`~~, requires `dhcp-client`, `iproute` and `iputils`
    * `dracut-tools`, preventing a split
        * depends on `bash`
        * depends on `dracut`

* `dwz`
    * `dwz`, pulled in by the depsolver

* `e2fsprogs`
    * `e2fsprogs`, preventing a split
    * `e2fsprogs-devel`, since `e2fsprogs-libs` is our API
    * `e2fsprogs-libs`, preventing a split
        * depends on `glibc`
        * depends on `libcom_err`
    * `e2fsprogs-static`, since `e2fsprogs-libs` is our API
    * `libcom_err`, pulled in by the depsolver
    * `libcom_err-devel`, since `libcom_err` is our API
    * `libss`, preventing a split
        * depends on `glibc`
        * depends on `libcom_err`
    * `libss-devel`, since `libss` is our API

* `efibootmgr`
    * `efibootmgr`, pulled in by the depsolver

* `efivar`
    * `efivar`, preventing a split
        * depends on `efivar-libs`
        * depends on `glibc`
        * depends on `popt`
    * `efivar-devel`, since `efivar-libs` is our API
    * `efivar-libs`, pulled in by the depsolver

* `elfutils`
    * `elfutils`, pulled in by the depsolver
    * `elfutils-default-yama-scope`, pulled in by the depsolver
    * `elfutils-devel`, since `elfutils` is our API
    * `elfutils-devel-static`, since `elfutils` is our API
    * `elfutils-libelf`, pulled in by the depsolver
    * `elfutils-libelf-devel`, since `elfutils-libelf` is our API
    * `elfutils-libelf-devel-static`, since `elfutils-libelf` is our API
    * `elfutils-libs`, pulled in by the depsolver

* `emacs`
    * ~~`emacs`~~, we don't want to ship Emacs
    * ~~`emacs-common`~~, we don't want to ship Emacs
    * `emacs-filesystem`, pulled in by the depsolver
    * ~~`emacs-nox`~~, we don't want to ship Emacs
    * ~~`emacs-terminal`~~, we don't want to ship Emacs

* `expat`
    * `expat`, pulled in by the depsolver
    * `expat-devel`, since `expat` is our API
    * `expat-static`, since `expat` is our API

* `fedora-logos`
    * `fedora-logos`, pulled in by the depsolver
    * `fedora-logos-httpd`, preventing a split

* `fedora-modular-release`
    * `fedora-modular-release`, pulled in by the depsolver

* `fedora-rpm-macros`
    * `fedora-rpm-macros`, pulled in by the depsolver

* `fedpkg-minimal`
    * `fedpkg-minimal`, pulled in by the depsolver

* `file`
    * `file`, pulled in by the depsolver
    * `file-devel`, since `file-libs` is our API
    * `file-libs`, pulled in by the depsolver
    * ~~`python-magic`~~, requires `python2`, deliberately unsupported
        * required by `alot`
        * required by `kimchi`
        * required by `lekhonee`
        * required by `python-bugzilla`
        * required by `python2-jira`
        * required by `retrace-server`
        * required by `s3cmd`
        * required by `soscleaner`
    * ~~`python3-magic`~~, requires `python3`, deliberately unsupported
        * required by `diffoscope`
        * required by `python3-bugzilla`
        * required by `python3-jira`
        * required by `rpmlint`

* `filesystem`
    * `filesystem`, pulled in by the depsolver

* `findutils`
    * `findutils`, pulled in by the depsolver

* `flex`
    * `flex`, pulled in by the depsolver
    * `flex-devel`, since `flex` is our API
    * `flex-doc`, preventing a split

* `fpc-srpm-macros`
    * `fpc-srpm-macros`, pulled in by the depsolver

* `freetype`
    * `freetype`, pulled in by the depsolver
    * ~~`freetype-demos`~~, requires `libX11`, deliberately unsupported
        * not required by anything
    * `freetype-devel`, since `freetype` is our API

* `fuse`
    * `fuse`, preventing a split
        * depends on `glibc`
        * depends on `which`
    * `fuse-devel`, since `fuse-libs` is our API
    * `fuse-libs`, pulled in by the depsolver

* `gawk`
    * `gawk`, pulled in by the depsolver
    * `gawk-devel`, since `gawk` is our API
    * `gawk-doc`, preventing a split

* `gc`
    * `gc`, pulled in by the depsolver
    * `gc-devel`, since `gc` is our API

* `gcc`
    * `cpp`, pulled in by the depsolver
    * `gcc`, pulled in by the depsoolver
    * `gcc-c++`, pulled in by the depsolver
    * `gcc-gdb-plugin`¸ preventing a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `gcc-gfortran`, preventng a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libgfortran`
        * depends on `libmpc`
        * depends on `libquadmath`
        * depends on `libquadmath-devel`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-gnat`, preventing a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libgnat`
        * depends on `libgnat-devel`
        * depends on `libmpc`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-go`, preventing a split
        * depends on `gcc`
        * depends on `glbc`
        * depends on `gmp`
        * depends on `libgo`
        * depends on `libgo-devel`
        * depends on `libmpc`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-objc++`, preventing a split
        * depends on `gcc-c++`
        * depends on `gcc-objc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libmpc`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-objc`, preventing a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libmpc`
        * depends on `libobjc`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-offload-nvptx`, preventing a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libgcc`
        * depends on `libgomp-offload-nvptx`
        * depends on `libmpc`
        * depends on `libstdc++`
        * depends on `mpfr`
        * depends on `zlib`
    * `gcc-plugin-devel`, since `gcc` is our API
    * `libasan`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `libasan-static`, since `libasan` is our API
    * `libatomic`, preventing a split
        * depends on `glibc`
    * `libatomic-static`, since `libatomic` is our API
    * `libcilkrts`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `libcilkrts-static`, since `libcilkrts` is our API
    * `libgcc`, pulled in by the depsolver
    * `libgccjit`, preventing a split
        * depends on `gcc`
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libmpc`
        * depends on `mpfr`
        * depends on `zlib`
    * `libgccjit-devel`, since `libgccjit` is our API
    * `libgfortran`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libquadmath`
    * `libgfortran-static`, since `libgfortran` is our API
    * `libgnat`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
    * `libgnat-devel`, since `libgnat` is our API
    * `libgnat-static`, since `libgnat` is our API
    * `libgo`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
    * `libgo-devel`, since `libgo` is our API
    * `libgo-static`, since `libgo` is our API
    * `libgomp`, pulled in by the depsolver
    * `libgomp-offload-nvptx`, preventing a split
        * depends on `glibc`
        * depends on `libgomp`
    * `libitm`, preventing a split
        * depends on `glibc`
    * `libitm-devel`, since `libitm` is our API
    * `libitm-static`, since `libitm` is our API
    * `liblsan`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `liblsan-static`, since `liblsan` is our API
    * `libmpx`, preventing a split
        * depends on `glibc`
    * `libmpx-static`, since `libmpx` is our API
    * `libobjc`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
    * `libquadmath`, preventing a split
        * depends on `glibc`
    * `libquadmath-devel`, since `libquadmath` is our API
    * `libquadmath-static`, since `libquadmath` is our API
    * `libstdc++`, pulled in by the depsolver
    * `libstdc++-devel`, pulled in by the depsolver
    * `libstdc++-docs`, preventing a split
    * `libstdc++-static`, since `libstdc++` is our API
    * `libtsan`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `libtsan-static`, since `libtsan` is our API
    * `libubsan`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `liubsan-static`, since `libubsan` is our API

* `gdb`
    * `gdb`, preventing a split
        * depends on `gdb-headless`
    * `gdb-doc`, preventing a split
    * `gdb-gdbserver`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libselinux`
        * depends on `libstdc++`
    * `gdb-headless`, pulled in by the depsolver

* `gdbm`
    * `gdbm`, pulled in by the depsolver
    * `gdbm-devel`, since `gdbm` is our API

* `gettext`
    * ~~`emacs-gettext`~~, requires `emacs`, deliberately unsupported
        * not required by anything
    * `gettext`, pulled in by the depsolver
    * `gettext-common-devel`, since `gettext` is our API
    * `gettext-devel`, since `gettext` is our API
    * `gettext-libs`, pulled in by the depsolver
    * ~~`msghack`~~, requires `python3`, deliberately unsupported
        * not required by anything

* `ghc-srpm-macros`
    * `ghc-srpm-macros`, pulled in by the depsolver

* `glib2`
    * `glib2`, pulled in by the depsolver
    * ~~`glib2-devel`~~, **temporarily removed**, requires `perl` and `python3`
    * `glib2-doc`, preventing a split
        * depends on `glib2`
    * ~~`glib2-fam`~~, requires `gamin`
    * ~~`glib2-static`~~, **temporarily removed**, requires `glib2-devel`
    * `glib2-tests`, preventing a split
        * depends on `glib2`
        * depends on `glibc`
        * depends on `libffi`
        * depends on `libgcc`
        * depends on `libmount`
        * depends on `libselinux`
        * depends on `libstdc++`
        * depends on `pcre`
        * depends on `zlib`

* `glibc`
    * `glibc`, pulled in by the depsolver
    * `glibc-all-langpacks`, preventing a split
    * ~~`glibc-benchtests`~~, requires `python2`
    * `glibc-common`, pulled in by the depsolver
    * `glibc-devel`, pulled in by the depsolver
    * `glibc-headers`, pulled in by the depsolver
    * `glibc-langpack-aa`, preventing a split
    * `glibc-langpack-af`, preventing a split
    * `glibc-langpack-ak`, preventing a split
    * `glibc-langpack-am`, preventing a split
    * `glibc-langpack-an`, preventing a split
    * `glibc-langpack-anp`, preventing a split
    * `glibc-langpack-ar`, preventing a split
    * `glibc-langpack-as`, preventing a split
    * `glibc-langpack-ast`, preventing a split
    * `glibc-langpack-ayc`, preventing a split
    * `glibc-langpack-az`, preventing a split
    * `glibc-langpack-be`, preventing a split
    * `glibc-langpack-bem`, preventing a split
    * `glibc-langpack-ber`, preventing a split
    * `glibc-langpack-bg`, preventing a split
    * `glibc-langpack-bhb`, preventing a split
    * `glibc-langpack-bho`, preventing a split
    * `glibc-langpack-bn`, preventing a split
    * `glibc-langpack-bo`, preventing a split
    * `glibc-langpack-br`, preventing a split
    * `glibc-langpack-brx`, preventing a split
    * `glibc-langpack-bs`, preventing a split
    * `glibc-langpack-byn`, preventing a split
    * `glibc-langpack-ca`, preventing a split
    * `glibc-langpack-ce`, preventing a split
    * `glibc-langpack-chr`, preventing a split
    * `glibc-langpack-cmn`, preventing a split
    * `glibc-langpack-crh`, preventing a split
    * `glibc-langpack-cs`, preventing a split
    * `glibc-langpack-csb`, preventing a split
    * `glibc-langpack-cv`, preventing a split
    * `glibc-langpack-cy`, preventing a split
    * `glibc-langpack-da`, preventing a split
    * `glibc-langpack-de`, preventing a split
    * `glibc-langpack-doi`, preventing a split
    * `glibc-langpack-dv`, preventing a split
    * `glibc-langpack-dz`, preventing a split
    * `glibc-langpack-el`, preventing a split
    * `glibc-langpack-en`, preventing a split
    * `glibc-langpack-eo`, preventing a split
    * `glibc-langpack-es`, preventing a split
    * `glibc-langpack-et`, preventing a split
    * `glibc-langpack-eu`, preventing a split
    * `glibc-langpack-fa`, preventing a split
    * `glibc-langpack-ff`, preventing a split
    * `glibc-langpack-fi`, preventing a split
    * `glibc-langpack-fil`, preventing a split
    * `glibc-langpack-fo`, preventing a split
    * `glibc-langpack-fr`, preventing a split
    * `glibc-langpack-fur`, preventing a split
    * `glibc-langpack-fy`, preventing a split
    * `glibc-langpack-ga`, preventing a split
    * `glibc-langpack-gd`, preventing a split
    * `glibc-langpack-gez`, preventing a split
    * `glibc-langpack-gl`, preventing a split
    * `glibc-langpack-gu`, preventing a split
    * `glibc-langpack-gv`, preventing a split
    * `glibc-langpack-ha`, preventing a split
    * `glibc-langpack-hak`, preventing a split
    * `glibc-langpack-he`, preventing a split
    * `glibc-langpack-hi`, preventing a split
    * `glibc-langpack-hne`, preventing a split
    * `glibc-langpack-hr`, preventing a split
    * `glibc-langpack-hsb`, preventing a split
    * `glibc-langpack-ht`, preventing a split
    * `glibc-langpack-hu`, preventing a split
    * `glibc-langpack-hy`, preventing a split
    * `glibc-langpack-ia`, preventing a split
    * `glibc-langpack-id`, preventing a split
    * `glibc-langpack-ig`, preventing a split
    * `glibc-langpack-ik`, preventing a split
    * `glibc-langpack-is`, preventing a split
    * `glibc-langpack-it`, preventing a split
    * `glibc-langpack-iu`, preventing a split
    * `glibc-langpack-ja`, preventing a split
    * `glibc-langpack-ka`, preventing a split
    * `glibc-langpack-kk`, preventing a split
    * `glibc-langpack-kl`, preventing a split
    * `glibc-langpack-km`, preventing a split
    * `glibc-langpack-kn`, preventing a split
    * `glibc-langpack-ko`, preventing a split
    * `glibc-langpack-kok`, preventing a split
    * `glibc-langpack-ks`, preventing a split
    * `glibc-langpack-ku`, preventing a split
    * `glibc-langpack-kw`, preventing a split
    * `glibc-langpack-ky`, preventing a split
    * `glibc-langpack-lb`, preventing a split
    * `glibc-langpack-lg`, preventing a split
    * `glibc-langpack-li`, preventing a split
    * `glibc-langpack-lij`, preventing a split
    * `glibc-langpack-ln`, preventing a split
    * `glibc-langpack-lo`, preventing a split
    * `glibc-langpack-lt`, preventing a split
    * `glibc-langpack-lv`, preventing a split
    * `glibc-langpack-lzh`, preventing a split
    * `glibc-langpack-mag`, preventing a split
    * `glibc-langpack-mai`, preventing a split
    * `glibc-langpack-mg`, preventing a split
    * `glibc-langpack-mhr`, preventing a split
    * `glibc-langpack-mi`, preventing a split
    * `glibc-langpack-mk`, preventing a split
    * `glibc-langpack-ml`, preventing a split
    * `glibc-langpack-mn`, preventing a split
    * `glibc-langpack-mni`, preventing a split
    * `glibc-langpack-mr`, preventing a split
    * `glibc-langpack-ms`, preventing a split
    * `glibc-langpack-mt`, preventing a split
    * `glibc-langpack-my`, preventing a split
    * `glibc-langpack-nan`, preventing a split
    * `glibc-langpack-nb`, preventing a split
    * `glibc-langpack-nds`, preventing a split
    * `glibc-langpack-ne`, preventing a split
    * `glibc-langpack-nhn`, preventing a split
    * `glibc-langpack-niu`, preventing a split
    * `glibc-langpack-nl`, preventing a split
    * `glibc-langpack-nn`, preventing a split
    * `glibc-langpack-nr`, preventing a split
    * `glibc-langpack-nso`, preventing a split
    * `glibc-langpack-oc`, preventing a split
    * `glibc-langpack-om`, preventing a split
    * `glibc-langpack-or`, preventing a split
    * `glibc-langpack-os`, preventing a split
    * `glibc-langpack-pa`, preventing a split
    * `glibc-langpack-pap`, preventing a split
    * `glibc-langpack-pl`, preventing a split
    * `glibc-langpack-ps`, preventing a split
    * `glibc-langpack-pt`, preventing a split
    * `glibc-langpack-quz`, preventing a split
    * `glibc-langpack-raj`, preventing a split
    * `glibc-langpack-ro`, preventing a split
    * `glibc-langpack-ru`, preventing a split
    * `glibc-langpack-rw`, preventing a split
    * `glibc-langpack-sa`, preventing a split
    * `glibc-langpack-sat`, preventing a split
    * `glibc-langpack-sc`, preventing a split
    * `glibc-langpack-sd`, preventing a split
    * `glibc-langpack-se`, preventing a split
    * `glibc-langpack-sgs`, preventing a split
    * `glibc-langpack-shs`, preventing a split
    * `glibc-langpack-si`, preventing a split
    * `glibc-langpack-sid`, preventing a split
    * `glibc-langpack-sk`, preventing a split
    * `glibc-langpack-sl`, preventing a split
    * `glibc-langpack-so`, preventing a split
    * `glibc-langpack-sq`, preventing a split
    * `glibc-langpack-sr`, preventing a split
    * `glibc-langpack-ss`, preventing a split
    * `glibc-langpack-st`, preventing a split
    * `glibc-langpack-sv`, preventing a split
    * `glibc-langpack-sw`, preventing a split
    * `glibc-langpack-szl`, preventing a split
    * `glibc-langpack-ta`, preventing a split
    * `glibc-langpack-tcy`, preventing a split
    * `glibc-langpack-te`, preventing a split
    * `glibc-langpack-tg`, preventing a split
    * `glibc-langpack-th`, preventing a split
    * `glibc-langpack-the`, preventing a split
    * `glibc-langpack-ti`, preventing a split
    * `glibc-langpack-tig`, preventing a split
    * `glibc-langpack-tk`, preventing a split
    * `glibc-langpack-tl`, preventing a split
    * `glibc-langpack-tn`, preventing a split
    * `glibc-langpack-tr`, preventing a split
    * `glibc-langpack-ts`, preventing a split
    * `glibc-langpack-tt`, preventing a split
    * `glibc-langpack-ug`, preventing a split
    * `glibc-langpack-uk`, preventing a split
    * `glibc-langpack-unm`, preventing a split
    * `glibc-langpack-ur`, preventing a split
    * `glibc-langpack-uz`, preventing a split
    * `glibc-langpack-ve`, preventing a split
    * `glibc-langpack-vi`, preventing a split
    * `glibc-langpack-wa`, preventing a split
    * `glibc-langpack-wae`, preventing a split
    * `glibc-langpack-wal`, preventing a split
    * `glibc-langpack-wo`, preventing a split
    * `glibc-langpack-xh`, preventing a split
    * `glibc-langpack-yi`, preventing a split
    * `glibc-langpack-yo`, preventing a split
    * `glibc-langpack-yue`, preventing a split
    * `glibc-langpack-zh`, preventing a split
    * `glibc-langpack-zu`, preventing a split
    * `glibc-locale-source`, preventing a split
        * depends on `glibc`
        * depends on `glibc-common`
    * `glibc-minimal-langpack`, pulled in by the depsolver
    * `glibc-nss-devel`, since `nss_db`, `nss_hesiod` and `nss_nis` are our API
    * `glibc-static`, since `glibc` is our API
    * ~~`glibc-utils`~~, requires `perl` and `gd`
    * `libcrypt`, pulled in by the depsolver
    * `libcrypt-nss`, preventing a split
        * depends on `glibc`
        * depends on `nss-softokn-freebl`
    * `nscd`, preventing a split
        * depends on `audit-libs`
        * depends on `glibc`
        * depends on `libcap`
        * depends on `libselinux`
        * depends on `shadow-utils`
    * `nss_db`, preventing a split
        * depends on `glibc`
    * `nss_hesiod`, preventing a split
        * depends on `glibc`
    * `nss_nis`, preventing a split
        * depends on `glibc`

* `gmp`
    * `gmp`, pulled in by the depsolver
    * `gmp-c++`, preventing a split
        * depends on `glibc`
        * depends on `gmp`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `gmp-devel`, since `gmp` is our API
    * `gmp-static`, since `gmp` is our API

* `gnat-srpm-macros`
    * `gnat-srpm-macros`, pulled in by the depsolver

* `gnupg2`
    * `gnupg2`, pulled in by the depsolver
    * ~~`gnupg2-smime`~~, requires `libusbx`

* `gnutls`
    * `gnutls`, pulled in by the depsolver
    * `gnutls-c++`, preventing a split
        * depends on `glibc`
        * depends on `gnutls`
        * depends on `libgcc`
        * depends on `libstdc++`
    * ~~`gnutls-dane`~~, requires `unbound-libs`
    * ~~`gnutls-devel`~~, **temporarily removed**, requires `gnutls-dane`
    * ~~`gnutls-guile`~~, requires `compat-guile18`
    * ~~`gnutls-utils`~~ requires `gnutls-dane`

* `go-srpm-macros`
    * `go-srpm-macros`, pulled in by the depsolver

* `gobject-introspection`
    * `gobject-introspection`, pulled in by the depsolver
    * ~~`gobject-introspection-devel`~~, **temporarily removed**, requires `python2`

* `gpgme`
    * `gpgme`, pulled in by the depsolver
    * `gpgme-devel`, since `gpgme` is our API
    * `gpgmepp`, preventing a split
        * depends on `glibc`
        * depends on `gpgme`
        * depends on `libassuan`
        * depends on `libgcc`
        * depends on `libgpg-error`
        * depends on `libstdc++`
    * `gpgmepp-devel`, since `gpgmepp` is our API
    * ~~`python2-gpg`~~, requires `python2`
    * ~~`python3-gpg`~~, requires `python3`
    * ~~`qgpgme`~~, requires `qt5-qtbase`
    * ~~`qgpgme-devel`~~, requires `qgpgme`

* `grep`
    * `grep`, pulled in by the depsolver

* `grub2`
    * `grub2`, pulled in by the depsolver
    * `grub2-efi`, pulled in by the depsolver
    * `grub2-efi-modules`, preventing a split
        * depends on `grub2-tools`
    * `grub2-starfield-theme`, preventing a split
        * depends on `fedora-logos`
    * `grub2-tools`, pulled in by the depsolver

* `guile`
    * `guile`, pulled in by the depsolver
    * `guile-devel`, since `guile` is our API

* `gzip`
    * `gzip`, pulled in by the depsolver

* `iptables`
    * ~~`iptables`~~, requires `libnetfilter_conntrack` and `libnfnetlink`
    * ~~`iptables-compat`~~, requires `iptables`, `libmnl`, `libnetfilter_conntrack`, `libnfnetlink` and `libnftnl`
    * ~~`iptables-devel`~~, **temporarily removed**, requires `iptables`
    * `iptables-libs`, pulled in by the depsolver
    * ~~`iptables-services`~~, requires `iptables`
    * ~~`iptables-utils`~~, requires `iptables` and `libnfnetlinks`

* `isl`
    * `isl`, pulled in by the depsolver
    * `isl-devel`, since `isl` is our API

* `kernel`
    * `kernel`, preventing a split
        * depends on `kernel-core`
        * depends on `kernel-modules`
    * `kernel-core`, pulled in by the depsolver
    * `kernel-cross-headers`, preventing a split
    * ~~`kernel-devel`~~, requires `perl`
    * `kernel-headers`, pulled in by the depsolver
    * `kernel-modules`, preventing a split
        * depends on `kernel-core`
    * `kernel-modules-extra`, preventing a split
        * depends on `kernel-core`
        * depends on `kernel-modules`
    * ~~`kernel-tools`~~, requires `pciutils-libs`
    * ~~`kernel-tools-libs`~~, pointless without `kernel-tools-libs-devel`
    * ~~`kernel-tools-libs-devel`~~, requires `kernel-tools`
    * ~~`perf`~~, requires `numactl-libs`, `perl`, `python2` and `slang`
    * ~~`python-perf`~~, requires `python2`

* `keyutils`
    * `keyutils`, preventing a split
        * depends on `bash`
        * depends on `glibc`
        * depends on `keyutils-libs`
    * `keyutils-libs`, pulled in by the depsolver
    * `keyutils-libs-devel`, since `keyutils-libs` is our API

* `kmod`
    * `kmod`, pulled in by the depsolver
    * `kmod-devel`, since `kmod-libs` is our API
    * `kmod-libs`, pulled in by the depsolver

* `krb5`
    * `krb5-devel`, since `krb5-libs` is our API
    * `krb5-libs`, pulled in by the depsolver
    * `krb5-pkinit`, preventing a split
        * depends on `glibc`
        * depends on `keyutils-libs`
        * depends on `krb5-libs`
        * depends on `libcom_err`
        * depends on `openssl-libs`
    * ~~`krb5-server`~~, requires `logrotate` and `words`
    * ~~`krb5-server-ldap`~~, requires `krb5-server`
    * `krb5-workstation`, preventing a split
        * depends on `bash`
        * depends on `glibc`
        * depends on `keyutils-libs`
        * depends on `krb5-libs`
        * depends on `libcom_err`
        * depends on `libkadm5`
        * depends on `libselinux`
        * depends on `libss`
        * depends on `pam`
    * `libkadm5`, preventing a split
        * depends on `glibc`
        * depends on `keyutils-libs`
        * depends on `krb5-libs`
        * depends on `libcom_err`

* `libarchive`
    * `bsdcat`, preventing a split
        * depends on `bzip2-libs`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libarchive`
        * depends on `libxml2`
        * depends on `lz4`
        * depends on `lzo`
        * depends on `openssl-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `bsdcpio`, preventing a split
        * depends on `bzip2-libs`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libarchive`
        * depends on `libxml2`
        * depends on `lz4`
        * depends on `lzo`
        * depends on `openssl-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `bsdtar`, preventing a split
        * depends on `bzip2-libs`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libarchive`
        * depends on `libxml2`
        * depends on `lz4`
        * depends on `lzo`
        * depends on `openssl-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `libarchive`, pulled in by the depsolver
    * `libarchive-devel`, since `libarchive` is our API

* `libassuan`
    * `libassuan`, pulled in by the depsolver
    * `libassuan-devel`, since `libassuan` is our API

* `libatomic_ops`
    * `libatomic_ops`, pulled in by the depsolver
    * `libatomic_ops-devel`, since `libatomic_ops` is our API
    * `libatomic_ops-static`, since `libatomic_ops` is our API

* `libcap`
    * `libcap`, pulled in by the depsolver
    * `libcap-devel`, since `libcap` is our API
    * `libcap-static`, since `libcap` is our API

* `libcap-ng`
    * `libcap-ng`, pulled in by the depsolver
    * `libcap-ng-devel`, since `libcap-ng` is our API
    * ~~`libcap-ng-python`~~, requires `python2`
    * ~~`libcap-ng-python3`~~, requires `python3`
    * `libcap-ng-utils`, preventing a split
        * depends on `glibc`
        * depends on `libcap-ng`

* `libcroco`
    * `libcroco`, pulled in by the depsolver
    * ~~`libcroco-devel`~~, **temporarily removed**, requires `glib2-devel`

* `libdb`
    * `libdb`, pulled in by the depsolver
    * `libdb-cxx`, preventing a split
        * depends on `glibc`
        * depends on `libdb`
        * depends on `libgcc`
        * depends on `libstdc++`
    * `libdb-cxx-devel`, since `libdb-cxx` is our API
    * `libdb-devel`, since `libdb` is our API
    * `libdb-devel-doc`, preventing a split
        * depends on `libdb`
        * depends on `libdb-devel`
    * `libdb-devel-static`, since `libdb` is our API
    * `libdb-java`, preventing a split
        * depends on `glibc`
        * depends on `libdb`
    * `libdb-java-devel`, since `libdb` is our API
    * `libdb-sql`, preventing a split
        * depends on `glibc`
        * depends on `libdb`
    * `libdb-sql-devel`, since `libdb-sql` is our API
    * `libdb-tcl`, preventing a split
        * depends on `glibc`
        * depends on `libdb`
    * `libdb-tcl-devel`, since `libdb-tcl` is our API
    * `libdb-utils`, pulled in by the depsolver

* `libdnf`
    * `libdnf`, pulled in by the depsolver
    * ~~`libdnf-devel`~~, **temporarily removed**, requires `rpm-ostree`
    * ~~`python2-hawkey`~~, requires `python2`
    * ~~`python3-hawkey`~~, requires `python3`

* `libev`
    * `libev`, pulled in by the depsolver
    * `libev-devel`, since `libev` is our API
    * `libev-libevent-devel`, since `libev` is our API

* `libffi`
    * `libffi`, pulled in by the depsolver
    * `libffi-devel`, since `libffi` is our API

* `libgcrypt`
    * `libgcrypt`, pulled in by the depsolver
    * `libgcrypt-devel`, since `libgcrypt` is our API

* `libgpg-error`
    * `libgpg-error`, pulled in by the depsolver
    * `libgpg-error-devel`, since `libgpg-error` is our API

* `libidn`
    * `libidn`, pulled in by the depsolver
    * `libidn-devel`, since `libidn` is our API
    * ~~`libidn-java`~~, requires `java-1.8.0-openjdk` and `javapackages-tools`, deliberately unsupported
        * required by `jxmpp-stringprep-libidn`
    * ~~`libidn-javadoc`~~, requires `javapackages-tools`, deliberately unsupported

* `libidn2`
    * `libidn2`, pulled in by the depsolver
    * `libidn2-devel`, since `libidn2` is our API

* `libipt`
    * `libipt`, pulled in by the depsolver
    * `libipt-devel`, since `libipt` is our API

* `libksba`
    * `libksba`, pulled in by the depsolver
    * `libksba-devel`, since `libksba` is our API

* `libmetalink`
    * `libmetalink`, pulled in by the depsolver
    * `libmetalink-devel`, since `libmetalink` is our API

* `libmpc`
    * `compat-libmpc`, preventing a split
        * depends on `glibc`
        * depends on `gmp`
        * depends on `mpfr`
    * `libmpc`, pulled in by the depsolver
    * `libmpc-devel`, since `libmpc` is our API

* `libpcap`
    * `libpcap`, pulled in by the depsolver
    * `libpcap-devel`, since `libpcap` is our API

* `libpeas`
    * `libpeas`, pulled in by the depsolver
    * ~~`libpeas-devel`~~, **temporarily removed**, requires `atk`, `cairo`, `cairo-gobject`, `gdk-pixbuf2`, `gtk3`, `gtk3-devel` and `pango`
    * ~~`libpeas-gtk`~~, requires `atk`, `cairo`, `cairo-gobject`, `gdk-pixbuf2`, `gtk3` and `pango`
    * ~~`libpeas-loader-python`~~, requires `python2`
    * ~~`libpeas-loader-python3`~~, requires `python3`

* `libpng`
    * `libpng`, pulled in by the depsolver
    * `libpng-devel`, since `libpng` is our API
    * `libpng-static`, since `libpng` is our API
    * `libpng-tools`, preventing a split
        * depends on `glibc`
        * depends on `libpng`
        * depends on `zlib`

* `libpsl`
    * `libpsl`, pulled in by the depsolver
    * `libpsl-devel`, since `libpsl` is our API
    * `psl`, preventing a split
        * depends on `glibc`
        * depends on `libpsl`
    * ~~`psl-make-dafsa`~~, requires `python3`, deliberately unsupported

* `libpwquality`
    * `libpwquality`, pulled in by the depsolver
    * `libpwquality-devel`, since `libpwquality` is our API
    * ~~`python-pwquality`~~, requires `python2`
    * ~~`python3-pwquality`~~, requires `python3`

* `librepo`
    * `librepo`, pulled in by the depsolver
    * ~~`librepo-devel`~~, **temporarily removed**, requires `compat-openssl10-devel`, `glib2-devel`
    * ~~`python2-librepo`~~, requires `python2`
    * ~~`python3-librepo`~~, requires `python3` (`system-python`)

* `libseccomp`
    * `libseccomp`, pulled in by the depsolver
    * `libseccomp-devel`, since `libseccomp` is our API
    * `libseccomp-static`, since `libseccomp` is our API

* `libselinux`
    * `libselinux`, pulled in by the depsolver
    * `libselinux-devel`, since `libselinux` is our API
    * ~~`libselinux-python`~~, requires `python2`
    * ~~`libselinux-python3`~~, requires `python3`
    * ~~`libselinux-ruby`~~, let's just not provide ruby bindings here
    * `libselinux-static`, since `libselinux` is our API
    * `libselinux-utils`, preventing a split
        * depends on `glibc`
        * depends on `libselinux`
        * depends on `libsepol`

* `libsemanage`
    * `libsemanage`, pulled in by the depsolver
    * `libsemanage-devel`, since `libsemanage` is our API
    * ~~`libsemanage-python`~~, requires `python2`
    * ~~`libsemanage-python3`~~, requires `python3`
    * `libsemanage-static`, since `libsemanage` is our API

* `libsepol`
    * `libsepol`, pulled in by the depsolver
    * `libsepol-devel`, since `libsepol` is our API
    * `libsepol-static`, since `libsepol` is our API

* `libsigsegv`
    * `libsigsegv`, pulled in by the depsolver
    * `libsigsegv-devel`, since `libsigsegv` is our API
    * `libsigsegv-static`, since `libsigsegv` is our API

* `libsolv`
    * `libsolv`, pulled in by the depsolver
    * `libsolv-demo`, preventing a split
        * depends on `curl`
        * depends on `glibc`
        * depends on `gnupg2`
        * depends on `libsolv`
    * `libsolv-devel`, since `libsolv` is our API
    * `libsolv-tools`, preventing a split
        * depends on `bash`
        * depends on `bzip2`
        * depends on `coreutils`
        * depends on `findutils`
        * depends on `glibc`
        * depends on `gzip`
        * depends on `libsolv`
        * depends on `xz`
        * depends on `xz-lzma-compat`
    * ~~`perl-solv`~~, requires `perl`
    * ~~`python2-solv`~~, requires `python2`
    * ~~`python3-solv`~~, requires `python3`
    * ~~`ruby-solv`~~, let's just not have a ruby binding here

* `libssh2`
    * `libssh2`, pulled in by the depsolver
    * ~~`libssh2-devel`~~, **temporarily removed**, requires `compat-openssl10-devel`
    * `libssh2-docs`, preventing a split
        * depends on `libssh2`

* `libtasn1`
    * `libtasn1`, pulled in by the depsolver
    * `libtasn1-devel`, since `libtasn1` is our API
    * `libtasn1-tools`, preventing a split
        * depends on `glibc`
        * depends on `libtasn1`

* `libtool`
    * ~~`libtool`~~, requires `autoconf` and `automake`
    * `libtool-ltdl`, pulled in by the depsolver
    * `libtool-ltdl-devel`, since `libtool-ltdl` is our API

* `libunistring`
    * `libunistring`, pulled in by the depsolver
    * `libunistring-devel`, since `libunistring` is our API

* `libutempter`
    * `libutempter`, pulled in by the depsolver
    * `libutempter-devel` since `libutempter` is our API

* `libverto`
    * `libverto`, pulled in by the depsolver
    * `libverto-devel`, since `libverto` is our API
    * `libverto-glib`, preventing a split
        * depends on `glib2`
        * depends on `glibc`
        * depends on `libverto`
    * `libverto-glib-devel`, since `libverto-glib` is our API
    * `libverto-libev`, preventing a split
        * depends on `glibc`
        * depends on `libev`
        * depends on `libverto`
    * `libverto-libev-devel`, since `libverto-libev` is our API
    * ~~`libverto-libevent`~~, requires `libevent`
    * ~~`libverto-libevent-devel`~~, pointless without `libverto-libevent`
    * ~~`libverto-tevent`~~, requires `libtalloc`, `libtevent`
    * ~~`libverto-tevent-devel`~~, pointless without `libverto-tevent`

* `libxml2`
    * `libxml2`, pulled in by the depsolver
    * `libxml2-devel`, since `libxml2` is our API
    * `libxml2-static`, since `libxml2` is our API
    * ~~`python-libxml2`~~, requires `python2`
    * ~~`python3-libxml2`~~, requires `python3` (`system-python`)

* `linux-firmware`
    * `iwl100-firmware-39.31.5.1`, firmware is our friend
    * `iwl1000-firmware-39.31.5.1`, firmware is our friend
    * `iwl105-firmware-18.168.6.1`, firmware is our friend
    * `iwl135-firmware-18.168.6.1`, firmware is our friend
    * `iwl2000-firmware-18.168.6.1`, firmware is our friend
    * `iwl2030-firmware-18.168.6.1`, firmware is our friend
    * `iwl3160-firmware-25.30.13.0`, firmware is our friend
    * `iwl3945-firmware-15.32.2.9`, firmware is our friend
    * `iwl4965-firmware-228.61.2.24`, firmware is our friend
    * `iwl5000-firmware-8.83.5.1_1`, firmware is our friend
    * `iwl5150-firmware-8.24.2.2`, firmware is our friend
    * `iwl6000-firmware-9.221.4.1`, firmware is our friend
    * `iwl6000g2a-firmware-18.168.6.1`, firmware is our friend
    * `iwl6000g2b-firmware-18.168.6.1`, firmware is our friend
    * `iwl6050-firmware-41.28.5.1`, firmware is our friend
    * `iwl7260-firmware-25.30.13.0`, firmware is our friend
    * `libertas-sd8686-firmware-20170213`, firmware is our friend
    * `libertas-sd8787-firmware-20170213`, firmware is our friend
    * `libertas-usb8388-firmware-20170213`, firmware is our friend
    * `libertas-usb8388-olpc-firmware-20170213`, firmware is our friend
    * `linux-firmware`, pulled in by the depsolver

* `lua`
    * `lua`, preventing a split
        * depends on `glibc`
        * depends on `lua-libs`
        * depends on `ncurses-libs`
        * depends on `readline`
    * `lua-devel`, since `lua` is our API
    * `lua-libs`, pulled in by the depsolver
    * `lua-static`, since `lua` is our API

* `lvm2`
    * ~~`cmirror`~~, requires `corosync`, `corosynclib`, `resource-agents`
    * ~~`cmirror-standalone`~~, requires `cmirror`
    * `device-mapper`, pulled in by the depsolver
    * `device-mapper-devel`, since `device-mapper` is our API
    * `device-mapper-event`, preventing a split
        * depends on `device-mapper`
        * depends on `device-mapper-event-libs`
        * depends on `device-mapper-libs`
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `systemd-libs`
    * `device-mapper-event-devel`, since `device-mapper-event` is our API
    * `device-mapper-event-libs`, preventing a split
        * depends on `device-mapper-libs`
        * depends on `glibc`
        * depends on `systemd-libs`
    * `device-mapper-libs`, pulled in by the depsolver
    * ~~`lvm2`~~, requires `device-mapper-persistent-data`
    * ~~`lvm2-cluster`~~, requires `corosync`, `corosynclib`, `dlm`, `dlm-lib` and `resource-agents`
    * ~~`lvm2-cluster-standalone`~~, requires `lvm2-cluster`
    * ~~`lvm2-dbusd`~~, requires `lvm2`, `python3`
    * ~~`lvm2-devel`~~, requires `lvm2`; we could drop `lvm2-libs`, perhaps
    * `lvm2-libs`, preventing a split
        * depends on `device-mapper-event`
        * depends on `device-mapper-event-libs`
        * depends on `device-mapper-libs`
        * depends on `glibc`
        * depends on `libblkid`
        * depends on `systemd-libs`
    * ~~`lvm2-lockd`~~, requires `dlm`, `dlm-lib`, `lvm2` and `sanlock-lib`
    * ~~`lvm2-python-libs`~~, requires `python2`
    * ~~`lvm2-python3-libs`~~, requires `python3`

* `lz4`
    * `lz4`, pulled in by the depsolver
    * `lz4-devel`, since `lz4` is our API
    * `lz4-static`, since `lz4` is our API

* `lzo`
    * `lzo`, pulled in by the depsolver
    * `lzo-devel`, since `lzo` is our API
    * `lzo-minilzo`, preventing a split
        * depends on `glibc`

* `m4`
    * `m4`, pulled in by the depsolver

* `mactel-boot`
    * `mactel-boot`, pulled in by the depsolver

* `make`
    * `make`, pulled in by the depsolver
    * `make-devel`, since `make` is our API

* `microdnf`
    * `microdnf`, pulled in by the depsolver

* `mokutil`
    * `mokutil`, pulled in by the depsolver

* `mpfr`
    * `mpfr`, pulled in by the depsolver
    * `mpfr-devel`, since `mpfr` is our API

* `mtools`
    * `mtools`, pulled in by the depsolver

* `ncurses`
    * `ncurses`, pulled in by the depsolver
    * `ncurses-base`, pulled in by the depsolver
    * `ncurses-c++-libs`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
        * depends on `ncurses-libs`
    * `ncurses-compat-libs`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
        * depends on `ncurses-base`
    * `ncurses-devel`, since `ncurses` is our API
    * `ncurses-libs`, pulled in by the depsolver
    * `ncurses-static`, since `ncurses` is our API
    * `ncurses-term`, preventing a split
        * depends on `ncurses-base`

* `nettle`
    * `nettle`, pulled in by the depsolver
    * `nettle-devel`, since `nettle` is our API

* `nghttp2`
    * `libnghttp2`, pulled in by the depsolver
    * `libnghttp2-devel`, since `libnghttp2` is our API
    * `nghttp2`, **possibly a temporary split**, requires `c-ares`

* `npth`
    * `npth`, pulled in by the depsolver
    * `npth-devel`, since `npth` is our API

* `nspr`
    * `nspr`, pulled in by the depsolver
    * `nspr-devel`, since `nspr` is our API

* `nss`
    * `nss`, pulled in by the depsolver
    * `nss-devel`, since `nss` is our API
    * `nss-pkcs11-devel`, since `nss` is our API
    * `nss-sysinit`, pulled in by the depsolver
    * `nss-tools`, pulled in by the depsolver

* `nss-pem`
    * `nss-pem` pulled in by the depsolver

* `nss-softokn`
    * `nss-softokn`, pulled in by the depsolver
    * `nss-softokn-devel`, since `nss-softokn` is our API
    * `nss-softokn-freebl`, pulled in by the depsolver
    * `nss-softokn-freebl-devel`, since `nss-softokn-freebl` is our API

* `nss-util`
    * `nss-util`, pulled in by the depsolver
    * `nss-util-devel`, since `nss-util` is our API

* `ocaml-srpm-macros`
    * `ocaml-srpm-macros`, pulled in by the depsolver

* `openldap`
    * `openldap`, pulled in by the depsolver
    * `openldap-clients`, preventing a split
        * depends on `cyrus-sasl-lib`
        * depends on `glibc`
        * depends on `openldap`
    * `openldap-devel`, since `openldap` is our API
    * ~~`openldap-servers`~~, requires `perl`, `tcp_wrappers-libs`

* `openssl`
    * `openssl`, preventing a split
        * depends on `bash`
        * depends on `coreutils`
        * depends on `glibc`
        * depends on `make`
        * depends on `openssl-libs`
        * depends on `zlib`
    * `openssl-devel`, since `openssl-libs` is our API
    * `openssl-libs`, pulled in by the depsolver
    * ~~`openssl-perl`~~, requires `perl`
    * `openssl-static`, since `openssl-libs` is our API

* `os-prober`
    * `os-prober`, pulled in by the depsolver

* `p11-kit`
    * `p11-kit`, pulled in by the depsolver
    * `p11-kit-devel`, since `p11-kit` is our API
    * `p11-kit-tools`, preventing a split
        * depends on `glibc`
        * depends on `libffi`
        * depends on `p11-kit`
    * `p11-kit-trust`, pulled in by the depsolver

* `pam`
    * `pam`, pulled in by the depsolver
    * `pam-devel`, since `pam` is our API

* `patch`
    * `patch`, pulled in by the depsolver

* `pcre`
    * `pcre`, pulled in by the depsolver
    * `pcre-cpp`, preventing a split
        * depends on `glibc`
        * depends on `libgcc`
        * depends on `libstdc++`
        * depends on `pcre`
    * `pcre-devel`, since `pcre` is our API
    * `pcre-doc`, preventing a split
    * `pcre-static`, since `pcre` is our API
    * `pcre-tools` preventing a split
        * depends on `glibc`
        * depends on `pcre`
        * depends on `pcre-utf16`
        * depends on `pcre-utf32`
        * depends on `readline`
    * `pcre-utf16`, preventing a split
        * depends on `glibc`
    * `pcre-utf32`, preventing a split
        * depends on `glibc`

* `perl-srpm-macros`
    * `perl-srpm-macros`, pulled in by the depsolver

* `pkgconf`
    * `libpkgconf`, pulled in by the depsolver
    * `libpkgconf-devel`, since `libpkgconf` is our API
    * `pkgconf`, pulled in by the depsolver
    * `pkgconf-m4`, pulled in by the depsolver
    * `pkgconf-pkg-config`, pulled in by the depsolver

* `popt`
    * `popt`, pulled in by the depsolver
    * `popt-devel`, since `popt` is our API
    * `popt-static`, since `popt` is our API

* `procps-ng`
    * `procps-ng`, pulled in by the depsolver
    * `procps-ng-devel`, since `procps-ng` is our API
    * `procps-ng-i18n`, preventing a split
        * depends on `procps-ng`

* `pth`
    * `pth`, preventing a split
        * depends on `glibc`
    * `pth-devel`, pulled in by the depsolver

* `publicsuffix-list`
    * `publicsuffix-list`, preventing a split
    * `publicsuffix-list-dafsa`, pulled in by the depsolver

* `pyparsing`
    * ~~`pyparsing`~~, requires `python2-pyparsing`
    * `pyparsing-doc`, preventing a split
    * ~~`python2-pyparsing`~~, requires `python2`
    * `python3-pyparsing`, pulled in by the depsolver

* `python-appdirs`
    * ~~`python2-appdirs`~~, requires `python2`
    * `python3-appidrs`, pulled in by the depsolver

* `python-packaging`
    * `python-packaging-doc`, preventing a split
    * ~~`python2-packaging`~~, requires `python2`
    * `python3-packaging`, pulled in by the depsolver

* `python-pip`
    * ~~`python2-pip`~~, requires `python2`
    * `python3-pip`, pulled in by the depsolver

* `python-rpm-macros`
    * `python-rpm-macros`, preventing a split
        * depends on `python-srpm-macros`
    * `python-srpm-macros`, pulled in by the depsolver
    * `python2-rpm-macros`, preventing a split
    * `python3-rpm-macros`, preventing a split

* `python-setuptools`
    * ~~`python2-setuptools`~~, requires `python2`
    * `python3-setuptools`, pulled in by the depsolver

* `python-six`
    * ~~`python2-six`~~, requires `python2`
    * `python3-six`, pulled in by the depsolver

* `python3`
    * `python3`, pulled in by the depsolver
    * ~~`python3-debug`~~, requires `libX11`, `tcl`, `tk`
    * `python3-devel`, since `python3` is our API
    * `python3-libs`, pulled in by the depsolver
    * ~~`python3-test`~~, requires `python3-tools`
    * ~~`python3-tkinter`~~, requires `libX11`, `tcl`, `tk`
    * ~~`python3-tools`~~, requires `python3-tkinter`
    * `system-python`, pulled in by the depsolver
    * `system-python-libs`, pulled in by the depsolver

* `qrencode`
    * `qrencode`, preventing a split
        * depends on `glibc`
        * depends on `libpng`
        * depends on `qrencode-libs`
    * `qrencode-devel`, since `qrencode-libs` is our API
    * `qrencode-libs`, pulled in by the depsolver

* `qt5`
    * ~~`qt5`~~, we don't want Qt5, deliberately unsupported
    * ~~`qt5-devel`~~, we don't want Qt5, deliberately unsupported
    * ~~`qt5-rpm-macros`~~, requires `cmake`, deliberately unsupported
    * `qt5-srpm-macros`, pulled in by the depsolver

* `readline`
    * `readline`, pulled in by the depsolver
    * `readline-devel`, since `readline` is our API
    * `readline-static`, since `readline` is our API

* `redhat-rpm-config`
    * ~~`kernel-rpm-macros`~~, requires `perl`
    * `redhat-rpm-config`, pulled in by the depsolver

* `rpm`
    * ~~`python2-rpm`~~, requires `python2`
    * ~~`python3-rpm`~~, requires `python3` (`system-python`)
    * `rpm`, pulled in by the depsolver
    * `rpm-apidocs`, preventing a split
    * `rpm-build`, pulled in by the depsolver
    * `rpm-build-libs`, pulled in by the depsolver
    * ~~`rpm-cron`~~, requires `crontabs` and `logrotate`
    * `rpm-devel`, since `rpm` is our API
    * `rpm-libs`, pulled in by the depsolver
    * `rpm-plugin-ima`, preventing a split
        * depends on `bzip2-libs`
        * depends on `elfutils-libelf`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libcap`
        * depends on `libdb`
        * depends on `lua-libs`
        * depends on `nss`
        * depends on `popt`
        * depends on `rpm-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `rpm-plugin-selinux`, pulled in by the depsolver
    * `rpm-plugin-syslog`, preventing a split
        * depends on `bzip2-libs`
        * depends on `elfutils-libelf`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libcap`
        * depends on `libdb`
        * depends on `lua-libs`
        * depends on `nss`
        * depends on `popt`
        * depends on `rpm-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `rpm-plugin-systemd-inhibit`, preventing a split
        * depends on `bzip2-libs`
        * depends on `dbus-libs`
        * depends on `elfutils-libelf`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libcap`
        * depends on `libdb`
        * depends on `lua-libs`
        * depends on `nss`
        * depends on `popt`
        * depends on `rpm-libs`
        * depends on `xz-libs`
        * depends on `zlib`
    * `rpm-sign`, preventing a split
        * depends on `bzip2-libs`
        * depends on `elfutils-libelf`
        * depends on `glibc`
        * depends on `libacl`
        * depends on `libcap`
        * depends on `libdb`
        * depends on `lua-libs`
        * depends on `nss`
        * depends on `popt`
        * depends on `rpm-build-libs`
        * depends on `rpm-libs`
        * depends on `xz-libs`
        * depends on `zlib`

* `sed`
    * `sed`, pulled in by the depsolver

* `setup`
    * `setup`, pulled in by the depsolver

* `shadow-utils`
    * `shadow-utils`, pulled in by the depsolver

* `shim-signed`
    * `shim`, pulled in by the depsolver

* `sqlite`
    * `lemon`, preventing a split
        * depends on `glibc`
    * `sqlite`, preventing a split
        * depends on `glibc`
        * depends on `ncurses-libs`
        * depends on `readline`
        * depends on `sqlite-libs`
    * ~~`sqlite-analyzer`~~, requires `tcl`
    * `sqlite-devel`, since `sqlite-libs` is our API
    * `sqlite-doc`, preventing a split
    * `sqlite-libs`, pulled in by the depsolver
    * ~~`sqlite-tcl`~~, requires `tcl`

* `syslinux`
    * `syslinux`, pulled in by the depsolver
    * `syslinux-devel`, since `syslinux` is our API
    * `syslinux-extlinux`, pulled in by the depsolver
    * `syslinux-extlinux-nonlinux`, pulled in by the depsolver
    * `syslinux-nonlinux`, pulled in by the depsolver
    * ~~`syslinux-perl`~~, requires `perl`
    * `syslinux-tftp-boot`, preventing a split
        * depends on `syslinux`

* `systemd`
    * `systemd`, pulled in by the depsolver
    * `systemd-container`, preventing a split
        * depends on `bzip2-libs`
        * depends on `glibc`
        * depends on `iptables-libs`
        * depends on `libacl`
        * depends on `libblkid`
        * depends on `libcap`
        * depends on `libcurl`
        * depends on `libgcc`
        * depends on `libgcrypt`
        * depends on `libseccomp`
        * depends on `libselinux`
        * depends on `systemd`
        * depends on `xz-libs`
        * depends on `zlib`
    * `systemd-devel`, since `systemd-libs` is our API
    * ~~`systemd-journal-remote`~~, requires `firewalld-filesystem` and `libmicrohttpd`
    * `systemd-libs`, pulled in by the depsolver
    * `systemd-pam`, pulled in by the depsolver
    * `systemd-udev`, pulled in by the depsolver

* `tar`
    * `tar`, pulled in by the depsolver

* `texinfo`
    * `info`, pulled in by the depsolver
    * ~~`texinfo`~~, requires `perl`
    * ~~`texinfo-tex`~~, requires `texinfo` and `texlive`

* `tzdata`
    * `tzdata`, pulled in by the depsolver
    * `tzdata-java`, preventing a split

* `unzip`
    * `unzip`, pulled in by the depsolver

* `ustr`
    * `ustr`, pulled in by the depsolver
    * `ustr-debug`, since `ustr` is our API
    * `ustr-debug-static`, since `ustr` is our API
    * `ustr-devel`, since `ustr` is our API
    * `ustr-static`, since `ustr` is our API

* `util-linux`
    * `libblkid`, pulled in by the depsolver
    * `libblkid-devel`, since `libblkid` is our API
    * `libfdisk`m pulled in by the depsolver
    * `libfdisk-devel`, since `libfdisk` is our API
    * `libmount`, pulled in by the depsolver
    * `libmount-devel`, since `libmount` is our API
    * `libsmartcols`, pulled in by the depsolver
    * `libsmartcols-devel`, since `libsmartcols` is our API
    * `libuuid`, pulled in by the depsolver
    * `libuuid-devel`, since `libuuid` is our API
    * ~~`python3-libmount`~~, requires `python3`
    * `util-linux`, pulled in by the depsolver
    * ~~`util-linux-user`~~, requires `libuser`
    * `uuidd`, preventing a split
        * depends on `glibc`
        * depends on `libuuid`
        * depends on `systemd`
        * depends on `systemd-libs`

* `which`
    * `which`, pulled in by the depsolver

* `xz`
    * `xz`, pulled in by the depsolver
    * `xz-devel`, since `xz-libs` is our API
    * `xz-libs`, pulled in by the depsolver
    * `xz-lzma-compat`, preventing a split
        * depends on `glibc`
        * depends on `xz`
        * depends on `xz-libs`
    * `xz-static`, since `xz-libs` is our API

* `zip`
    * `zip`, pulled in by the depsolver

* `zlib`
    * `minizip`, preventing a split
        * depends on `glibc`
        * depends on `zlib`
    * `minizip-devel`, since `minizip` is our API
    * `zlib`, pulled in by the depsolver
    * `zlib-devel`, since `zlib` is our API
    * `zlib-static`, since `zlib` is our API
