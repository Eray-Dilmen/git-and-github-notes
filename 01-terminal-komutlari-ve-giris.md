## Git Nedir?

Git, bir versiyon kontrol sistemidir[cite: 7]. Temel olarak projelerde checkpoint yapmaya yarar[cite: 7]. Belli bir şeyi yaptıktan sonra kaydedip, sonrasında hata vs. yaparsak geri dönebilmemizi sağlayacak bir kaydedilmiş nokta oluşturulmasını sağlıyor[cite: 7].

## Terminal Komutları

* **pwd** (Print Working Directory): Şu anda bulunduğunuz dizini gösterir[cite: 7].
* **ls** (List): Bulunduğunuz konumdaki tüm dosyaları ve klasörleri listeler[cite: 7].
* **cd** (Change Directory): Gitmek istediğiniz dizini ayarlamanızı sağlar[cite: 7].
* **clear**: Terminal ekranını temizler[cite: 7].

`/` işareti ile alt klasörlere (subfile/subdirectory) gidebiliyoruz[cite: 7]. Örnek: `cd Desktop/Python` klasörüne erişmek gibi[cite: 7].
Geri çıkmak (bir üst klasöre geçmek) için: `cd ..` kullanılır[cite: 7].

**Hata: Klasör adının boşluk içermesi**
Terminalde `cd Python Stuffs` yazdığında bunu tek bir klasör adı olarak anlamaz; "Python" ve "Stuffs"ı iki ayrı parametre sanır[cite: 7].
Doğru kullanım: `cd "Python Stuffs"`[cite: 7]

## Dosya ve Klasör İşlemleri

* **mkdir** (make directory): Klasör oluşturma komutudur[cite: 7]. `mkdir GitKursu` dediğimiz zaman şu an olduğumuz bölgede GitKursu adında bir klasör oluşturur[cite: 7].
* **touch**: Dosya oluşturur[cite: 7]. `cd GitKursu` diyerek içine girdikten sonra `touch not.txt` ifadesi ile not.txt dosyası oluşturulur[cite: 7].
* **rm** (remove): Dosya silme komutudur[cite: 7]. `rm not.txt` bulunduğumuz directory'de 'not.txt' adlı dosyanın silinmesini sağlar[cite: 7].

**Peki klasörü nasıl sileceğiz?**
`rm` sadece dosyaları siler, fakat `rm -rf` yazarsak klasör de siliniyor[cite: 7]. Fakat bunu yapabilmek için o klasörün bulunduğu lokasyonda olman lazım (içindeyken silemezsin)[cite: 7]. Klasörle beraber içindeki her şeyi silmek için klasör lokasyonunda `rm -rf` diyebiliriz[cite: 7].

Komutun sözdizimi ve çalışma mantığı:
* `rm` (remove): Silme komutudur[cite: 7].
* `-r` (recursive): Belirtilen klasörü ve içindeki tüm alt klasörleri, dosyaları derinlemesine siler[cite: 7].
* `-f` (force): Yazma korumalı dosyalarda onay sormaz ve dosya yoksa hata vermeden işlemi zorlar[cite: 7].

```powershell
rm -rf KlasorAdi
```

## Git Kullanıcı Adı Yapılandırması

Git üzerinde tüm projelerde geçerli olacak kullanıcı adını ayarlamak ve kontrol etmek için kullanılır[cite: 7]:

```powershell
# Global kullanıcı adını belirleme
git config --global user.name "Ad Soyad"

# Mevcut kullanıcı adını kontrol etme
git config user.name
```

## Git E-posta Yapılandırması

Git üzerinde tüm projelerde geçerli olacak e-posta adresini ayarlamak ve kontrol etmek için kullanılır[cite: 7]:

```powershell
# Global e-posta adresini belirleme
git config --global user.email "ornek@gmail.com"

# Mevcut e-posta adresini kontrol etme
git config user.email
```