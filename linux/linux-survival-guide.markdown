//--- //layout: default //---

= GNU/Linux Survival Guide :data-uri: :toc: :toclevels: 3

\[\[user-management\]\] == User management

=== Users

\[cols="1,3"\] |=== |Display all | `cut -d: -f1 /etc/passwd` |Create |
`useradd -G ${g01,g02,g03...} ${user}` |Change password |
`passwd ${user}` |Display user groups | `id ${username}` |Add to group |
`usermod -a -G ${group} ${user}` |Logged Users | `w` |===

=== Groups

\[cols="1,3"\] |=== |List | `groups` |Display all |
`cut -d: -f1 /etc/group` |Create group | `groupadd ${group}` |===

=== Set a memory quote per user

./etc/security/limits.conf
--------------------------

\${username} hard as 6500000
----------------------------

\[\[file-system\]\] == File system

=== Permissions chmod (-R) 400 \${resource}

\[width="50%", options="header"\] |=== |Octal | Binary | File mode |`0`
| `000` | `---` |`1` | `001` | `--x` |`2` | `010` | `-w-` |`3` | `011` |
`-wx` |`4` | `100` | `r--` |`5` | `101` | `r-x` |`6` | `110` | `rw-`
|`7` | `111` | `rwx` |===

Alternative usage: chmod (+|-) (w|r|x) \${resource}

=== Basic operations

\[cols="1,3"\] |=== |Change file owner |
`chown (-R) user:group ${resource}` |Symbolic link creation |
`ln -s ${resource} ${link}` |Folder size | `du -sh ${folder}` |Text
replace | `sed -i 's/xxx/yyy/g' *.txt` |File system disk space | `df -h`
|File monitoring | `tail -f ${file}` |Mount |
`mount /dev/{sdax} /home/user/folder` |Umount |
`umount /home/user/folder` |Mount SSH |
`sshfs user@host:/home/user /mnt/folder` |===

=== File search

\[cols="1,3"\] |=== |Find by name | `find ${basePath} -name ${filename}`
|Find by name and execute action |
`find ${basePath} -name ${filename} -exec rm -r {} +` |Find by content
(grep) | `grep -R ${keyword} ${basePath}` |Find by content (find) |
`find ${basePath} -name ${filename} -exec grep ${keyword} {} +` |Count
occurrences | `grep -R ${keyword} ${basePath} \| wc -l` |===

=== Compress and extract files

\[options="header",cols="2,3,3"\] |=== |Format |Compress |Extract
|gzip/bzip2 |`tar -cvzf ${target} ${source}` |`tar -xvzf ${source}` |zip
|`zip ${target} ${source}` |`unzip -qq ${source}` |7z |`7z a`\${target}
\${source}`|`7z x \${source}\` |===

=== Management

\[cols="1,3"\] |=== |Delete old files |
`find ${basePath} -mtime +{days} -exec rm {} +` |Find duplicates |
`fdupes ${folder}` |===

\[\[process-management\]\] == Process management

\[cols="1,3"\] |=== |Process list | `ps -fea (\| grep ${expression})`
|Memory usage | `free -m` |Process resources | `lsof -p ${processId}`
|Force release memory | `sync && sysctl -w vm.drop_caches=3` |Disk
transfer rate | `sudo hdparm -t /dev/sda5` |===

=== Remove existing service update-rc.d -f \${servicename} remove

== Package and distribution management

\[cols="1,3"\] |=== |Install package from deb archive |
`dpkg -i foo.deb` |List installed packages | `dpkg-query -l 'foo*'`
|User manual search | `apropos ${keyword}` |Determine distro and kernel
| `uname -a ; cat /proc/version` |Search program folder |
`whereis ${programName}` |RPM to DEB | `alien --to-dev filename.rpm`
|===

\[\[networking\]\] == Networking

\[cols="1,3"\] |=== |Network interfaces | `ip addr` |Route table |
`ip route list` |DNS configuration | `cat /etc/resolv.conf` |SSH file
copy | `scp file user@host:/home/user/file` |SSH tunneling |
`ssh -L 3307:localhost:3306  root@10.1.1.12 -N -f` |TCP Traceroute |
`tcptraceroute -i wlan1 -w1 {host}` |===

//// Deprecated netstat -npl netstat -puta netstat -r ////

=== nmap

\[cols="1,3"\] |=== |Port scan | `nmap -p ${portinit}-${portEnd}`
|Silent port scan | `nmap -sS ${host}` |IP range scan |
`nmap -sP ${rango:=192.186.1.1-255}` |OS detection | `nmap -O ${host}`
|===

=== Configure proxy
-------------------

!/bin/bash
==========

execute using ". ./set-proxy"
=============================

export http\_proxy="http://{user}:{password}@{host}:{port}" // &lt;1&gt;
export https\_proxy=$http_proxy export HTTP_PROXY=$http\_proxy export
HTTPS\_PROXY=\$http\_proxy ---- &lt;1&gt; `user:password` is not
mandatory on anonymous proxy.

== Utilities and apps

=== General

\[cols="1,3"\] |=== |JSON pretty print file |
`cat unformatted.json \| python -m json.tool > formatted.json` |JSON
pretty print curl |
`curl -s http://host/resource \| python -m json.tool` |Eclipse cleanup |
`find . \( -name ".settings" -or -name ".project" -or -name ".classpath" \) -exec rm -rI {} +`
|===

