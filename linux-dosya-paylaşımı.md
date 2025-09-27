## Linux Dağıtımlarında Dosya Paylaşımı (WebDAV ile)

Bu yazıda, Linux dağıtımları üzerinden WebDAV kullanarak dosya paylaşımının nasıl yapılacağını adım adım anlatacağız.

### 1. Rclone Yükleyelim

Rclone, bulut depolama ve dosya senkronizasyonu için kullanılan bir araçtır. Öncelikle, sisteminize Rclone'u yüklemeniz gerekiyor. Eğer Debian tabanlı bir dağıtım kullanıyorsanız, aşağıdaki komutu terminalde çalıştırarak Rclone'u yükleyebilirsiniz:

```sh
sudo apt install rclone
```

### 2. Paylaşıma Açılacak Yer İçin Servis Ayarlayalım

Rclone ile WebDAV paylaşımı yapmak için bir yapılandırma dosyası oluşturmalısınız. Aşağıdaki adımları izleyerek gerekli ayarları yapabilirsiniz:

- Rclone yapılandırma dosyasını oluşturun:

```sh
rclone config
```

- Bu komut, sizi bir dizi menüye yönlendirecek. Burada yeni bir "remote" (uzak bağlantı) oluşturmak için "n" tuşuna basın.
- Paylaşım için bir isim verin (örneğin, `webdav`).
- "Storage" seçeneği olarak "WebDAV" seçin.
- WebDAV sunucunuzun URL'sini, kullanıcı adını ve şifresini girin.

### 3. Servisi Etkinleştirelim

Rclone ile oluşturduğunuz paylaşımı bir sistem servisi olarak çalıştırmak için aşağıdaki adımları izleyin:

- Rclone için bir sistem servisi dosyası oluşturun. Aşağıdaki örneği kullanarak `/etc/systemd/system/rclone-share.service` dosyasını oluşturun:

```ini
[Unit]
Description=Rclone WebDAV Share
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone serve webdav webdav: --addr 0.0.0.0:8080
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

- Servisi etkinleştirin ve başlatın:

```sh
# systemd kullanıyorsanız
sudo systemctl enable rclone-share.service
sudo systemctl start rclone-share.service
```

### 4. Paylaşımı Test Edelim

WebDAV paylaşımınızın çalışıp çalışmadığını test etmek için bir WebDAV istemcisi veya tarayıcı kullanarak `http://<sunucu_ip>:8080` adresine gidin. Eğer her şey doğru ayarlandıysa, dosya paylaşımınıza erişebilmelisiniz.

Bu adımları takip ederek Linux dağıtımınızda WebDAV ile dosya paylaşımını başarıyla gerçekleştirebilirsiniz.

## Bağlantının Eklenmesi

Linux üzerinde WebDAV bağlantısını eklemek için birkaç farklı yöntem bulunmaktadır. Aşağıda, hem dosya yöneticisi hem de terminal üzerinden nasıl bağlantı ekleyeceğinizi açıklayacağım.

### 1. Linux Dosya Yöneticisi ile Bağlantı Ekleme

Linux dosya yöneticinizin adres çubuğuna gidin ve aşağıdaki formatta bağlantıyı girin:

```
dav://<sunucu_ip>:8080
```

Bu adresi girdikten sonra **Enter** tuşuna basın. Bağlantı başarıyla eklenmiş olacaktır ve dosya yöneticiniz üzerinden paylaşıma erişebilirsiniz.

### 2. Terminal Üzerinden Bağlantı Ekleme

Terminal üzerinden WebDAV bağlantısını eklemek için `gio` komutunu kullanabilirsiniz. Aşağıdaki komutu terminalde çalıştırın:

```sh
gio mount dav://<sunucu_ip>:8080
```

Bu komut, belirtilen WebDAV bağlantısını sisteminize ekleyecektir.

### 3. Windows Tarafında Bağlantı Ekleme

Windows işletim sisteminde WebDAV bağlantısını eklemek için şu adımları izleyin:

1. **Bilgisayarım** (veya **Bu Bilgisayar**) klasörünü açın.
2. Üst menüde bulunan **Ağ sürücüsü bağla** (Map network drive) seçeneğine tıklayın.
3. Açılan pencerede **Klasör** kısmına aşağıdaki adresi yazın:

```
http://<sunucu_ip>:8080
```

4. Ardından **Tamam** butonuna basın.

Bu adımları takip ederek, Windows işletim sisteminde de WebDAV bağlantısını başarıyla ekleyebilirsiniz. Herhangi bir sorunla karşılaşırsanız, bağlantı ayarlarınızı ve ağ bağlantınızı kontrol etmeyi unutmayın.

