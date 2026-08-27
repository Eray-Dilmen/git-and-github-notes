## Git Temel Kavramları

* **Commit**: Projede yaptığınız değişikliklerin (stage alanına alınanların) kalıcı olarak yerel depoya (local repository) kaydedilmesidir. Kaydedilmiş bir "kontrol noktası" (checkpoint) oluşturur.
* **Branch (Dal)**: Projenin ana kodunu etkilemeden, bağımsız bir şekilde geliştirme yapmak için açılan ayrı çalışma alanlarıdır. İki veya daha fazla kişinin aynı projede birbirlerinin kodunu bozmadan farklı özellikler (dallar) üzerinde çalışmasını sağlar.
* **Merge (Birleştirme)**: Farklı branch'lerde (dallarda) yapılan çalışmaların tamamlandıktan sonra ana kodla veya diğer dallarla birleştirilmesi işlemidir.

## Git Çalışma Alanları ve Akışı

Git üzerinde dosyaların geçtiği 3 temel aşama vardır:
1. **Çalışma Klasörü (Working Directory)**: Kodları yazdığımız ve düzenlediğimiz mevcut klasördür.
2. **Hazırlık Alanı (Staging Area / Index)**: `git add` komutu kullanıldığında dosyalar bu alana gider. Henüz commit edilmemiş, bir nevi "arafta" veya "sahnede" bekleyen, commit'e hazırlanmış değişikliklerin tutulduğu yerdir.
3. **Yerel Depo (Local Repository)**: `git commit` komutu çalıştırıldığında, staging alanındaki dosyalar yerel depoya bir versiyon (kayıt noktası) olarak kalıcı şekilde gönderilir.

## Temel Git Komutları ve Dosya Takibi

* **`git status`**: Projedeki dosyaların güncel durumunu (değiştirilmiş, staging alanına alınmış veya henüz takip edilmeyen dosyaları) gösterir.
* **`git init`**: Bulunduğunuz klasörü Git tarafından versiyon kontrolü yapılabilen yerel bir depoya dönüştürür (Git Initializer). Başlatıcı komuttur. 

**Not:** Bu komut çalıştırıldığında klasörün içinde gizli bir `.git` dizini oluşturulur. Örneğin `ProjeKlasoru` adında bir dizinde `git init` çalıştırırsanız, o klasör artık Git deposu olur. Ancak, hazır bir projeyi uzak bir sunucudan çekiyorsanız (`git clone <repo-url>`), klasör `.git` dosyasıyla birlikte ineceği için **tekrar `git init` yapmanıza gerek yoktur**.

### Gizli Dosyalar ve `.git` Klasörü

Terminalde `ls` yazdığınızda gizli klasörler görünmez. Gizli klasörleri görmek için `ls -la` (veya `ls -a`) komutunu kullanmanız gerekir.
`.git` klasörü, projenin tüm commit geçmişini, versiyon kayıtlarını ve yapılandırma ayarlarını barındıran "beyin" klasörüdür. `cd .git` diyerek içine girip yapıları inceleyebilirsiniz.

**`.git` klasörünü yanlışlıkla silersek ne olur?**
Eğer proje klasörünüzde yanlışlıkla `rm -rf .git` komutunu çalıştırırsanız, proje klasörünüz Git deposu olmaktan çıkar. `git status` yaptığınızda `fatal: not a git repository` hatası alırsınız. Projenizdeki kodlar silinmez, ancak **önceden yaptığınız tüm commit logları ve versiyon geçmişi tamamen yok olur.**

## Durum Kontrolü (Status) ve Staging Area (Add)

Yeni dosyalar oluşturulduğunda Git bunları otomatik olarak takip etmez (Untracked files). `git status` çıktısı şu şekilde görünür:

```text
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .idea/
    01-Terminal Komutları ve giriş.md
    02-Git Temelleri.md

nothing added to commit but untracked files present (use "git add" to track)
```

Dosyayı staging alanına (sahneye) eklemek için `git add` kullanılır. (Not: Dosya adında boşluk olduğu için tırnak içine alınmalıdır):

```bash
git add "01-Terminal Komutları ve giriş.md"
```

Dosyayı ekledikten sonra `git status` ile durumu tekrar kontrol ettiğimizde çıktı şu şekilde değişir:

```plaintext
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
    new file:   01-Terminal Komutları ve giriş.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .idea/
    02-Git Temelleri.md
```

Görüldüğü gibi ilk dosya "Changes to be committed" alanına (staging) geçti, diğer dosyalar hala "Untracked" (takip edilmeyen) kısmında bekliyor.

## Commit İşlemi (Kaydetme)

Staging alanına (sahneye) aldığımız dosyaları kalıcı olarak Git deposuna (.git içine) kaydetmek için `git commit` kullanılır.