=== git

\[cols="1,3"\] |=== |Update | `git fetch; git pull` |Local commit |
`git commit (-a\|${fileFilter}) -m "Comments"` |List branches |
`git branch -a` |Create local branch | `git checkout -b branchName`
|Override local changes |
`git fetch --all; git reset --hard origin/master` |Initialize local repo
| `git init` |Create patch | `git diff > name.patch` |Apply patch |
`git apply name.patch` |Export master |
`git archive master \| gzip > latest.tgz` |Determine URL |
`git remote show origin` |Determine URL (broken ref) |
`git config --get remote.origin.url` |===

=== docker

\[cols="1,3"\] |=== |Stop container | `docker stop ${containerId}` |Stop
all containers | `docker stop $(docker ps -a -q)` |Display all images |
`docker images` |Display running containers | `docker ps` |Display all
containers | `docker ps -a` |Docker compose start | `docker-compose up`
|Open container bash console | `docker exec -it ${containerName} bash`
|Remove image | `docker rmi (-f) ${imageId}` |Remove all containers |
`docker rm $(docker ps -a -q)` |===

//// === Samba

=== Montar unidad de red \[source,bash\] ---- smbmount //hsost/resorce
\${folder} -o rw,username=xxxpassword=xxx ---- ////

=== mysql

\[cols="1,3"\] |=== |Execute SQL |
`-u root -proot --execute="show databases"` |Backup |
`mysqldump -u {user} -p{password} schemaName > file.sql` |Restore /
execute script |
`mysqldump -u {user} -p{password} schemaName < file.sql` |===

=== vi

\[cols="1,3"\] |=== |Exit | `:q` |Exit !save | `:q!` |Exit and save |
`:wq` |Undo | `<esc>+u` |Delete curred line | `dd` |Insert new line |
`<ctrl>+j` |Search | `/ {key}` |Search backward | `? {key}` |Replace
first | `:s/OLD/NEW` |Globally (all) on current line | `:s/OLD/NEW/g`
|Between two lines | `:#,#s/OLD/NEW/g` |Every ocurrence in file |
`:%s/OLD/NEW/g` |===

////

apt-get ---------------------------------------------------------------------
=============================================================================

    apt-get autoremove
    apt-get install sun-java6-jdk
    apt-get install ubuntu-restricted-extras

    apt-get dist-update (cuidado, puede eliminar paquetes que necesitemos)

Deshabilitar ipv6 -----------------------------------------------------------
=============================================================================

    vi /etc/modprobe.d/blacklist
    > añadir al final la linea:
    blacklist ipv6
    > para asegurarnos ejecutamos
    ip a | grep inet6

