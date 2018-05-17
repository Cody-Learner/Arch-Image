
# Arch-Image
An Arch Linux container image used for testing aurt and a ready to use alternative to building your own: https://github.com/Cody-Learner/aurt.aurutils.based <br>

This is more of a VM alternative, complete, containerized Arch system, than a specialized minimal development environment.<br>

```arch-aurt<version>.tar.lrz```is a ~ 96MB lrzip compressed, ```arch-aurt<version>.tar``` using ```lrzip -z <image-name>``` for a high compression ratio. The uncompressed filesystem comes in at ~ 431MB. <br>

Decompress the image, boot into it, and follow the instructions on the screen to complete the set up, or for a step by step:<br>

Get the image:<br>
 ```$ git clone https://github.com/Cody-Learner/Arch-Image.git``` <br>

Decompress it:<br>
 ```$ lrzip -cd arch-aurt<version>.tar.lrz``` <br>
 
 Verify the checksums:<br>
```$ md5sum * ```<br>
 
Point machinectl to the directory containing arch-aurt<version>.tar:<br>
 ```$ sudo machinectl import-tar /<path>/<to>/arch-aurt<version>.tar``` <br>
 
By default, machinectl will extract the archive and place it in /var/lib/machines:<br>
 ```$ sudo ls -l /var/lib/machines``` <br>

 *   *Alternatively, as of version 2018-05-13.09.18, you can run it from any user directory. Extract the .tar archive and run:* <br>
      ```sudo systemd-nspawn -b ```
  
Start the container:<br>
 ```$ sudo systemd-nspawn -b -D /path/to/image```<br>
 
For shutdown run in container:<br>
 ```$ sudo poweroff``` <br>

Login to root, no password. Use instructions upon login to complete setup.<br>

## May 15 2018
Removed some packages from the image build script: nano file grep systemd-sysvcompat, to reduce the size.
These packages will be installed upon running the 'finbld' script inside the container image. 

After you boot the container image, you'll see this on initial root login only:
```
arch-aurt-2018-05-16 login: root

=======================================================================
 This is a stripped down Arch container image. Use as is for 
 general testing, or modify to suite for any testing requirements.

			 Run:  finbld
 
 As root to set up user aurt, and prep image for aurt testing.

 This message will run one time on first login.
 To see it again, Run: /root/first-root-run
=======================================================================
```

When finbld finishes...
```
:: To have a more useable system at this stage:

:: Installed: expac file git grep nano sudo 
:: Run 'slithery' for package status and info

:: setup user
:: setup sudo

:: non root user : aurt

:: aurt password : test
:: root password : test

:: Currently logged in as: aurt
```

...finbld 'switch user' to aurt, you'll see below on initial login only.

At this stage, you could use this image for anything.
Run the 'slithery' script for some general info.

For testing aurt, run: aurt-setup-img
```
=======================================================================
 To finish setup to test aurt, Run:   aurt-setup-img

 This will install missing base pkgs (minus linux and deps),
 install the current version of aurt from github:
 https://github.com/Cody-Learner/aurt.aurutils.based
 and configure aurt and dependencies as required.

 This message will run one time on first login.
 To see it again, Run: ~/first-aurt-run
=======================================================================

[aurt@arch-aurt-2018-05-16 root]$
```

After the 'aurt-setup-img' script finishes, you'll see this:
```
========================================================================

 Aurt setup has finished!

 For a menu ............................. Run:   aurt
 To list all installed AUR packages ..... Run:   aurt -la
 To reinstall aurutils and cower *....... Run:   aurt -S aurutils cower repoctl

 * This will install the packages with aurt so that they get 
   properly registered and show up in the local aur repo database.

 Note: Ignore the warning: 
  ==> WARNING: No packages remain, creating empty database.

 Translation: Install some AUR packages so it's not empty.
 
========================================================================

```

arch-aurt details:<br>
```
------------------------------------------------------
 Users:		Passwords:
 
 aurt		test
 root		test
------------------------------------------------------
# du -smc *

Image Size:
 
431	arch-aurt-2018-05-12.12.32.tar
96	arch-aurt-2018-05-12.12.32.tar.lrz
------------------------------------------------------

[root@test ~]# slithery

# Number of installed packages total and list: 95

acl			device-mapper		hwids			libidn			libtirpc		pcre
archlinux-keyring	dhcpcd			iana-etc		libidn2			libunistring		pcre2
argon2			e2fsprogs		iptables		libksba			libusb			perl
attr			expat			json-c			libldap			libutil-linux		pinentry
bash			file			kbd			libmnl			linux-api-headers	popt
bzip2			filesystem		keyutils		libnftnl		lz4			readline
ca-certificates		findutils		kmod			libnghttp2		nano			shadow
ca-certificates-cacert	gcc-libs		krb5			libnl			ncurses			sqlite
ca-certificates-mozilla	gdbm			libarchive		libpcap			nettle			sudo
ca-certificates-utils	glib2			libassuan		libpsl			npth			systemd
coreutils		glibc			libcap			libsasl			openssl			systemd-sysvcompat
cracklib		gmp			libcap-ng		libseccomp		p11-kit			tzdata
cryptsetup		gnupg			libelf			libsecret		pacman			util-linux
curl			gnutls			libffi			libssh2			pacman-mirrorlist	xz
db			gpgme			libgcrypt		libsystemd		pam			zlib
dbus			grep			libgpg-error		libtasn1		pambase

-----------------------------------------------------------
# Number of base group packages not installed and list: 40

diffutils		iputils			linux-firmware		mkinitcpio-busybox	psmisc			thin-provisioning-tools
gawk			jfsutils		logrotate		mpfr			reiserfsprogs		usbutils
gettext			less			lvm2			netctl			s-nail			vi
groff			libaio			man-db			openresolv		sed			which
gzip			libpipeline		man-pages		pciutils		sysfsutils		xfsprogs
inetutils		licenses		mdadm			pcmciautils		tar
iproute2		linux			mkinitcpio		procps-ng		texinfo

-----------------------------------------------------------
# Number of non base group packages installed and list: 1

sudo

-----------------------------------------------------------
# pstree:

pstree NA

-----------------------------------------------------------
# Enabled systemd services:

autovt@.service                        enabled        
console-getty.service                  enabled-runtime
getty@.service                         enabled        

-----------------------------------------------------------
# Disk storage (ext4):

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda7       517G  219G  272G  45% /

-----------------------------------------------------------
# free:

free NA


```

Some info on lrzip compression. It saved 32MB on a 102MB tar archive compared to using:<br>
```export GZIP=-9  ; tar cvpzf <image-name>.tar.gz .``` <br>

Special recomendation goes out to systemd-nspawn! If you're looking for an easy to use, fast to implement Linux container management tool, check it out. After working with Docker containers a bit, I find systemd-nspawn refreshingly KISS and a much better fit for my "cowboy coding".<br>

Also want to give a shout out to Slithery on the Arch forums: https://bbs.archlinux.org/viewtopic.php?id=236622 <br>
He offered a link to a nifty one liner he made up years ago. Need to log in for the link.... I've been using it in my images as a nice time saving tool. The package list above is part of it's output. I'll put it up here as the slithery script in the future.
