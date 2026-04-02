# Домашнє завдання №2. Файлова система і права доступу

## Завдання 1. Навігація по файловій системі

```bash
cd ~
pwd
/home/w3nden

cd /
ls
bin                home               mnt   sbin.usr-is-merged  usr
bin.usr-is-merged  lib                opt   snap                var
boot               lib64              proc  srv
cdrom              lib.usr-is-merged  root  swapfile
dev                lost+found         run   sys
etc                media              sbin  tmp

cd /etc
ls
adduser.conf            gtk-4.0              pnm2ppa.conf
alsa                    gufw                 polkit-1
alternatives            hdparm.conf          ppp
anacrontab              host.conf            profile
apg.conf                hostname             profile.d
apm                     hosts                protocols
apparmor                hosts.allow          pulse
apparmor.d              hosts.deny           python3
apt                     hp                   python3.12
avahi                   ifplugd              rc0.d
bash.bashrc             init                 rc1.d
bash_completion         init.d               rc2.d
bash_completion.d       initramfs-tools      rc3.d
bindresvport.blacklist  inputrc              rc4.d
binfmt.d                insserv.conf.d       rc5.d
bluetooth               inxi.conf            rc6.d
bogofilter.cf           ipp-usb              rcS.d
brlapi.key              iproute2             resolv.conf
brltty                  issue                rmt
brltty.conf             issue.net            rpc
ca-certificates         kernel               rsyslog.conf
ca-certificates.conf    kerneloops.conf      rsyslog.d
chatscripts             ld.so.cache          rygel.conf
chromium                ld.so.conf           sane.d
colord                  ld.so.conf.d         security
console-setup           legal                selinux
cracklib                libao.conf           sensors3.conf
credstore               libaudit.conf        sensors.d
credstore.encrypted     libblockdev          services
cron.d                  libibverbs.d         sgml
cron.daily              libnl-3              shadow
cron.hourly             libpaper.d           shadow-
cron.monthly            libreoffice          shells
crontab                 locale.alias         skel
cron.weekly             locale.conf          snmp
cron.yearly             locale.gen           speech-dispatcher
cups                    localtime            ssh
cupshelpers             logcheck             ssl
dbus-1                  login.defs           subgid
dconf                   logrotate.conf       subgid-
debconf.conf            logrotate.d          subuid
debian_version          lsb-release          subuid-
debuginfod              machine-id           sudo.conf
default                 magic                sudoers
deluser.conf            magic.mime           sudoers.d
depmod.d                manpath.config       sudo_logsrvd.conf
dhcp                    mime.types           supercat
dhcpcd.conf             mke2fs.conf          sysctl.conf
dictionaries-common     ModemManager         sysctl.d
dpkg                    modprobe.d           sysstat
e2scrub.conf            modules              systemd
emacs                   modules-load.d       terminfo
environment             mtab                 thermald
environment.d           nanorc               timezone
ethertypes              netconfig            tmpfiles.d
firefox                 netscsid.conf        udev
fonts                   network              udisks2
fprintd.conf            networkd-dispatcher  ufw
fstab                   NetworkManager       update-manager
fuse.conf               networks             update-motd.d
fwupd                   newt                 update-notifier
gai.conf                nftables.conf        UPower
gdm3                    nsswitch.conf        usb_modeswitch.conf
geoclue                 openvpn              usb_modeswitch.d
ghostscript             opt                  vconsole.conf
glvnd                   os-release           vdpau_wrapper.cfg
gnome                   PackageKit           vim
gnome-remote-desktop    pam.conf             vtrgb
gnutls                  pam.d                vulkan
groff                   papersize            wgetrc
group                   passwd               wodim.conf
group-                  passwd-              wpa_supplicant
grub.d                  pcmcia               X11
gshadow                 perl                 xattr.conf
gshadow-                pki                  xdg
gss                     plymouth             xml
gtk-2.0                 pm                   zsh_command_not_found

cd /home
ls
w3nden
```

## Завдання 2. Робота з файлами, каталогами та посиланнями

```bash
cd ~
mkdir lab2
cd lab2
pwd
/home/w3nden/lab2

touch file.txt
echo "Hello" > file.txt
cp file.txt file_copy.txt
mv file_copy.txt file_renamed.txt
ln file.txt file_hardlink.txt
ln -s file.txt file_symlink.txt

cd ~
find . -name "file.txt"
./lab2/file.txt
```

## Завдання 3. Права доступу та umask

```bash
cd ~/lab2
ls -l file.txt
-rw-rw-r-- 2 w3nden w3nden 6 apr  2 13:05 file.txt

chmod 444 file.txt
ls -l file.txt
-r--r--r-- 2 w3nden w3nden 6 apr  2 13:05 file.txt

chmod 644 file.txt
ls -l file.txt
-rw-r--r-- 2 w3nden w3nden 6 apr  2 13:05 file.txt

umask
0002

umask 022
umask
0022
```

## Завдання 4. Користувачі

```bash
sudo adduser trainee
sudo usermod -aG sudo trainee
getent passwd trainee
trainee:x:1001:1001:,,,:/home/trainee:/bin/bash
```