Programa para editar la configuracion de los servicios ----------------------
=============================================================================

    sudo apt-get install sysv-rc-conf
    syssv-rc-conf
    > visualizaremos una aplicacion con los runlevels de cada servicio

////

=== Configure defaults applications

  ----
  \#!/
  bin/
  bash

  \#
  Use
  this
  comm
  and
  to
  quer
  y
  medi
  a
  type
  :
  \#
  xdg-
  mime
  quer
  y
  file
  type
  \${f
  ile}

  DEFA
  ULT\
  _TEX
  T\_E
  DITO
  R=co
  de.d
  eskt
  op

  xdg-
  mime
  defa
  ult
  fire
  fox.
  desk
  top
  text
  /xml
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  text
  /pla
  in
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  appl
  icat
  ion/
  xml
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  appl
  icat
  ion/
  json
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  text
  /x-j
  ava
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  appl
  icat
  ion/
  x-sh
  ells
  crip
  t
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  text
  /x-p
  ytho
  n
  xdg-
  mime
  defa
  ult
  \${D
  EFAU
  LT\_
  TEXT
  \_ED
  ITOR
  }
  text
  /mar
  kdow
  n

  echo
  "Def
  ault
  appl
  icat
  ions
  :"

  cat
  \~/.
  loca
  l/sh
  are/
  appl
  icat
  ions
  /mim
  eapp
  s.li
  st
  ----

=== SSH login without password prompt

  -----------------------------------------
  \#!/usr/bin/expect
  spawn scp ${source} user@host:${target}
  expect "\*password:"
  send "changeit\r"
  interact
  -----------------------------------------

=== PostgreSQL

.backup-restore-script.sh
-------------------------

!/usr/bin/env bash
==================

echo "PostGree backup helper"

DATABASE='performance' SCHEMA\_NAME='public' SYS\_USER='postgres'
HOST='localhost' USER='performance' FILE='dump-performance-postgresql'

read -p "Options: (C)reate; (R)estore: " OPTION

if \[ \$OPTION = "C" \] then echo "Creating backup" rm \$FILE pg\_dump
\$DATABASE -h \$HOST -U \$USER -W &gt; \$FILE elif \[ \$OPTION = "R" \]
then echo "Restoring backup" echo "DROP SCHEMA public CASCADE;" psql
\$DATABASE -h \$HOST -U \$SYS\_USER -c 'DROP SCHEMA public CASCADE;'
echo "CREATE SCHEMA public AUTHORIZATION performance;" psql \$DATABASE
-h \$HOST -U \$SYS\_USER -c 'CREATE SCHEMA public AUTHORIZATION
performance;' psql \$DATABASE -h \$HOST -U \$USER -W &lt; \$FILE rm -r
/KISS/performance/local else echo "Invalid option" fi ----

////

Generar un pendrive bootable a partir de una ISO

http://rufus.akeo.ie/ (windows)

Ok after some research I've figured out a solution, and I'll go through
it step by step. Problem was two-fold. Plug in the USB flash drive and
determine the device it's mounted on with the command: sudo fdisk -l

This time around it was /dev/sdc1 for me, so I'll use that as my
example. Umount the device umount /dev/sdc1

Not sure if necessary but I formatted the drive in FAT32, just in case
sudo mkdosfs -n 'USB-Drive-Name' -I /dev/sdc -F 32

Now my ISO was using isolinux not syslinux. I knew it worked with CDs so
I figured out that I needed to call the isohybrid command, which allows
for an ISO to be recognized by the BIOS from a hard drive. isohybrid
filename.iso

You can find out more about this command here, but this was the cause of
the message "Missing Operating System" The first problem was fixed, but
now it said "isolinux.bin was missing or corrupt" The next step is to
copy the iso. My second problem lay here, where I was copying to the
partition, sdc1, not the device, sdc. sudo dd if=filename.iso
of=/dev/sdc bs=4k

This seems to work just fine, but the forum where I got the last fix, it
was recommended to do the following before unplugging the device: sync
sudo eject /dev/sdc

////
