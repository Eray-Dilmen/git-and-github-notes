## `git log diyince HEAD -> main` ifadesindeki HEAD nedir ?

`HEAD`, Git içerisinde o an projenin neresinde (hangi branch'te veya hangi commit'te) olduğumuzu gösteren aktif bir işaretçidir (pointer).

* `HEAD -> main` ifadesi, şu anda `main` branch'inde bulunduğumuzu ve çalışma dizinimizin `main` dalındaki en son commit'e baktığını gösterir.
* Eski bir commit'e geçiş yaptığımızda (`git checkout <hash>`) veya farklı bir branch'e geçtiğimizde `HEAD` de bizimle birlikte o konuma taşınır.
* Kısacası `HEAD`, "Şu anda buradasın" anlamına gelen dinamik bir konum göstergesidir.

## Branch (Dal) Mantığı ve Yeni Bir Proje Üzerinde Uygulama

Projeye yeni bir özellik eklerken veya deneysel çalışmalar yaparken ana koda (`main`) doğrudan dokunmak risklidir. Bunun yerine yeni bir dal (branch) açılır, geliştirmeler orada tamamlanır, test edilir ve sorunsuzsa ana dala (`merge`) birleştirilir.

### Adım Adım Uygulama:

Öncelikle önceki projeden çıkıp aynı dizinde `Kitap` adında yeni bir proje klasörü oluşturuyoruz:

```bash
mkdir Kitap
cd Kitap
```

İlk olarak `git status` ile dizinin zaten bir Git deposu olup olmadığını kontrol ediyoruz. Eğer başlatılmamışsa `git init` komutunu çalıştırıyoruz.

> **Not:** Bir klasörde zaten Git başlatılmışsa tekrar `git init` yapmak yapıyı bozmaz (var olan depoyu yeniden başlatır) ancak iç içe depolar oluşturmamak veya yanlışlıkla üst dizinlerde depo açıp çakışmalara yol açmamak için önce `git status` ile kontrol etmek en sağlıklı yöntemdir.

Proje içinde `ilkbolum.txt` adında `touch [dosya ismi]` komutu ile bir dosya oluşturup içine metin yazıyoruz ve commit atıyoruz:

```bash
touch ilkbolum.txt
git add ilkbolum.txt
git commit -m "feat: ilk bolum eklendi"
```

Mevcut branch'leri listelemek için `git branch` komutunu kullanıyoruz:

```bash
git branch
```

Çıktı:

```plaintext
* main
```

Şu anda sadece `main` branch'i bulunuyor. Baştaki `*` işareti (ve yeşil renk), aktif olarak o branch üzerinde olduğumuzu gösterir.

### Yeni Branch Açma ve Geçiş Yapma

Yeni bir özellik geliştirmek için `git branch <dal_adi>` komutuyla yeni bir dal açıyoruz (burada dal adı olarak `feature` verdik):

```bash
git branch feature
```

Tekrar `git branch` yazarak dalları kontrol ediyoruz:

```plaintext
  feature
* main
```

Görüldüğü üzere artık 2 tane branch'imiz var ve şu an hala `main` branch'indeyiz.

Açtığımız `feature` dalına geçmek için `git switch feature` (veya `git checkout feature`) komutunu çalıştırıyoruz:

```bash
git switch feature
```

Tekrar `git branch` ile aktif dalımızı kontrol ediyoruz:

```plaintext
* feature
  main
```

Görüldüğü üzere `*` işareti `feature` dalına geçti ve artık bu dal üzerinde çalışıyoruz.

---

git log çalıştıralım: 

```text
commit 64091972467312ae25bbddab506ee05a2512cd68 (HEAD -> feature, main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı

```

