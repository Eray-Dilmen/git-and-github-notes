## Git Nedir?

Git, bir versiyon kontrol sistemidir. Temel olarak projelerde checkpoint yapmaya yarar. Belli bir şeyi yaptıktan sonra kaydedip, sonrasında hata vs. yaparsak geri dönebilmemizi sağlayacak bir kaydedilmiş nokta oluşturulmasını sağlıyor.

## Terminal Komutları

* **pwd** (Print Working Directory): Şu anda bulunduğunuz dizini gösterir.
* **ls** (List): Bulunduğunuz konumdaki tüm dosyaları ve klasörleri listeler.
* **cd** (Change Directory): Gitmek istediğiniz dizini ayarlamanızı sağlar.
* **clear**: Terminal ekranını temizler.

`/` işareti ile alt klasörlere (subfile/subdirectory) gidebiliyoruz. Örnek: `cd Desktop/Python` klasörüne erişmek gibi.
Geri çıkmak (bir üst klasöre geçmek) için: `cd ..` kullanılır.

**Hata: Klasör adının boşluk içermesi**
Terminalde `cd Python Stuffs` yazdığında bunu tek bir klasör adı olarak anlamaz; "Python" ve "Stuffs"ı iki ayrı parametre sanır.
Doğru kullanım: `cd "Python Stuffs"`

## Dosya ve Klasör İşlemleri

* **mkdir** (make directory): Klasör oluşturma komutudur. `mkdir GitKursu` dediğimiz zaman şu an olduğumuz bölgede GitKursu adında bir klasör oluşturur.
* **touch**: Dosya oluşturur. `cd GitKursu` diyerek içine girdikten sonra `touch not.txt` ifadesi ile not.txt dosyası oluşturulur.
* **rm** (remove): Dosya silme komutudur. `rm not.txt` bulunduğumuz directory'de 'not.txt' adlı dosyanın silinmesini sağlar.

**Peki klasörü nasıl sileceğiz?**
`rm` sadece dosyaları siler, fakat `rm -rf` yazarsak klasör de siliniyor. Fakat bunu yapabilmek için o klasörün bulunduğu lokasyonda olman lazım (içindeyken silemezsin). Klasörle beraber içindeki her şeyi silmek için klasör lokasyonunda `rm -rf` diyebiliriz.

Komutun sözdizimi ve çalışma mantığı:
* `rm` (remove): Silme komutudur.
* `-r` (recursive): Belirtilen klasörü ve içindeki tüm alt klasörleri, dosyaları derinlemesine siler.
* `-f` (force): Yazma korumalı dosyalarda onay sormaz ve dosya yoksa hata vermeden işlemi zorlar.

```bash
rm -rf KlasorAdi
```

## Git Kullanıcı Adı Yapılandırması

Git üzerinde tüm projelerde geçerli olacak kullanıcı adını ayarlamak ve kontrol etmek için kullanılır:

```bash
# Global kullanıcı adını belirleme
git config --global user.name "Ad Soyad"

# Mevcut kullanıcı adını kontrol etme
git config user.name
```

## Git E-posta Yapılandırması

Git üzerinde tüm projelerde geçerli olacak e-posta adresini ayarlamak ve kontrol etmek için kullanılır:

```bash
# Global e-posta adresini belirleme
git config --global user.email "ornek@gmail.com"

# Mevcut e-posta adresini kontrol etme
git config user.email
```
