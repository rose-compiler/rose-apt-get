# ROSE Compiler debian package creator

## Directory layout

The subdirectory debian contains control, postinst, postrm ,etc to indicate dependencies and things to do after installation or uninstallation

build.sh is the script to obtain rose source files and build binaries out of them

runRoseUtil: a wrapper execution script for all ROSE tools. 

genkey.sh: simple script to generator gpg key pairs. 

## Registering an account with https://launchpad.net

If you don't yet have an account there, you have to create one first there. We assume your account is user1 for the purpose of this documentation. 

## Generating GPG key

Run genkey.sh, choose key type and length (you can use default values for them), specify you full name and email address, and set the password (it could be empty). Generating key takes a while. After this copy printed fingerprint of the key and send it to Launchpad on this [page](https://launchpad.net/~/+editpgpkeys) and wait for verification email. This email will contain encrypted GPG message. Copy it into a file and decrypt with *gpg --decrypt <file>=*. In the bottom of the list there will be a verification link, just open it with your browser.

Generation and sending key may be done only once. After that you can simply move your key between machines with *gpg --export-secret-keys* and *gpg --import*.

Find the key of your public key (the 8-character string after pub 2048R/ below), then send it to ubuntu server

```
gpg --fingerprint
/home/user1/.gnupg/pubring.gpg
-----------------------------
pub   2048R/12345678 2019-03-14
      Key fingerprint = XXXX xxxx xxxx xxxx xxxx  xxxx xxxx xxxx xxxx XXXX
uid                  You name  <user1@email.com>
sub   2048R/xxxxXXAA 2019-03-14


gpg --send-keys --keyserver keyserver.ubuntu.com 12345678 
```

## Building

To build a package simply run build.sh script. It installs all the dependencies, fetches the source code, builds it, and packs into a package.

## Publishing

You have to create a new PPA under your account with https://launchpad.net first. Assuming your account name is user1, go to https://launchpad.net/~user1 and click on "Create a new PPA" . We suggest to name your PPA as rose, then the PPA path to your binariy package is ppa:user1/rose 
 

```
dput <ppa path> <file with .changes extension>

dput ppa:user1/rose <file with .changes extension>

```
