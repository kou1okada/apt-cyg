cyg-fast
=======

cyg-fast is a command-line installer for [Cygwin](http://cygwin.com/) which cooperates with Cygwin Setup and uses the same repository. The syntax is similar to apt-get. Usage examples:

* "cyg-fast install &lt;package names&gt;" to install packages
* "cyg-fast remove &lt;package names&gt;" to remove packages
* "cyg-fast update" to update setup.ini
* "cyg-fast show" to show installed packages
* "cyg-fast find &lt;pattern(s)&gt;" to find packages matching patterns
* "cyg-fast describe &lt;pattern(s)&gt;" to describe packages matching patterns
* "cyg-fast packageof &lt;commands or files&gt;" to locate parent packages
* "cyg-fast pathof &lt;cache|mirror|mirrordir|cache/mirrordir&gt;" to show path"
* "cyg-fast key-add &lt;files&gt; ..." to add keys contained in &lt;files&gt;
* "cyg-fast key-del &lt;keyids&gt; ..." to remove keys &lt;keyids&gt;
* "cyg-fast key-list" to list keys
* "cyg-fast key-finger" to list fingerprints

Requirements
-----------

cyg-fast requires the cygwin default environment and optional packages below.

* aria2,wget,ca-certificates,gnupg

In 32bit version of cygwin, wget requires an additional setting for ca-certificates package.
Choose one of below settings.

    # 1. Create symbolic link for the default ca-directory of wget. 
    ln -s /usr/ssl /etc/
    
    # or
    # 2. Set ca-directory paramete in '/etc/wgetrc'. 
    echo "ca-directory = /usr/ssl/certs" >> /etc/wgetrc
    
    # or
    # 3. Set ca-directory paramete in '~/.wgetrc'. 
    echo "ca-directory = /usr/ssl/certs" >> ~/.wgetrc

Remarks:
Above additional settings for wget is not required for 64bit version of cygwin.
But, as of 2014-01-17, perhaps ca-certificates package makes fail of certification in 64bit version of cygwin with Windows 8. See below:

* Known Problem / [2014-01-17: ca-certificates package is not setup correct at x86_64 with Windows 8.](#2014-01-17-ca-certificates-package-is-not-setup-correct-at-x86_64-with-windows-8)

Quick start
-----------

cyg-fast is a simple script. Once you have a copy, make it executable:

    # chmod +x /bin/cyg-fast

Optionally place cyg-fast in a bin/ folder on your path.

Then use cyg-fast, for example:

    # cyg-fast install nano

New features
------------

### Pre-resolving dependencies

cyg-fast finds all depending packages before downloading and installing.

### Parallel downloading with aria2

Aria2 is an advances file downloading tool that allows you to download files with faster speed.

cyg-fast provides multi-connection downloading using aria2.

You can change the maximum number of connections with the ```--max-count``` option.

### True multi-architecture support

Let think a case that you want to install the x86 package when you are working under the x86_64 environment.
For example:

    # cyg-fast --charch x86 install chere

As of 2013-10-26, chere package is provided for only the repository for x86.

Remarks:
Of course, you must install both environments of x86_64 and x86, beforehand.

### Signature check and key management by GnuPG

The default action of cyg-fast has been changed to check signature for 'setup.ini'.
Of course you can also avoid signature check by using --no-verify or -X options.
Public keys of cygwin and cygwinports are already registered to trusted keys of embeded.
If you want to use some other public keys, please use key-* subcommands.

Contributing
------------

This project has been re-published on GitHub to make contributing easier. Feel free to fork and modify this script.

The [Google Code project](https://code.google.com/p/apt-cyg/) has a list of open issues.

### Forks on the github

Caution:
Please do not merge forks that have incompatible licenses.

Ex.) Merging to the GPL from the MIT is possible. But merging to the MIT from the GPL  is impossible.

#### Official (MIT license)

* [transcodes-open / apt-cyg](https://github.com/transcode-open/apt-cyg/network)

#### Unofficial (GPLv2)

* [cfg / apt-cyg](https://github.com/cfg/apt-cyg/network)
* [ashumkin / apt-cyg](https://github.com/ashumkin/apt-cyg/network)
* [svn2github / apt-cyg](https://github.com/svn2github/apt-cyg/network)
* [nosuchuser / apt-cyg](https://github.com/nosuchuser/apt-cyg/network)
* [kazuhisya / apt-cyg64](https://github.com/kazuhisya/apt-cyg64/network)
* [bnormsoftware / apt-cyg](https://github.com/bnormsoftware/apt-cyg/network)
* [rcmdnk / apt-cyg](https://github.com/rcmdnk/apt-cyg/network)
* [buzain / apt-cyg](https://github.com/buzain/apt-cyg/network)
* [wuyangnju / apt-cyg](https://github.com/wuyangnju/apt-cyg/network)
* [takuya / apt-cyg](https://github.com/takuya/apt-cyg/network)

Todo
------------

* Support multi mirrors: Cygwin setup can use multi mirrors. They are recorded at last-mirror section in '/etc/setup/setup.rc'. It's useful for using [Cygwinports](http://cygwinports.org/).
* Support completion: Some other forks already supported it. For example, [Milly / apt-cyg](https://github.com/Milly/apt-cyg) under the cfg / apt-cyg fork, [ashumkin / apt-cyg](https://github.com/ashumkin/apt-cyg) and etc supported it.
* Support upgrade: But maybe, busy resources can not be upgraded, and rebase problem will happen. Cygwin setup resolves by replacing them at next reboot.
* Support dependency check for remove subcommand.

Known Problem
------------
### 2014-01-17: ca-certificates package is not setup correct at x86_64 with Windows 8.

After clean installing with setup-x86_64, there are something wrong about ca-certificate package as below:

    $ ls -lnG \
         /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt \
         /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
    -r--r--r-- 1 1001 314336 Jan 17 18:17 /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
    -rw-r--r-- 1 1001      0 Oct 16 12:35 /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
    $ ls -lnG \
         /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem \
         /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
    -r--r--r-- 1 1001 232342 Jan 17 18:17 /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
    -rw-r--r-- 1 1001      0 Oct 16 12:35 /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem

The x86 environment seems no problem except that need an additional setting for wget and ca-certificate. Hmm,,,    

It seems that /usr/bin/update-ca-trust is failed at x86_64.

It's ad hoc, but effective way to fix it, is to copy files from cygwin32 to cygwin64, as below:

    cp -a /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt \
          /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
    cp -a /cygdrive/c/cygwin/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem \
          /cygdrive/c/cygwin64/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem

This problem is fixed at 2014-01-24.
version 0.18.7-1 of p11-kit, p11-kit-trust and libp11-kit0 did not work at the cygwin64 under the Windows 8.
If you face the above problem, please upgrade these three packages from version 0.18.7-1 to version 0.18.7-2.

For more details, see a thread of below:

* Cygwin mailing list / cygwin / [Re: Is there someone who have a same problem ?](http://cygwin.com/ml/cygwin/2014-01/msg00368.html)

### 2014-01-15: Signature check failed at cygwinports x86_64.
Oops, setup.bz2 is newer than setup.bz2.sig.
And it seems to be corrupted.

    $ date
    Wed Jan 15 01:30:19 JST 2014
    $ w3m -dump ftp://ftp.cygwinports.org/pub/cygwinports/x86_64/
    Index of ftp://ftp.cygwinports.org/pub/cygwinports/x86_64/
    
    [Upper Directory]
    md5.sum. . . . . . . Jan 10 14:24    184
    release/ . . . . . . Jan 10 18:00
    setup.bz2. . . . . . Jan 10 13:59   579K
    setup.bz2.sig. . . . Dec  6 11:28     72
    setup.ini. . . . . . Dec  6 11:28  3.20M
    setup.ini.sig. . . . Dec  6 11:28     72
    
    $ wget -q -O - ftp://ftp.cygwinports.org/pub/cygwinports/x86_64/setup.bz2 | bzip2 -tvv
      (stdin):
        [1: huff+mtf rt+rld]
        [2: huff+mtf data integrity (CRC) error in data
    
    You can use the `bzip2recover' program to attempt to recover
    data from undamaged sections of corrupted files.

As of 2014-01-24, above problem is recovered at least.

### FIXED: Check setup (cygcheck -c) can not detect .tar.xz packages
At cygwin 1.7.25, cygcheck is hardcoded for .tar.gz and .tar.bz2.
So check setup (cygcheck -c) can not detect .tar.xz packages.
This bug was already fixed with [src/winsup/utils/dump_setup.cc#rev1.28](http://cygwin.com/cgi-bin/cvsweb.cgi/src/winsup/utils/dump_setup.cc?cvsroot=src#rev1.28).
Please wait a release of cygwin 1.7.26.

This Problem was fixed [cygwin 1.7.26](http://cygwin.com/ml/cygwin-announce/2013-11/msg00027.html) which released at 2013-11-29.
