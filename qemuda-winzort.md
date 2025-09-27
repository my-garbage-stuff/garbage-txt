## QEMU ile Winzort Kurulumu ve Ayarlanması 🖥️

Winzort işletim sistemini QEMU üzerinde kurmak için aşağıdaki adımları takip edebilirsiniz. Bu rehber, adım adım ilerleyerek gerekli hazırlıkları ve ayarları içermektedir.

---

## Hazırlık 🛠️

1. **QEMU Kurulumu**: Eğer QEMU yüklü değilse, Debian tabanlı bir sistemde aşağıdaki komutu kullanarak kurabilirsiniz:
   ```sh
   sudo apt install qemu-kvm
   ```

2. **Boş Alan Ayırma**: En az **50 GB** boş alan ayırın. Bu, Winzort'un düzgün çalışması için idealdir. 💾

3. **Winzort ISO İndirme**: Winzort'un LTSC sürümünü [indirin](https://archive.org/details/windows_11_enterprise_ltsc_2024). Bu sürüm, gereksiz bileşenlerden kaçınmanıza yardımcı olur. 📥

4. **Besmele Çekme**: Kurulum öncesi bir motivasyon kaynağı olarak besmele çekin. 🙏

---

## Disk İmajı Hazırlanması 💽

Disk imajı oluşturmak için aşağıdaki komutu kullanın:
```sh
# Disk imajı oluşturun
qemu-img create -f qcow2 ~/winzort.qcow 50G
```

---

## Winzort Kurulumu 🏗️

Winzort kurulumunu başlatmak için aşağıdaki komutu kullanın. Bu komut, ISO dosyasını ve disk imajını göstererek internetsiz bir kurulum yapmanızı sağlar. UEFI kurulumunu gerçekleştirmek için OVMF dosyasını indirmeniz gerekecek.

```sh
# OVMF.fd dosyasını indirin
# https://github.com/clearlinux/common/blob/master/OVMF.fd
qemu-system-x86_64 --enable-kvm -m 8G -smp `nproc` \
  -cpu host \
  -cdrom ~/Downloads/en-us_windows_10_consumer_editions_version_22h2_updated_feb_2023_x64_dvd_c29e4bb3.iso \
  -hda ~/winzort.qcow \
  -net none \
  -bios ~/OVMF.fd \
  -usbdevice tablet
```
Kurulum sırasında diskin tamamını kullanın ve oturum açmayın. 🚫

Kurulum tamamlandıktan sonra `-net none` parametresine artık ihtiyaç duymayacaksınız.

### TPM ve Secure Boot Hatası Çözümü

Winzort 11 kurulumunda TPM ve Secure Boot hatası alıyorsanız, aşağıdaki adımları izleyerek bu sorunu çözebilirsiniz. 🚀

1. **Kuruluma başlamadan önce** `Shift + F10` tuşlarına basın.
2. **Regedit** uygulamasını açın: `regedit.exe` yazın ve Enter'a basın.
3. **HKEY_LOCAL_MACHINE\SYSTEM\Setup** yoluna gidin.
4. Sağ tıklayıp **Yeni** kısmından **LabConfig** adında bir anahtar oluşturun ve içine girin.
5. **BypassTPMCheck**, **BypassSecureBootCheck**, **BypassRAMCheck** adında 3 tane **DWORD (32-bit)** oluşturun ve değerini **1** yapın.
6. Kuruluma devam edin. 🎉

---

## Virtio Yükleme 🚀

Virtio sürücülerini yüklemek, çözünürlük ve performans sorunlarını gidermek için önemlidir.

1. **Sanal Makineyi Kapatın**. 📴
2. **Virtio ISO'sunu İndirin**: Aşağıdaki bağlantıdan uygun sürümün ISO dosyasını indirin ve CD-ROM olarak bağlayın:
   [Virtio Sürücü İndirme](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/)
