> **Note:** this repo is a fork of the original github project nzbget/nzbget
> originally written by @hugbug. He deserves all the credit for this
> software.
> 
> Since he has now archived the project, I've combined my changes
> (surgical fixes to accomodate the intentially-malformed 'wtf' NZB
> headers, which result in the infamous 'abc.xyz' file names) and
> merged several pull requests from the original repo.
> - Paul, 2022.11.28

NZBGet
=======

NZBGet is a binary downloader, which downloads files from Usenet
based on information provided by nzb-files. 

NZBGet is written in C++ and is known for its performance and efficiency.

NZBGet can run on almost any device - classic PC, NAS, media player, SAT-receiver, WLAN-router, etc.

The download area provides precompiled binaries for Windows, macOS,
Linux (compatible with many CPUs and platform variants), FreeBSD
and Android. For other platforms the program can be compiled from
sources.

This is a summary. For full documentation, please visit the NZBGet-NG
home page at: http://nzbget-ng.github.io

Contents
--------
1.  [About NZBGet](#about-nzbget)
2.  [Supported Platforms](#supported-platforms)
3.  [Prerequisites for Posix](#prequisites-for-posix)
4.  [Installation on POSIX](#installation-on-posix)
5.  [Compiling for Windows](#compiling-for-windows)
6.  [Configuring nzbget](#configuring-nzbget)
7.  [Usage](#usage)
8.  [Authors](#authors)
9.  [Copyright](#copyright)
10. [Contact](#contact)

About NZBGet <a name="about-nzbget"></a>
============

NZBGet is a binary newsgrabber, which downloads files from usenet
based on information provided by nzb files. NZBGet can be used in
standalone and in server/client modes. In standalone mode you
pass a nzb-file as parameter in command-line, NZBGet downloads
listed files and then exits.

In server/client mode NZBGet runs as server in background.
Then you use client to send requests to server. The sample requests
are: download nzb-file, list files in queue, etc.

There is also a built-in web-interface. The server has RPC-support
and can be controlled from third party applications too.

Standalone-tool, server and client are all contained in only one
executable file "nzbget". The mode in which the program works
depends on command-line parameters passed to the program.

Supported Platforms <a name="supported-platforms"></a>
===================

NZBGet is written in C++ and works on Windows, OS X, Linux and
most POSIX-conform Operating systems.

Clients and servers running on different operating systems may
communicate with each other. For example, you can use NZBGet as
client on Windows to control your NZBGet server running on Linux.

The download section of NZBGet web-site provides binary files
for Windows, OS X and Linux. For other POSIX-compliant systems
you will need to compile the program yourself.

If you have downloaded binaries you can just jump to section
"Configuration".

Prerequisites for POSIX <a name="prequistites-for-posix"></a>
=======================

NZBGet is developed on a linux-system, but it runs on other
POSIX platforms.

NZBGet requires the following libraries:

- libstdc++     (usually provided by the compiler)
- libxml2       (http://www.xmlsoft.org)

The following libraries are optional:

- for curses-output-mode (enabled by default):
    - libcurses   (usually part of commercial systems)
      or (preferred)
    - libncurses  (http://invisible-island.net/ncurses)

- for encrypted connections (TLS/SSL):
    - OpenSSL     (http://www.openssl.org)
      or
    - GnuTLS      (http://www.gnu.org/software/gnutls)

- for gzip support in web-server and web-client (enabled by default):
    - zlib     (http://www.zlib.net)

All these libraries are included in modern POSIX distributions and
should be available as installable packages. Please note that you
also need the developer packages for these libraries too, they 
package names have often suffix "dev" or "devel". On other systems
you may need to download the libraries at the given URLs and 
compile them (see hints below).

Installation on POSIX <a name="installation-on-posix"></a>
=====================

Installation from the source distribution archive (nzbget-**_\<version\>_**.tar.gz):

- untar the nzbget-source via
>  tar -zxf nzbget-**_\<version\>_**.tar.gz

- change into nzbget-directory via
>  cd nzbget-**_\<version\>_**

- configure it via
>  ./configure

You might have to tell `configure` where to find some libraries.
  `./configure --help` is your friend!
See also "Configure Options" below.

- in case you don't have root access, or want to install the program
  in your home directory, use the configure parameter --prefix, e. g.:

  `./configure --prefix ~/usr`

- compile it via `make`

- install it via `sudo make install`

- install configuration files into <prefix>/etc via:
  `make install-conf`

  (you can skip this step if you intend to store configuration
  files in a non-standard location)

Options for `configure`
-----------------------
You may run configure with additional arguments:

`--disable-curses` - to make without curses-support. Use this option
if you can not use curses/ncurses.

`--disable-parcheck` - to make without parcheck-support. Use this option
if you have troubles when compiling par2-module.

`--with-tlslib=` (OpenSSL|GnuTLS) - to select which TLS/SSL library
should be used for encrypted server connections.

`--disable-tls` - to make without TLS/SSL support. Use this option if
you can not neither OpenSSL nor GnuTLS.

`--disable-gzip` - to make without gzip support. Use this option
if you can not use zlib.

`--enable-debug` - to build in debug-mode, if you want to see and log
debug-messages.

Optional package: `par-check`
---------------------------
NZBGet can check and repair downloaded files for you. For this purpose
it uses library par2.

For your convenience the source code of `libpar2` is integrated into
NZBGet’s source tree and is compiled automatically when you make NZBGet.

In a case errors occur during this process the inclusion of par2-module
can be disabled using configure option `--disable-parcheck`:

> ./configure --disable-parcheck

Optional package: `curses`
-------------------------
For `curses-outputmode` you need ncurses or curses on your system.
If you do not have one of them you can download and compile ncurses
yourself. The following configure-parameters may be useful:

> --with-libcurses-includes=/path/to/curses/includes <br>
> --with-libcurses-libraries=/path/to/curses/libraries

If you are not able to use curses or ncurses or do not want them you can
make the program without support for curses using option "--disable-curses":

> ./configure --disable-curses

Optional package: TLS
-------------------------
To enable encrypted server connections (TLS/SSL) you need to build the program
with TLS/SSL support. NZBGet can use eitherr of two libraries: OpenSSL or GnuTLS.

Configure-script checks which library is installed and uses it. If both are
available it gives the precedence to OpenSSL. You may override that with
the option `--with-tlslib=`(OpenSSL|GnuTLS).

For example to build with GnuTLS:

> ./configure --with-tlslib=GnuTLS

Following configure-parameters may be useful:

> --with-libtls-includess=/path/to/gnutls/includes <br>
> --with-libtls-libraries=/path/to/gnutls/libraries

> --with-openssl-includess=/path/to/openssl/includes <br>
> --with-openssl-libraries=/path/to/openssl/libraries

If none of these libraries is available you can make the program without
TLS/SSL support using option `--disable-tls`:

> ./configure --disable-tls
  
Compiling on Windows <a name="compiling-on-windows"></a>
====================

NZBGet is developed on Windows using MS Visual Studio 2015 (Community Edition).
The project file is provided.

To compile the program with TLS/SSL support you need either OpenSSL or GnuTLS:
- OpenSSL  (http://www.openssl.org) <br>
  or
- GnuTLS   (http://www.gnu.org/software/gnutls)

Also required are:
- Regex (http://gnuwin32.sourceforge.net/packages/regex.htm)
- Zlib (http://gnuwin32.sourceforge.net/packages/zlib.htm)

Configuring `nzbget` <a name="configuring-nzbget"></a>
====================

NZBGet requires a configuration file.

An example configuration file is provided in `nzbget.conf`, which
is installed into **_\<prefix\>_**`/share/nzbget`
(where **_\<prefix\>_** depends on system conventions and options 
given to `configure`. It is typically `/usr`, `/usr/local` or `/opt`).

The installer adjusts the file according to your system paths. If
you have performed the installation step `make install-conf` this
file is already copied to **_\<prefix\>_**`/etc` and NZBGet finds it
automatically. If you install the program manually from a binary
archive you have to copy the file from `<prefix>/share/nzbget`
to one of the locations listed below.

Open the file in a text editor and modify it according to your needs.

You must set at least the option `MAINDIR` and one news server in
the configuration file. The file provided contains comments on how
to use each option.

The program looks for configuration file in following standard
locations (in this order):

On POSIX systems:

    1. <path to executable>/nzbget.conf
    2. ${HOME}/.nzbget
    3. /etc/nzbget.conf
    4. /usr/etc/nzbget.conf
    5. /usr/local/etc/nzbget.conf
    6. /opt/etc/nzbget.conf

On Windows:

    1. <EXE-DIR>\nzbget.conf

If you put the configuration file in other place, you can use command-
line switch `-c <path to config file>` to point the program to the 
correct location.

In special cases, you can run program without a config file using
switch `-n`, which disables the automatic search for a config
file. You need to use the switch `-o` to pass
required configuration options via command-line.

> *Note:* this is not recommended, since the credentials required to
> access your news server account will be exposed in plain text on
> the command line.

Usage <a name="usage"></a>
=====

NZBGet can be used in either standalone mode, which downloads a single
file, or as a server which is able to queue up numerous download
requests.

> *TIP:* for Windows users: NZBGet can be controlled via various
> command line parameters. For easier use, there is a simple shell
> script included in "nzbget-shell.bat". Start this script from
> Windows Explorer and you will be running a command shell with
> PATH adjusted to find NZBGet executable. Then you can type all
> commands without full path to nzbget.exe.

Standalone mode:
----------------

> nzbget <nzb-file>

Server mode:
------------

First start the nzbget-server:

- in console mode:

>  nzbget -s

- or in daemon mode (POSIX only):

>  nzbget -D

- or as a service (Windowx only, firstly install the service with 
  the command `nzbget -install`):

> net start NZBGet

To stop server use:

> nzbget -Q

> *TIP for POSIX users:* with included script "nzbgetd" you can use standard
commands to control daemon:

- nzbgetd start
- nzbgetd stop
- etc.

When NZBGet is started in console server mode it displays a message that
it is ready to receive download requests. In daemon mode it doesn't print
any messages to console since it runs in background.

When the server is running it is possible to queue up downloads. This can
be done either in terminal with `nzbget -A <nzb-file>` or by uploading
a nzb-file into server's monitor-directory (`<MAINDIR>/nzb` by default).

To check the status of server start client and connect it to server:

> zbget -C

The client have three different (display) outputmodes, which you can select
in configuration file (on client computer) or in command line. Try them:

> nzbget -o outputmode=log -C

> nzbget -o outputmode=color -C

> nzbget -o outputmode=curses -C

To list files in server's queue:

> nzbget -L

It should print something like:

```
[1] nzbname\filename1.rar (50.00 MB)
[2] nzbname\filename1.r01 (50.00 MB)
[3] another-nzb\filename3.r01 (100.00 MB)
[4] another-nzb\filename3.r02 (100.00 MB)
```

This is the list of individual files listed within nzb-file. To print
the list of nzb-files (without content) add G-modifier to the list command:

```
[1] nzbname (4.56 GB)
[2] another-nzb (4.20 GB)
```

The numbers in square braces are ID's of files or groups in queue.
They can be used in edit-command. For example to move file with
ID 2 to the top of queue:

> nzbget -E T 2

or to pause files with IDs from 10 to 20:

> nzbget -E P 10-20

or to delete files from queue:

> nzbget -E D 3 10-15 20-21 16

The edit-command has also a group-mode which affects all files from the
same nzb-file. You need to pass an ID of the group. For example to delete
the whole group 1:

> nzbget -E G D 1

The switch `-o` is useful to override options in configuration files.
For example:

> nzbget -o reloadqueue=no -o dupecheck=no -o parcheck=yes -s

or:

> nzbget -o createlog=no -C

Running client & server on separate machines:
---------------------------------------------

Since nzbget communicates via TCP/IP it's possible to have a server running on
one computer and adding downloads via a client on another computer.

Do this by setting the "ControlIP" option in the nzbget.conf file to point to the
IP of the server (default is localhost which means client and server runs on
same computer)

Security warning
----------------

NZBGet communicates via unsecured socket connections. This makes it vulnerable.
Although server checks the password passed by client, this password is still
transmitted in unsecured way. For this reason it is highly recommended
to configure your Firewall to not expose the port used by NZBGet to WAN.

If you need to control server from WAN it is better to connect to server's
terminal via SSH (POSIX) or remote desktop (Windows) and then run
nzbget-client-commands in this terminal.

Post processing scripts
-----------------------

After the download of nzb-file is completed nzbget can call post-processing
scripts, defined in configuration file.

Example post-processing scripts are provided in directory "scripts".

To use the scripts copy them into your local directory and set options
`<ScriptDir>`, `<PostScript>` and `<ScriptOrder>`.

For information on writing your own post-processing scripts please
visit NZBGet web site.

Web Interface
-------------

NZBGet has a built-in web-server providing the access to the program
functions via web-interface.

To activate web-interface set the option "WebDir" to the path with
web-interface files. If you install using "make install-conf" as
described above the option is set automatically. If you install using
binary files you should check if the option is set correctly.

To access web-interface from your web-browser use the server address
and port defined in NZBGet configuration file in options `ControlIP`
and `ControlPort`. For example:

> http://localhost:6789/

For login credentials type username and the password defined by
options `ControlUsername` (default `nzbget`) and `ControlPassword`
(default `tegbzn6789`).

> **Caution:** Like any default password, this should be changed
> immediately after installation!

In a case your browser does not remember credentials, to prevent
typing them each time, there is a workaround - use a URL of the
form:

> http://localhost:6789/username:password/

> **Caution** In this case the password is saved in a bookmark or
> in browser history in plain text, and may easily be discovered
> by anyone who has access to your computer.

Authors <a name="authors"></a>
=======

The project was originally created by Sven Henkel
<sidddy@users.sourceforge.net> in 2004 and later developed by
Bo Cordes Petersen <placebodk@users.sourceforge.net> until 2005.

In 2007, Andrey Prygunkov <hugbug@users.sourceforge.net> took
over the project which was abandoned in 2005. Since then, he has
completely rewritten it. Andrey maintained it through July 2021.

Andrey's repo was archived in November 2022, containing a number
of outstanding pull requests and issues. A github organization
named `nzbget-ng` was created and Andrey's `nzbget/nzbget` repo
was forked into it. The pending pull requests were then
merged into it.

Other Components Used by `nzbget`
---------------------------------

The NZBGet distribution archive includes additional components
written by other authors:

| Component        | Author                              | Reference                                |
|------------------|-------------------------------------|-------------------------------------------------|
| Par2             | Peter Brian Clements                | peterbclements@users.sourceforge.net            |
| Par2 library API | Francois Lesueur                    | flesueur@users.sourceforge.net                  |
| Catch            | Two Blue Cubes Ltd                  | <https://github.com/philsquared/Catch>          |
| jQuery           | John Resig <br> Dojo Foundation     | <http://jquery.com> <br> <http://sizzlejs.com>  |
| Bootstrap        | Twitter, Inc                        | <http://twitter.github.com/bootstrap>           |
| Raphaël          | Dmitry Baranovskiy <br> Sencha Labs | <http://raphaeljs.com> <br> <http://sencha.com> |
| Elycharts        | Void Labs s.n.c.                    | <http://void.it>                                |
| iconSweets       | Yummygum                            | <http://yummygum.com>                           |

Copyright <a name="copyright"></a>
=========

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

The complete content of license is provided in file COPYING.

Additional exemption: compiling, linking, and/or using OpenSSL is allowed. 

Contact <a name="contact"></a>
-------

The best place to start is `/r/nzbget` on reddit:

    https://www.reddit.com/r/nzbget/

Other users should be able to help if you're having struggling.
The `nzbget` developers have been known to hang out there, too.

Please check the reddit forum first **before** filing a new issue on
github. Only file an issue if no-one on the subreddit can help you.
Developers would rather be _developing_ than answering the same
questions repeatedly. 