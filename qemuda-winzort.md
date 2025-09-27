# Qemu ile winzort kurulumu ve ayarlanması
## Hazırlık
1- qemu yoksa kurun (debian kullanıyorsan `apt install qemu-kvm`)

2- boşta alan ayırın (50gb ve üstü ideal)

3- winzort isosu indirin (ltsc olsa daha iyi olur. daha az çöple uğraşırsınız)

3- besmele çekin

## Disk imajı hazırlanması
```sh
# disk imajı oluşturun
qemu-img create -f qcow2 ~/winzort.qcow 50G
```
## Winzort kurulumu
```sh
# iso dosyasını ve disk imajını gösterip internetsiz kurulum yapalım.
# uefi kurulum yapalım.
# OVMF.fd yoksa indirin: https://github.com/clearlinux/common/blob/master/OVMF.fd
qemu-system-x86_64 --enable-kvm -m 8G -smp `nproc`\
  -cdrom Downloads/en-us_windows_10_consumer_editions_version_22h2_updated_feb_2023_x64_dvd_c29e4bb3.iso \
  -hda ~/winzort.qcow \
  -net none \
  -bios ~/OVMF.fd
# kurarken diskin tamanını kullanın. Oturum falan açmayın.
```