3. **Winzort'u Başlatın** ve sürücüleri ve misafir araçlarını yükleyin. 💻
4. **Sanal Makinayı Yeniden Başlatın**. 🔄

---

## Edge ve Defender'ı Kaldırma ❌

Winzort Defender, performansı olumsuz etkileyebilir. Bu nedenle kapatılması önerilir. Defender'ı kaldırmak için aşağıdaki bağlantıyı kullanabilirsiniz:
[Defender Sikici](https://github.com/ionuttbara/windows-defender-remover)

Ayrıca, Edge tarayıcısını kaldırıp Firefox yüklemek için şu bağlantıyı kullanabilirsiniz:
[Edge Sikici](https://github.com/ShadowWhisperer/Remove-MS-Edge)

---

## RDP Ayarlama 🔧

Winzort'un ayarlarından RDP (Uzak Masaüstü Protokolü) servisini açın. Ayarları bulmak için menülerde gezinin.

---

## Sanal Makinenin Nihai Parametreleri ile Başlatma 🎉

Sanal makinayı başlatmak için `run.sh` adında bir dosya oluşturun ve aşağıdaki komutları ekleyin:

```sh
qemu-system-x86_64 --enable-kvm -m 8G -smp `nproc` \
  -cpu host \
  -drive id=disk0,format=qcow2,file=winzort.qcow,cache=writeback,aio=native,cache.direct=on \
  -rtc base=localtime \
  -bios ~/OVMF.fd \
  -vga virtio \
  -display none \
  -net user,hostfwd=tcp::3389-:3389 -net nic

```

Bağlanmak için [Remmina](https://remmina.org/) kullanabilirsiniz.

Sanal makinenizi başarıyla başlattıktan sonra, Winzort'un keyfini çıkarabilir ve ihtiyaçlarınıza göre özelleştirebilirsiniz.
Herhangi bir sorunla karşılaşırsanız, dökümantasyon veya topluluk forumlarından yardım almayı unutmayın. İyi çalışmalar! 🎊

## Dosya Paylaşımı 🚀

Ana makinada **rclone** kullanarak WebDAV sunucusu başlatabilirsiniz. Sanal makinanızın **10.0.2.2** IP adresi ile ana makinayla ağ bağlantısı bulunmaktadır.

```sh
# Debian tabanlı bir sistemde rclone yüklemek için:
sudo apt install rclone

# WebDAV sunucusunu başlatmak için:
rclone serve webdav --addr 127.0.0.1:8000 $HOME
# 127.0.0.1 yerine 0.0.0.0 yazarsanız, tüm IP adreslerinden bağlantı kabul edilir.
```

### Winzort'tan WebDAV'a Bağlanmak için: 🖥️
1. Bilgisayarınızı açın.
2. "Map Network Drive" tuşuna basın.
3. Adres kısmına `http://10.0.2.2:8000` yazın.
4. Kaydedin ve bağlantınızı oluşturun! 🎉

Artık dosyalarınıza kolayca erişebilir ve paylaşabilirsiniz! 😊


## USB Bağlama 🔌

1. Önce USB'yi takın. 💻
   - Örneğin, bir USB bellek veya harici bir disk takabilirsiniz.

2. `lsusb` komutunu kullanarak vendor ve product ID'sini bulun. 🆔
   - Terminalde şu komutu çalıştırın:
     ```
     lsusb
     ```
   - Çıktı örneği:
     ```
     Bus 002 Device 003: ID 3131:6969 Example Corp. USB Device
     ```
   - Burada **vendor ID** `0x3131` ve **product ID** `0x6969`'dir.

3. QEMU'ya aşağıdaki parametreyi ekleyin: 
   ```
   -device qemu-xhci,id=xhci -device usb-host,vendorid=0x3131,productid=0x6969
   ```
   - Bu parametre, USB cihazınızı QEMU sanal makinesine bağlamak için kullanılır. 🛠️
