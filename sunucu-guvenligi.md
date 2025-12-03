# Sunucu güvenliğinin sağlanması
Bu yazıda sunucunuzun güvenliğini nasıl arttırabiliriz onu anlatacağım.
Sunucuda debian kullandığınızı farzediyorum. Debian kullanmıyorsanız silip debiana geçin.

## Gereksiz paketlerin silinmesi
Öncelikle sunucu firmasının imajı bizim için hayırlamaka kullandığı fakat sonradan silmediği paketleri silelim.
```bash
# kurulu listesinden gereksiz olan şeyleri bulalım.
$ apt list --installed
# silelim (sunucuya python veya x11 ile ilgili paketler olmaması lazım. cloud-init varsa mutlaka silin)
$ apt purge --autoremove python3* cloud-init -yq
```

## Ssh ayarları
Ssh bağlantısı için öncelikle elinizde sadece root hesabı varsa yeni kullanıcı oluşturun ve root ile girişi kapatın.
```bash
# tahmin etmesi zor kullanıcı adı kullanın
$ adduser pingu31
```
Şu ayarları /etc/ssh/sshd_config (veya .d olandan) değiştirin
```bash
# portu 22 yapmayın. 
Port 2231
# root girişi kapatın
PermitRootLogin no
# parolalı girişi kapatın (sadece ssh keyi ile giriş yapın)
# Bunu yapmadan önce sunucuya ssh keyinizi ekleyip önce test edin.
PasswordAuthentication no
# Pam kapatın (zaten parola ile doğrulama yapmadığımız için gerek yok)
UsePAM no
```
## sysctl ayarları
Sunucuyu tarayan botlara gözükmemek için ve sunucuyu daha sağlam hale getirmek için bazı değişiklikler yapmamız gerekmektedir. 
/etc/sysctl.d/990-security.conf dosyasını aşağıdaki gibi doldurun. 
```
## eBPF hardening
kernel.unprivileged_bpf_disabled=1
net.core.bpf_jit_harden=2

## better ASLR
kernel.randomize_va_space=2
vm.mmap_rnd_bits=32
vm.mmap_rnd_compat_bits=16

## fs hardening
fs.protected_hardlinks=1
fs.protected_symlinks=1
fs.protected_fifos=2
fs.protected_regular=2

## mmap
vm.max_map_count=1048576

## suid
fs.suid_dumpable=0

## network
net.ipv4.tcp_syncookies=1
net.ipv4.tcp_rfc1337=1
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.default.rp_filter=1
net.ipv4.icmp_echo_ignore_all=1
net.ipv6.icmp.echo_ignore_all=1
net.ipv4.icmp_ignore_bogus_error_responses=1
net.ipv4.tcp_timestamps=0
net.ipv4.ip_default_ttl=128
```
Ayarları uygulamak için:
```
sysctl -p /etc/sysctl.d/990-security.conf
```

## Dosyaları şifreleme
Sunucu firmasının disk yedeğini alıp dosya sisteminden bilgi elde etmesini risk olarak görüp buna engel olmak istiyorsanız disk şifreleme kullanabilirsiniz. 
Sunucuda luks kullanmak mümkün olmadığı için dosya tabanlı şifrelemeler işimize daha çok yarayacaktır.

Detaylı bilgi için: https://wiki.archlinux.org/title/Fscrypt

## Docker / Podman kullanın
Sunucuda her ne çalıştıracaksanız sunucuya doğrudan kurmak yerine docker veya podman ile kullanın.
Bu sayede oldu da uygulamaya sızıldı tam yetki alamamış olurlar.
Ayrıca yedek alması ve güncellemesi daha basit.

## Açık portları kontrol edin
Açık portları kontrol edin ve gerekli olmadığını düşündüğünüz şeyleri sonlandırın. 
```bash
ss -tlpnu
# veya
netstat -tlpnu
```

## Sunucuda kerneli güncelleme
Sunucuyu normal şekilde yeniden başlatmak yerine `kexec` kullanın. Hem daha hızlı yeniden başlatılır hem de olası bootloader sorunlarını ortadan kaldırır.
```bash
kexec -l /boot/vmlinuz-xxx --initrd=/boot/initrd.img-xxx --reuse-cmdline
kexec -e
```

## Systemd kullanmayın
Systemd varsa silip sysvinit veya openrc kullanın. 

* Bunun için [devuan](https://www.devuan.org/) depolarını da ekleyip sistemi devuana çevirebilirsiniz.

* Alternatif olarak debian üzerine sysvinit ve openrc kurmak mümkündür. https://gist.github.com/sulincix/1aeeb6ff99ea14e1ff76afc301b25785

## Brute force engelleme
fail2ban kullanın
```bash
$ apt install fail2ban
$ service fail2ban start
```

Ayrıca `iptables` kullanmayı öğrenin.

## Zram ayarlayın
Zram ramin bir kısmını şifrelediği için daha zor dolmasını sağlar.
```bash
$ apt install zram-tools
# Varsayılan ayarları kurcalamaya gerek yok :D
$ service zramswap start
```