Commit mesajları büyük önem taşır. "Ne önemi var?" denilebilir ama özellikle 5 ay veya daha uzun süren büyük projelerde, neyin nerede ve neden yapıldığını geriye dönük takip edebilmek açısından hayati rol oynar. Ayrıca GitHub üzerinde ilgili commit mesajına tıklandığında, o commit ile hangi dosyalarda ne gibi değişiklikler yapıldığı satır satır görüntülenebilir.

Örnek commit atımı:

```bash
git commit -m "docs(01): Add Terminal Commands File"
```

Komut çalıştırıldığında terminal çıktısı:

```plaintext
[main (root-commit) 8bcf1d0] docs(01): Add Terminal Commands File
 1 file changed, 76 insertions(+)
 create mode 100644 01-Terminal Komutları ve giriş.md
```

Burada `1 file changed, 76 insertions(+)` ifadesi, 1 dosyanın değiştiğini ve dosyaya 76 satır eklendiğini belirtir.

Commit işleminden sonra tekrar `git status` çalıştırdığımızda:

```plaintext
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .idea/
    02-Git Temelleri.md

nothing added to commit but untracked files present (use "git add" to track)
```

Commit edilen dosya artık listede yer almaz. Henüz commit edilmemiş veya eklenmesinden emin olunmamış diğer dosyalar (`02-Git Temelleri.md` vb.) untracked olarak listede durmaya devam eder.

> **Not (`git add .` Kullanımı):** Dosyaları tek tek `git add "dosya_adi"` şeklinde eklemek yerine, bulunduğunuz dizindeki ve alt dizinlerdeki takip edilmeyen veya değiştirilmiş tüm dosyaları tek seferde staging alanına almak için `git add .` komutunu kullanabilirsiniz.

---

## `git log` Nedir?

`git log`, projede o ana kadar atılmış olan tüm commit geçmişini (tarihçesini) kronolojik olarak görüntülemeyi sağlar.

Örnek çıktı:

```plaintext
commit 8bcf1d0cfc80228dfde5c1812ea27cdf313de24a (HEAD -> main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 14:47:59 2026 +0300

    docs(01): Add Terminal Commands File
```

Bu çıktıda yer alan bilgiler:

* **Commit Hash (SHA-1):** En üstte yer alan 40 karakterlik benzersiz kimlik kodudur (`8bcf1d0...`). Bu hash kodu, o commit'in parmak izidir. İleride eski bir sürüme geri dönmek (`git checkout <hash>` veya `git revert <hash>`), iki commit arasındaki farkları incelemek ya da belirli bir noktayı referans almak için bu kod kullanılır.
* **HEAD -> main:** Şu an `main` branch'inde (dalında) olduğunuzu ve projenin en son kaydedilen durumunun bu commit'e işaret ettiğini gösterir.
* **Author:** Commiti atan kişinin adı ve e-posta adresidir.
* **Date:** Commit'in tam olarak hangi tarih ve saatte kaydedildiğini gösterir.
* **Commit Mesajı:** Yapılan değişikliğin amacını açıklayan açıklamadır (`docs(01): Add Terminal Commands File`).

## Takip Edilen Dosyalarda Değişiklik Yapılması (`modified`)

Daha önce commit'lediğimiz `01-Terminal Komutları ve giriş.md` dosyası üzerinde yeni bir değişiklik yapıp kaydettiğimizde, `git status` çıktısı şu şekilde olur:

