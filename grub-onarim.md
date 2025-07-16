# Grub Nasıl Elle Kurulur/Onarılır

Eğer kurulum aşamasında "grub yüklenemedi" gibi bir hata aldıysanız ve sorun detaylı olarak yazmıyorsa, grubu elle yüklemeyi deneyerek hatayı daha detaylı bulabilirsiniz.

![Grub Kurulum](https://github.com/user-attachments/assets/4dabe90e-d115-441b-b7ba-c4b3422f07a9)

Öncelikle sistemi bu haliyle kurduktan sonra bir terminal açalım ve sistemin kurulu olduğu diski tespit edelim.

![Disk Tespiti](https://github.com/user-attachments/assets/fe87c7b8-9aae-4ba7-82f4-0c818551591b)

Buradaki örneğimizde `/dev/sda1` diskimiz EFI bölümü, `/dev/sda2` bölümümüz ise kök dizin. (Sizde farklı olabilir)

Öncelikle bir dizine diskimizi bağlayalım.

![Disk Bağlama](https://github.com/user-attachments/assets/bdf34bee-2465-45da-8049-69fc11cc642b)

Ardından kök dizinin içine girelim ve grub yükleyelim.

![Grub Yükleme](https://github.com/user-attachments/assets/bf201552-2947-4a2b-aafe-5d1a917cc4ff)

Bu noktada yukarıdaki gibi bir çıktı aldıysanız, sorunsuz şekilde yüklenmiş demektir.

İşlem bittikten sonra aşağıdaki gibi diski ayırabilirsiniz.

![Disk Ayırma](https://github.com/user-attachments/assets/e1556bd7-1b9a-41ae-8380-3e23436a0199)

## Olası Çözümler

1. **Kök dizinin grub tarafından okunabilir bir yerde olduğundan emin olun.** Örneğin, grub LVM üzerine kurulamaz. Eğer kök dizini grub tarafından okunamayacak bir şekilde kullanıyorsanız, ayrı bir `/boot` bölümü oluşturun.

2. **Eski kayıtları silin.**

   ![Eski Kayıtları Silme](https://github.com/user-attachments/assets/ce5423fa-4fcc-409e-9941-c99ff155e416)

   Ardından tekrar kurmaya çalışın.

3. **Eğer işe yaramıyorsa, EFI bölümündeki `grubx64.efi` dosyasını kopyalayın.**

   ![Grub EFI Dosyası](https://github.com/user-attachments/assets/b0669f32-a0a4-4fa9-b825-de60efe08d64)

4. **Eğer hâlâ işe yaramıyorsa ve bilgisayarın Windows'tan başka bir şey boot etmediğini düşünüyorsanız, EFI dosyasını Windows'un dosyasının olması gereken konum ile değiştirin.**

   ![EFI Dosyası Değiştirme](https://github.com/user-attachments/assets/9ebcadc3-1b74-40a6-aa85-f33e00ec8f57)

5. **Bu da işe yaramıyorsa, bilgisayarınızı çöpe atın ve adam akıllı bir bilgisayar alın.** :D 

Bu adımları takip ederek grub kurulumunu veya onarımını gerçekleştirebilirsiniz.
