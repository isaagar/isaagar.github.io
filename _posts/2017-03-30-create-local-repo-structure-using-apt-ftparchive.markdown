---
layout: post
title:  "Create local repo structure using apt-ftparchive"
date:   2017-01-24
description:
---
apt-ftparchive is used for generating index files. I’m mentioning following steps to create index files out of existing pool directory.

Go to the directory in which pool and dists directory are placed. If you want to create dists from scratch again then you should delete the dists.

Lets create directories for dists structure
```
$ mkdir -p dists/<codename>/main/binary-<arch>
```

## Create packages file
```
$ apt-ftparchive packages pool/dists/main/<codename>/binary-<arch>/Packages
$ gzip -9 -c Packages > Packages.gz
```

## Create Release file
```
$ apt-ftparchive release dists/main/< codename >/binary-$arch > dists/main/$codename/binary-$arch/Release
```

The above generated Release file contains only checksums.

To add codename,version etc . Create apt.conf like below

```
APT {
FTPArchive {
Release {
Origin “Hamara Sugam”;
Label “Hamara Sugam”;
Suite “namaste”;
Version “2.0”;
Codename “namaste”;
Architectures “amd64”;
Components “main contrib non-free”;
};
};
};
```

Now, again generate Release file using apt.conf

```
$ apt-ftparchive -c apt.conf release dists/namaste > dists/namaste/Release
```

Specifying options on command line instead of creating config file

```
$ apt-ftparchive -o APT::FTPArchive::Release::Origin=”Hamara Sugam” \
-o APT::FTPArchive::Release::Label=”Hamara Sugam” \
-o APT::FTPArchive::Release::Suite=”namaste” \
-o APT::FTPArchive::Release::Version=”2.0″ \
-o APT::FTPArchive::Release::Codename=”namaste” \
-o APT::FTPArchive::Release::Architectures=”amd64″ \
-o APT::FTPArchive::Release::Components=”main contrib non-free” \
release dists/namaste/ > dists/namaste/Release
```
