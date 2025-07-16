# Grub nasıl elle kurulur/Onarılır

Eğer kurulum aşamasında grub yüklenemedi gibi bir hata aldışsanız ve sorun detaylı olarak yazmıyorsa grubu elle yüklemeyi deneyerek hatayı daha detaylı bulabiliriz.

![photo_2025-07-16_13-57-19](https://github.com/user-attachments/assets/4dabe90e-d115-441b-b7ba-c4b3422f07a9)

Şimdi öncelikle sistemi bu hali ile kurduktan sonra bir terminal açalım ve sistemin kurulu olduğu diski tespit edelim.

<img width="289" height="158" alt="Screenshot from 2025-07-16 14-21-25" src="https://github.com/user-attachments/assets/fe87c7b8-9aae-4ba7-82f4-0c818551591b" />

Buradaki örneğimizde /dev/sda1 diskimiz efi bölümü /dev/sda2 bölümümüz kök dizin. (sizde farklı olabilir)

Öncelikle bir dizine diskimizi bağlayalım.

<img width="663" height="134" alt="Screenshot from 2025-07-16 14-23-28" src="https://github.com/user-attachments/assets/bdf34bee-2465-45da-8049-69fc11cc642b" />

Ardından kök dizinin içine girelim. ve grub yükleyelim.

<img width="809" height="291" alt="Screenshot from 2025-07-16 14-25-00" src="https://github.com/user-attachments/assets/bf201552-2947-4a2b-aafe-5d1a917cc4ff" />

Bu noktada yukarıdaki gibi bir çıktı aldıysanız sorunsuz şekilde yüklenmiş demektir. 

İşlem bittikten sonra aşağıdaki gibi diski ayırabilirsiniz.


## Olası çözümler

Kök dizinin grub tarafından okunabilir bir yerde olduğundan emin olun. Örneğin grub lvm üzerine kurulamaz. Eğer kök dizini grub tarafından okunamayacak bir şekilde kullancaksanız ayrı bir /boot bölümü oluşturun. 

Eski kayıtları silin.

<img width="459" height="410" alt="Screenshot from 2025-07-16 14-27-15" src="https://github.com/user-attachments/assets/ce5423fa-4fcc-409e-9941-c99ff155e416" />

Ardından tekrar kurmaya çalışın.

Eğer işe yaramıyorsa efi bölümündeki grubx64.efi dosyasını kopyalayın.

<img width="704" height="73" alt="Screenshot from 2025-07-16 14-29-31" src="https://github.com/user-attachments/assets/b0669f32-a0a4-4fa9-b825-de60efe08d64" />

Eğer halen daha işe yaramıyorsa ve bilgisayarın windowstan başka bir şey boot etmediğini düşünüyorsanız efi dosyasını windowsun dosyasının olması gereken konum ile değiştirin. 

<img width="794" height="61" alt="Screenshot from 2025-07-16 14-31-43" src="https://github.com/user-attachments/assets/9ebcadc3-1b74-40a6-aa85-f33e00ec8f57" />

Bu da işe yaramıyorsa bilgisayarınızı çöpe atın ve adam akıllı bir bilgisayar alın :D 