görüldüğü üzere HEAD şu an hem feature, hem main'de duruyor. Daha feautureyi hareket ettirmedik
(Komutu çalıştırdığın anda main dalı hangi commit'te duruyorsa, feature dalı da tam o noktayı ve geçmişini işaret eden birebir kopyası olarak başlar)


şimdi feature'da olduğumuz için
yeni bir dosya açtık deneysel.txt

içine 1 satır yazı yazdık ve commit mesajına 
'ilk satır deneyseller yazıldı.'
dedik 
commit ettikten sonra 'git log' ile duruma bakalım:

```text

commit a1b2d769cdf0b9474eb3827ca925c397780652b9 (HEAD -> feature)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:10:32 2026 +0300

    ilk satır deneyseller yazıldı.

commit 64091972467312ae25bbddab506ee05a2512cd68 (main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı
```

HEAD yani şu an bulunduğumuz yer -> feature branch'inde 
main ise aşağıda yani bir önceki işlemde kaldı

Tekrar bir satır ekleyip commit'leyelim:

```text
commit 963b7c70fefa73f8472e04f02e2d9d116994a714 (HEAD -> feature)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:13:05 2026 +0300

    ikinci deneysel satır

commit a1b2d769cdf0b9474eb3827ca925c397780652b9
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:10:32 2026 +0300

    ilk satır deneyseller yazıldı.

commit 64091972467312ae25bbddab506ee05a2512cd68 (main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı
```

> **Not (`git log` Ekranında Gezinme):** Commit geçmişi ekran boyunu aşıp uzadığında terminal bir sayfalayıcı (pager) açar. Aşağı-yukarı gezinmek için yön tuşlarını veya `Enter`/`Space` tuşlarını kullanabilirsiniz. Log ekranından çıkıp normal terminale dönmek için `q` tuşuna basmanız gerekir; siz `q` tuşuna basana kadar komut satırına geri dönemezsiniz.

### Branch'ler Arası Dosya Yalıtımı

Bu noktada `git switch main` komutunu çalıştırırsak, `deneysel.txt` dosyası IDE/kod editörü ekranımızdan ve klasörümüzden kaybolur; çünkü o dosya sadece `feature` dalında oluşturulmuş ve commit'lenmiştir.

Tekrar `git switch feature` yazdığımızda, Git çalışma alanımızı o dala günceller ve `deneysel.txt` dosyası geri gelir.

Ardından `main` dalındayken 2 sefer `ilkbolum.txt` dosyasına yeni satırlar ekleyip commit'ler atıyoruz.

`main` dalında atılan commit'ler sonrası log geçmişi:

```text
commit d15b260cb1cabb9451083361970d107af65bbef6 (HEAD -> main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:21:09 2026 +0300

    son commit satırı eklendi

commit 243d2d474f1511d3f6d83856d3dcf049116e0b52
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:20:47 2026 +0300

    bugün öğrenilenler yazıldı

commit 64091972467312ae25bbddab506ee05a2512cd68
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı
```

`main` dalına son 2 commit eklendi. Şimdi `git switch feature` komutu ile tekrar `feature` dalına dönüyoruz.

Bu dala geçtiğimizde, `main` üzerinde atılan son 2 commit mesajını burada göremeyiz. Hatta `ilkbolum.txt` dosyasına `main` dalındayken eklediğimiz o satırlar da burada tamamen yokmuş (hiç yazılmamış) gibi görünür.

`feature` dalındayken `git log` çıktısı:

```text
commit 963b7c70fefa73f8472e04f02e2d9d116994a714 (HEAD -> feature)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:13:05 2026 +0300

    ikinci deneysel satır

commit a1b2d769cdf0b9474eb3827ca925c397780652b9
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:10:32 2026 +0300

    ilk satır deneyseller yazıldı.

commit 64091972467312ae25bbddab506ee05a2512cd68
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı
```

Görüldüğü üzere bu dalda en son atılan deneysel commit'ler yer alıyor. Hangi branch'teyseniz IDE/dosya yöneticisi de o branch'in dosya durumunu gösterir; bu yüzden `feature` dalındayken `main` dalındaki değişiklikler görünmez.

## Branch Birleştirme (Merge)

İki farklı dalı birleştirmek için hangi dalı hedef dala dahil edeceksek onu komutta belirtmemiz gerekir. Biz `feature` dalındaki değişiklikleri `main` dalına aktaracağımız için komutumuz `git merge feature` olacaktır.

Birleştirme işlemine başlamadan önce hedef dala geçmemiz gerekir:

```bash
git switch main
```

(`main` dalına geçtiğimiz anda, daha önce `ilkbolum.txt` dosyasına eklediğimiz değişiklikler tekrar geri gelir.)

Şimdi birleştirme komutunu çalıştırıyoruz:

```bash
git merge feature
```

Komutu çalıştırdığımızda Vim editörü açılarak karşımıza şu ekran gelir:

```text
Merge branch 'feature'
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

En baştaki `#` işareti olmayan `Merge branch 'feature'` yazısı varsayılan merge commit mesajımızdır. Genellikle standart olarak bu mesaj bırakılır. Değişiklik yapmadan çıkmak için sırasıyla `Esc` tuşuna basıp ardından `:wq` yazıp `Enter`lıyoruz.

**Vim Çalışma Mantığı:**
Vim iki temel modla çalışır: **Yazma (Insert)** modu ve **Komut (Normal)** modu.

* **Neden önce `Esc` bastık:** Yazı yazma modundan çıkıp Vim'e işlem yaptıracağımız **komut moduna** geçmek için. (`Esc` basmadan doğrudan `:wq` yazılırsa bunu komut olarak algılamaz, commit mesajının içine düz metin olarak yazar.)
* **Neden `:wq` yazdık:**
  * `w` **(write):** Dosyayı kaydet (*yaz*) anlamına gelir.
  * `q` **(quit):** Editörden çık anlamına gelir.
  * `:wq` : *"Değişiklikleri kaydet ve editörü kapatarak terminale dön"* komutudur.

Kaydedip çıktıktan sonra terminalde merge işleminin tamamlandığına dair şu çıktı görüntülenir:

```text
Merge made by the 'ort' strategy.
 deneysel.txt | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 deneysel.txt
```

Merge işleminden sonra `git log` çıktısını kontrol edelim:

```text
commit 8e4ad1d3ed348b692513f350b13b4681a960925f (HEAD -> main)
Merge: d15b260 963b7c7
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:29:55 2026 +0300

    Merge branch 'feature'

commit d15b260cb1cabb9451083361970d107af65bbef6
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:21:09 2026 +0300

    son commit satırı eklendi

commit 243d2d474f1511d3f6d83856d3dcf049116e0b52
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:20:47 2026 +0300

    bugün öğrenilenler yazıldı

commit 963b7c70fefa73f8472e04f02e2d9d116994a714 (feature)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:13:05 2026 +0300

    ikinci deneysel satır

commit a1b2d769cdf0b9474eb3827ca925c397780652b9
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:10:32 2026 +0300

    ilk satır deneyseller yazıldı.

commit 64091972467312ae25bbddab506ee05a2512cd68
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:00:45 2026 +0300

    ilk satırlar yazıldı

commit 174538be31b32f8b8a3d7a118e2f89cee2f33afc
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 17:55:47 2026 +0300

    ilk bolum başlığı yazıldı
```

Log geçmişinde görüldüğü üzere:
* `feature` dalında atılan deneysel commit'ler geçmişe dahil edilmiştir.
* En üstte ise bu iki dalın birleştiğini belirten `Merge branch 'feature'` commiti yer almaktadır.

Böylece iki farklı dal başarıyla tek bir kolda birleştirilmiştir.

---

## Çakışma (Conflict) ve Fast-Forward Kavramları

Önceki örneklerde iki dalda farklı dosyalar üzerinde çalıştık. Ancak iki farklı dalda aynı dosyanın aynı satırları üzerinde değişiklik, silme veya ekleme yapılsaydı **Conflict (Çakışma)** meydana gelirdi ve Git birleştirmeyi otomatik yapamayıp bizden hangi kodun kalacağını seçmemizi isterdi.

### Fast-Forward Merge Nedir?

Eğer yeni bir dal açıldıktan sonra `main` (ana) dalda **hiçbir yeni commit atılmamışsa** ve değişiklikler sadece açılan yeni dalda yapılmışsa; bu dalı `main` ile birleştirdiğimizde Git yeni bir merge commit'i oluşturmaz. `main` dalının işaretçisini doğrudan yeni dalın son commit'ine taşır (ileriye sarar). Buna **Fast-forward** denir (teknik mülakatlarda sıkça sorulan bir kavramdır).

### Fast-Forward Uygulaması:

`arkakapak` adında yeni bir branch oluşturup geçiş yapıyoruz:

```bash
git branch arkakapak
git switch arkakapak
```

Bu dalda `kapakyazisi.txt` dosyasını oluşturup 2 kez metin ekleyerek 2 ayrı commit atıyoruz:

1. `git commit -m "arka kapak ilk satır yazıldı"`
2. `git commit -m "arka kapak yazısı güçlendirildi"`

Bu süreçte `main` dalında hiçbir yeni commit atmadık. Ardından `main` dalına dönüp `arkakapak` dalını birleştiriyoruz:

```bash
git switch main
git merge arkakapak
```

Terminal çıktısı:

```text
Updating 8e4ad1d..8475223
Fast-forward
 kapakyazisi.txt | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 kapakyazisi.txt
```

Görüldüğü üzere çıktıda `Fast-forward` yazar ve bizden yeni bir merge commit mesajı istemez. Sadece ek olarak gelen özellikler `main` dalına doğrudan aktarılır.

Birleştirme sonrası `git log` çıktısı:

```text
commit 8475223d339937e405a625efef9164662517118e (HEAD -> main, arkakapak)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 19:46:48 2026 +0300

    arka kapak yazısı güçlendirildi

commit f2499c9c2c5a7cd22ebea485d40231ce70838545
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 19:46:09 2026 +0300

    arka kapak ilk satır yazıldı

commit 8e4ad1d3ed348b692513f350b13b4681a960925f
Merge: d15b260 963b7c7
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 18:29:55 2026 +0300

    Merge branch 'feature'
```

`arkakapak` dalında atılan commit'ler doğrudan `main` geçmişine dahil oldu. Artık `HEAD` hem `main` hem de `arkakapak` dallarının en son commit'inde durmaktadır (`HEAD -> main, arkakapak`).

---

## Merge Sırasında Çakışma (Conflict) Hataları ve Çözümü

Şimdi merge işlemlerinde karşımıza çıkabilecek çakışma (conflict) senaryolarına bakacağız. Daha önce sorunsuz birleştirdiğimiz örnekleri bu kez bilerek çakışma yaratarak deneyelim ve nasıl sorunlarla karşı karşıya kaldığımızı görelim.

Kafa karışıklığı olmasın diye işlemleri tamamen yeni ve farklı bir proje üzerinden yürüteceğiz.

Her zaman olduğu gibi ilk yapacağımız şey `git status` ile dizini kontrol etmek, eğer Git başlatılmamışsa `git init` ile depoyu hazır hale getirmektir.

### Adım Adım Çakışma Senaryosu:

1. `touch ilkbolum.txt` ile yeni dosya oluşturduk ve içerisine **x** `[1- java kitabımız çok güzel olacak. 3-bence okuyun.]` cümlelerini yazıp commit'ledik.
2. `arkakapak` adında bir branch oluşturup bu dala geçtik. `ilkbolum.txt` dosyasının içindeki **x** cümlesini silip yerine **y** `[1-arka kapak çok güzeldir]` cümlesini yazdık ve commit'ledik.
3. `main` branch'ine geri döndük. `main` dalında `ilkbolum.txt` içerisinde hala **x** duruyor. Burada `ilkbolum.txt` dosyasını tamamen sildik, yerine `bolum1.txt` adında yeni bir dosya oluşturup içine **z** `[1-java kitabımız çok güzel.]` metnini yazarak commit ettik.
4. `main` dalındayken `git merge arkakapak` komutunu çalıştırdık.

Bir tarafta (`main`) dosya tamamen silinmişken, diğer tarafta (`arkakapak`) aynı dosya düzenlendiği için Git otomatik birleştirme yapamadı ve şu çakışma uyarısını verdi:

```text
CONFLICT (modify/delete): ilkbolum.txt deleted in HEAD and modified in arkakapak. Version arkakapak of ilkbolum.txt left in tree.
Automatic merge failed; fix conflicts and then commit the result.
```

Bu aşamada `git status` ile durumu kontrol ettiğimizde:

```text
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add/rm <file>..." as appropriate to mark resolution)
    deleted by us:   ilkbolum.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`ilkbolum.txt` dosyasının unmerged (birleştirilememiş) olarak kaldığını görüyoruz; çünkü dosya `main` dalında silinmişken (`deleted by us`), `arkakapak` dalında düzenlenmişti (**y** yapılmıştı).

### Çakışmanın Çözülmesi ve Commit Edilmesi:

Çakışmayı manuel olarak çözüp kararı verdikten sonra dosyayı `git add .` ile staging alanına alıyoruz ve `git commit -m "merge conflict çözüldü"` mesajı ile kaydediyoruz.

Eğer bu aşamadan sonra tekrar `git merge arkakapak` komutunu çalıştırmayı denerseniz terminalde:

```text
Already up to date.
```

uyarısı alırsınız. Bunun anlamı, birleştirme işleminin zaten başarıyla tamamlandığı, `arkakapak` dalındaki tüm geçmişin `main` dalına dahil edildiği ve birleştirilecek yeni hiçbir değişikliğin kalmadığıdır.

`git branch` ile kontrol ettiğimizde hala `main` dalındayız.

`git log` ile geçmişe baktığımızda:

```text
commit 81f258dd75c0bdc5ffafaf5a7af319ea98dd16b9 (HEAD -> main)
Merge: 48fe652 2aa8456
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 20:04:57 2026 +0300

    merge conflict çözüldü

commit 48fe6527c4a5acd7c3036bba61752ef19b67e035
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 20:00:34 2026 +0300

    ilk bolum tekrar yazıldı

commit 2aa84566bc11d89a9a838ef6ed48caa9c70b228f (arkakapak)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 19:59:02 2026 +0300

    arka kapak yazıldı.

commit a1320a55bb1df56b66766370b7c3832def406241
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 19:57:50 2026 +0300

    giriş cümlesi yazıldı
```

"ilk bolum tekrar yazıldı" yani **y**'yi almış oldu. 

Final durumda:
* `arkakapak` branch'indeki `ilkbolum.txt` dosyasında **y** yazısı duruyor.
* `main` branch'indeki `bolum1.txt` dosyasında **z** yazısı duruyor.

**Peki biz burada y'nin üstüne yazılması için force'luyor muyuz?**  
Hayır; çakışma durumunda Git iki tarafı da korur, dosyaları silmez veya zorla ezmez. Çakışma çözüldükten sonra her iki branch'in değişiklikleri onaylanmış haliyle ortak geçmişe dahil edilir.

---

### Git Merge Çakışması: Silme ve Değiştirme Durumu

* **Çakışmanın Kaynağı:** Git, dosya içeriğinden ziyade dosyanın kendisini takip eder. Çakışma eski içerik ('x') ile yeni içerik ('y') arasında değil; `main` dalındaki **dosyayı silme** eylemi ile diğer daldaki **dosyayı değiştirme** eylemi arasında yaşanır. Dosya `main` dalında silindiğinde varlığı yok edilmiş olur.
* **Durum Raporu ("Deleted by us"):** Bu uyarı bir durum raporudur ve Git temelde şunu sorar: *"Bu dosya `main`'de silinmiş ama 2. dalda içi değiştirilmiş. Dosya silinmiş olarak mı kalsın, yoksa 2. daldaki haliyle var olmaya devam mı etsin?"*.
* **Çözüm Kararı:** Veri kaybını önlemek amacıyla Git, diğer daldaki yeni versiyonu ('y') çalışma klasörüne yerleştirir.
* **`git add .` Komutunun Etkisi:** Doğrudan `git add .` çalıştırıldığında, dosyanın silinmiş olması kararından vazgeçilmiş olur. Bu komut Git'e "klasörde şu an ne varsa onu nihai karar olarak kabul et" talimatını verir.
* **Sonuç:** İşlem sonucunda geri gelen şey silinen 'x' metni değil, dosyanın fiziken hayata dönmesi ve içindeki yeni 'y' metniyle birlikte klasöre geri gelmesidir.

---

## `git stash` Nedir?

Bir branch üzerinde çalışırken henüz commit atmaya hazır olmadığınız (yarım kalan) değişiklikler varken başka bir branch'e geçmek istediğinizde Git buna izin vermeyebilir veya değişiklikler diğer dala taşınabilir. Bu durumu çözmek için `git restore` yaparsak yaptığımız değişiklikler kalıcı olarak silinir ve kaybolur.

`git stash`, üzerinde çalıştığınız yarım kalmış değişiklikleri kaybetmeden geçici bir "zula" / depolama alanına saklamanızı ve çalışma alanınızı temizlemenizi sağlar. İşi bitirip geri döndüğünüzde bu değişiklikleri tekrar geri yükleyebilirsiniz.

### Temel Stash Komutları

* **`git stash`**: Çalışma alanındaki commit edilmemiş değişiklikleri geçici depolama alanına (zula) kaldırır ve çalışma dizinini son commit'in temiz haline getirir.
* **`git stash pop`**: Zula listesindeki en son kaydedilen değişikliği geri yükler ve onu stash listesinden tamamen **siler**.
* **`git stash list`**: Zula havuzunda bekleyen tüm stash kayıtlarını listeler.
* **`git stash apply stash@{0}`**: Belirtilen indeksteki değişikliği geri yükler fakat `pop` komutunun aksine onu stash listesinden **silmez**, listede tutmaya devam eder.
  * *Not (`stash@{0}` Mantığı):* `stash@{0}` her zaman **en son eklenen (en güncel)** stash kaydını ifade eder. Yeni bir stash eklendikçe o `stash@{0}` olur, eskiler birer basamak geriye kayar (`stash@{1}`, `stash@{2}` şeklinde).
* **`git stash clear`**: Zula havuzundaki tüm stash geçmişini tamamen temizler ve siler.