```plaintext
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   01-Terminal Komutları ve giriş.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .idea/
    02-Git Temelleri.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Bu çıktıda:
* **`Changes not staged for commit`**: Dosyanın zaten Git tarafından takip edildiğini (`tracked`), üzerinde değişiklik yapıldığını (`modified`) ancak bu yeni değişikliğin henüz staging alanına eklenmediğini belirtir.
* **`git restore <file>`**: Eğer yapılan son değişikliklerden vazgeçip dosyayı son commit edilen haline geri döndürmek isterseniz bu komut kullanılır.
* Değişikliği commit'e dahil etmek için tekrar `git add "01-Terminal Komutları ve giriş.md"` komutunu çalıştırmak gerekir.
Değiştirilen dosyaları da görüldüğü üzere ekranda görebiliyoruz. `Changes not staged for commit` ile bilgi veriyor ve `modified:` şeklinde hangi dosyanın değiştirildiğine dair dosya adını gösteriyor.

En altta yer alan `git commit -a` (veya `git commit -am "mesaj"`) komutu, daha önce takip edilen (`tracked`) ve üzerinde değişiklik yapılmış (`modified`) dosyaları otomatik olarak `git add` adımını atlayıp direkt commit etmeye yarar. (Not: Yeni oluşturulmuş `untracked` dosyaları kapsamaz).

> **Not (`git add .` ve Toplu Commit Yaklaşımı):** `git add .` opsiyonundan bahsetmiştik ancak tamamını aynı anda almayı doğru bulmuyorum; çünkü her dosyada farklı bir geliştirme yapıyor olabiliriz ve bu durum farklı amaçtaki dosyalara tek bir commit mesajı atılmasına neden olur. Büyük projelerde belki daha büyük çapta toplu dosya commitleri atılabilir ama küçük/modüler geliştirmelerde dosyaları ayrı ayrı commit'lemek takibi kolaylaştırır.

---

`01-Terminal Komutları ve giriş.md` dosyasını değiştirip tekrar commit ettikten sonra loga bir daha bakıyoruz:

```plaintext
commit dbe0bf92cf6b911347bf573cbb32eec3511620de (HEAD -> main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 15:01:32 2026 +0300

    ufak değişiklikler

commit 8bcf1d0cfc80228dfde5c1812ea27cdf313de24a
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Tue Aug 25 14:47:59 2026 +0300

    docs(01): Add Terminal Commands File
```

Burada yine aynı dosya olsa bile değişikliği yapan kişinin adını, soyadını, e-posta adresini ve değiştirilme tarih/saatini kronolojik sıra ile görebiliyoruz.

## Commit Yaparken `-m` Kullanılmazsa Ne Olur?

Commit mesajı zorunlu olduğu için terminale sadece `git commit` yazıp çalıştırırsanız, Git varsayılan metin düzenleyicisini (çoğunlukla **Vim** veya **Nano**) açarak sizden bir commit mesajı girmenizi bekler.

* **Vim editörü açılırsa:**
  * Mesaj yazmak için önce `i` tuşuna basıp ekleme moduna (`Insert`) geçmeniz gerekir.
  * Mesajınızı yazdıktan sonra `Esc` tuşuna basıp `:wq` yazıp `Enter`layarak kaydedip çıkabilirsiniz.
  * Hiçbir değişiklik yapmadan işlemi iptal edip çıkmak için `Esc` tuşuna basıp `:q!` veya `:q` yazmanız gerekir.

--- 

## `.gitignore` Nedir?

Git'in görmezden geleceği (takip etmeyeceği) dosya ve klasörleri belirtmemizi sağlar. Bunun için `.gitignore` dosyası oluşturmamız gerekir. Terminalden `touch .gitignore` şeklinde oluşturabiliriz (tabii lokasyonu kontrol etmemiz ve mevcut projenin ana dizininde bulunduğumuzdan emin olmamız lazım).

Örneğin API key gibi kimsenin görmesini istemediğimiz hassas veriler içeren bir dosya oluşturmak istiyoruz. Bu tarz dosyaların hiçbir zaman yerel veya uzak repoya konulmaması ve staging alanına (`git add`) dahil edilmemesi gerekir.

`gizli.txt` adında bir dosya oluşturduğumuzda, `git status` komutunda bu dosyayı görebiliriz. Bu durumda yanlışlıkla `git add .` yaparsak gizli dosya da sahneye eklenir ve istemediğimiz bir durum gerçekleşir.

`.gitignore` dosyasının içine `gizli.txt` yazdığımız zaman artık hiçbir Git komutunda (takip edilmeyen dosya olarak) listelenmeyecektir.

`.gitignore`'a eklemeden önce `git status` ile bakalım:

```plaintext
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .gitignore
    .idea/
    02-Git Temelleri.md
    gizli.txt

nothing added to commit but untracked files present (use "git add" to track)
```


`gizli.txt` dosyası başlangıçta untracked listesinde gözüküyor.

Şimdi `.gitignore` dosyası içerisine `gizli.txt` yazıp kaydediyoruz ve tekrar `git status` yapıyoruz:

```plaintext
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .DS_Store
    .gitignore
    .idea/
    02-Git Temelleri.md

nothing added to commit but untracked files present (use "git add" to track)
```

Görüldüğü üzere `gizli.txt` artık burada gözükmüyor bile. Yanlışlıkla bile olsa `git add .` yaparak onu ekleyemeyiz; çünkü artık Git'in takip listesinden tamamen çıkarılmıştır ve `untracked` alanında dahi yer almaz.

> **`.gitignore` Neden Hala `untracked` Olarak Duruyor?**
> Kuralların yerel ortamda geçerli olması için `.gitignore` dosyasını commit'lemek şart değildir; dosya kaydedildiği anda içindeki kurallar anında çalışır (bu yüzden `gizli.txt` hemen listeden kayboldu). Ancak `.gitignore` dosyasının kendisi de yeni oluşturulmuş bir dosya olduğu için Git onu takip etmeye çalışır. Kuralların projenin geneline dahil olması ve repoya aktarılması için `.gitignore` dosyasının da staging alanına alınıp commit edilmesi gerekir.

Şimdi `.gitignore` dosyasını commit'leyelim ve ardından `gizli.txt` dosyasına bir şeyler yazalım.

On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.DS_Store
	.idea/
	02-Git Temelleri.md

nothing added to commit but untracked files present (use "git add" to track)

---

hiçbir şekilde gizli.txt gelmiyor bunu görmüş olduk.

