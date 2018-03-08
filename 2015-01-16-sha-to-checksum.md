---
layout: post
title: "Easily check SHA checksum when a checksum file is not provided"
tags: security SHA Bash shell shasum
---
<img style="float:left;border-radius:10px;border:1px solid #000;margin-right:15px;margin-top:5px" class="img-inline" src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bb/Cyphoma_signatum_%28Fingerprint_Cowry_-_Haiti%29.jpg/320px-Cyphoma_signatum_%28Fingerprint_Cowry_-_Haiti%29.jpg" alt="Cyphoma Signatum" width="215px" height="160px"/>

It is always a good idea to ensure that a file you download is exactly the one you are expecting. For obvious security reason, you want to check its integrity. One way to verify that it hasn't been modified during transport is to use SHA checksum when provided. Because I always forgot the syntax of `shasum` command-line tool, I wrote this memo and a Bash script to automate the process.

<!-- more -->

Checksum is a unique ID for a file like a fingerprint for a human. When you go to a download page for a package, checksum may be provided as a long identifier, or in a checksum file.

![MAMP-download-screenshot](/images/2015-01-16-Easily-check-SHA-checksum-when-a-checksum-file-is-not-provided/SHA-on-MAMP-download.png)


Verify checksum without a checksum file
--------

In the case a checksum file is provided, the verification is easy, because you just have to run `shasum -c` command.

However, sometimes, only the checksum is provided.
You could manually create a checksum file and then use `shasum -c` command. However, this solution is not very convenient because you have to create a file, and furthermore not to forget its format, which needs **two spaces** between the checksum and the file name:

	checksum  file-name
	
Or, you could directly use _[here string](http://tldp.org/LDP/abs/html/x17837.html)_ to feed `shasum` command, although you still need to remember the two spaces.

Therefore, I created a command-line script that takes the checksum and the name of the file as arguments: `checkshasum.sh CHECKSUM FILE`. You only need to copy the checksum, and paste it as an argument to the script in a terminal.

```bash
$> ./checkshasum.sh ed3f6dbbe8ac2d89aba26b9cf152f2e477c561e8 Downloads/MAMP_MAMP_PRO_3.0.7.3.pkg
MAMP_MAMP_PRO_3.0.7.3.pkg: OK
```

The `checkshasum.sh` script
------------

```	bash
#!/bin/bash

#
# Check SHA checksum of a file when no checksum file is provided.
#

if [[ "$#" -lt "2" ]]; then
	echo "Usage: checkshasum CHECKSUM FILE"
	echo "Check SHA checksums using shasum(1) command without checksum file."
	exit 64 # EX_USAGE from sysexits.h
fi

# Check SHA checksum. It uses 'here string' to feed shasum with a file.
# NOTE: There are two spaces '  ' between $1 and $2 (checksum file format).
shasum -c <<< "$1  $2"
```

You can copy it and do whatever you want with it. I decline all responsibilities for any use.

`shasum` memo
---------

 - `shasum FILE` -- compute checksum of a file.
 - `shasum -a N FILE` -- compute checksum using algorithm `N` (1 (default), 224, 256, 384, 512, 512224, 512256).

```bash
$> shasum MAMP_MAMP_PRO_3.0.7.3.pkg
ed3f6dbbe8ac2d89aba26b9cf152f2e477c561e8  MAMP_MAMP_PRO_3.0.7.3.pkg
$> shasum -a 256 MAMP_MAMP_PRO_3.0.7.3.pkg
840877041af7a06d50a3ddc67030ae4f193f70add473ebad0f7418daf80cd553  MAMP_MAMP_PRO_3.0.7.3.pkg
```

 - `shasum -c CHECKSUMFILE` -- check the checksum using a checksum file.

```bash
$> shasum -c MAMP_MAMP_PRO_3.0.7.3.pkg.sha1
MAMP_MAMP_PRO_3.0.7.3.pkg: OK
```
 - `shasum -c <<< "CHECKSUM  FILE"` -- check the checksum without a checksum file (two spaces between CHECKSUM and FILE).

```bash
$> shasum -c <<< "ed3f6dbbe8ac2d89aba26b9cf152f2e477c561e8  MAMP_MAMP_PRO_3.0.7.3.pkg"
MAMP_MAMP_PRO_3.0.7.3.pkg: OK
```
 
 - `shasum FILE > CHECKSUMFILE` -- create checksum file.

```bash
$> shasum MAMP_MAMP_PRO_3.0.7.3.pkg > MAMP_MAMP_PRO_3.0.7.3.pkg.sha1
```
Beware that this command will overwrite any existing checksum file.

Conclusion 
---------

Whatever you download, it is always a good habit to check its integrity when a signature is provided. It can be SHA or MD5 checksums, or GPG signature file. Also, you should always run an antivirus.

1. Download.
2. Run an antivirus ([ClamAV](http://www.clamav.net/index.html) is a very good open source and free antivirus for every platform).
3. Check integrity.
4. Be safe!

Oh! And of course, immediately delete any file which doesn't pass antivirus, or integrity test.