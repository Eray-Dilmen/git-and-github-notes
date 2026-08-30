## Değişiklikleri ve Commit'leri Geri Alma

### 1. Commit Atılmamış Değişiklikleri Geri Alma (`git restore`)

Henüz commit edilmemiş, çalışma alanında (`working directory`) yapılmış değişiklikleri iptal etmek için `git restore` kullanılır.

Örnek olarak `giris.txt` dosyası oluşturup 1 satır yazı yazıp commit ettiğimizi varsayalım. Ardından bu dosyaya 100 veya daha fazla satır ekleme yaptık fakat ne değiştirdiğimizi tam hatırlamıyoruz veya bu değişikliklerden tamamen vazgeçmek istiyoruz.

`git status` çıktısına baktığımızda:

```plaintext
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   giris.txt
```

Terminalde `git restore giris.txt` komutunu çalıştırdığımızda dosya, **en son atılan commit'teki temiz haline** geri döner (yapılan tüm kaydedilmemiş değişiklikler silinir).

---

### 2. Eski Bir Commit'e Geçici Olarak Göz Atma (`git checkout <hash>` ve Detached HEAD)

Eski bir commit'in o anki durumunu incelemek için commit ID'si (hash) ile `git checkout <commit-id>` komutu kullanılır.

Bu komut çalıştırıldığında `detached HEAD` (ayrılmış HEAD) durumuna geçilir. Yani `HEAD`, bir branch'e bağlı olmak yerine doğrudan o geçmiş commit'e işaret eder; ana branch (`main`) ise ilerideki güncel yerinde kalır.

Terminal çıktısı:

```plaintext
Note: switching to '7bf82b00839b8dcd4b9ed7fe31510c92301b8731'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 7bf82b0 giris cümlesi yazıldı
```

* Bu durumdayken sadece eski kodları inceleyebilir veya deneysel değişiklikler yapabilirsiniz.
* Tekrar güncel ana dala dönmek için: `git switch main`
* Eğer o eski noktadan itibaren yeni bir geliştirme başlatmak isterseniz: `git switch -c <yeni-dal-adi>`


---

### Göreceli Referanslar ile Commit'ler Arasında Gezinme (`^` ve `~`)

Eski commit'lere gitmek için her zaman uzun commit hash kodunu yazmak zorunda değiliz. Git'te `^` (şapka) ve `~` (tilde) işaretlerini kullanarak bulunduğumuz noktaya veya belirli bir branch/commit'e göre geriye doğru hareket edebiliriz.

* **`^` (Caret / Şapka) ile 1 Adım Geriye Gitme:** Belirtilen referansın bir önceki commit'ine (ebeveynine) geçiş yapar.
  * `git checkout HEAD^`: Bulunduğumuz aktif commit'ten 1 önceki commit'e gider.
  * `git checkout main^`: `main` branch'inin işaret ettiği en son commit'in 1 önceki commit'ine gider.
  * `git checkout bugFix^`: `bugFix` branch'inin bulunduğu noktadan 1 önceki commit'e gider.
  * `git checkout <hash>^`: Belirtilen hash değerine sahip commit'in hemen 1 önceki commit'ine geri döner.
  *(Not: `HEAD^^` şeklinde yan yana yazarak 2 commit geriye de gidilebilir.)*

* **`~` (Tilde) ile Çok Sayıda Commit Geriye Gitme:** Geriye doğru birden fazla adım atmak istediğimizde tek tek `^^^` yazmak yerine `~<sayı>` yapısını kullanırız.
  * `git checkout HEAD~4`: Bulunduğumuz yerden 4 commit geriye gider.
  * `git checkout main~3`: `main` dalının son halinden 3 commit önceki duruma döner.
  * `git checkout <hash>~2`: İlgili commit'in 2 adım öncesine geçiş yapar.

> **Klavye İpuçları (`~` Tilde İşareti):**
> * **Mac:** `Option + N` tuşlarına basılır (ekrana karakter doğrudan gelmezse ardından `Space` tuşuna basılması gerekir).
> * **Windows:** `Alt Gr + Ü` tuşlarına basılır (ardından ekrana basılması için `Space` tuşuna basılır).

---

### 3. Commit'leri Kalıcı Olarak Geri Alma (`git reset`)

Daha önce atılmış commit'leri geçmişten tamamen silmek veya geri almak için `git reset` komutu kullanılır.

* **`git reset <commit-id>` (veya `--mixed` / `--soft`):** Belirtilen commit'e geri döner. Aradaki commit'ler geçmişten silinir ancak yazılmış olan kodlar dosyalardan silinmez; değişiklikler çalışma alanınızda kalır.
* **`git reset --hard <commit-id>`:** Belirtilen commit'ten sonra yapılmış **tüm commit'leri ve yazılan tüm kodları kalıcı olarak tamamen yok eder**. Çalışma dizinini doğrudan o eski commit'in haline eşitler.

> **Önemli Uyarı (`--hard` Kullanımı):** Çoklu ekip çalışmalarında `git reset --hard` komutunu dikkatli kullanmak gerekir. Uzak repoya (GitHub) gönderilmiş commit'leri kendi yerelinizde hard reset ile silip zorla push etmeye çalışırsanız, ekip arkadaşlarının çalışma alanlarında o commit'ler hala var olacağı için ciddi çakışmalar (conflict) ve veri kayıpları yaşanır.

---

## `git revert` Komutu

`git revert`, geçmişteki belirli bir commit'i ve onun yaptığı değişiklikleri geri almak için kullanılır. Ancak `git reset` gibi geçmişi silip yok etmek yerine, **o commit'in yaptığı değişikliklerin tam tersini uygulayan yepyeni bir commit oluşturur**.

Örnek çıktı:

```plaintext
commit e86f696d75bda1631a8faea35d6be4858776f1ac (HEAD -> main)
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Wed Aug 26 20:04:14 2026 +0300

    Revert "web geliştirme bilgisi verildi"
    
    This reverts commit 1104e7d9a7c4c36acb85fd08919a611686b98ec3.

commit 1104e7d9a7c4c36acb85fd08919a611686b98ec3
Author: Eray Dilmen <eraydilmen5@gmail.com>
Date:   Wed Aug 26 19:40:12 2026 +0300

    web geliştirme bilgisi verildi
```

Görüldüğü üzere üstteki commit mesajında otomatik olarak `Revert "..."` yazar ve altında `This reverts commit ...` uyarısı çıkar. Commit mesajını kendimiz de düzenleyebiliriz ancak varsayılan mesaj ne yapıldığını açıkça anlattığı için genellikle olduğu gibi bırakılır.

**`git reset --hard` ile Farkı ve Avantajı:**  
`git reset --hard` geçmişi silip tarihi yeniden yazar. Ancak `git revert` geçmişi silmez, eski commit listede kalmaya devam eder ve üzerine geri alma commit'i eklenir. Bu sayede uzak depoda (GitHub) veya ekip arkadaşlarında bulunan commit geçmişi bozulmaz ve çakışma (conflict) yaşamadan güvenli bir şekilde geri alma işlemi yapılır.

---

## Değişiklikleri ve Farkları İnceleme (`git diff`)

Hangi commit'ler veya branch'ler arasında ne farklar olduğunu unutmuş olabiliriz. Hatta henüz commit atmadığımız durumlarda çalışma alanımız ile staging/son commit arasındaki satır farklarını görmek için `git diff` (difference) komutu kullanılır.

Örneğin `giris.txt` dosyasına 2. bir satır eklediğimizde `git status` sadece dosyanın değiştiğini (`modified`) gösterir ama içeride neyin değiştiğini söylemez:

```plaintext
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   giris.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git diff` komutunu çalıştırdığımızda:

```diff
diff --git a/giris.txt b/giris.txt
index a45cad8..fc7b1ce 100644
--- a/giris.txt
+++ b/giris.txt
@@ -1 +1,4 @@
-kotlin çok güzel bir dildir, andorit uygulamaları yapılbilir.
\ No newline at end of file
+kotlin çok güzel bir dildir, andorit uygulamaları yapılbilir.
+
+
+giriş textine 2.cümleyi yazdım
\ No newline at end of file
```

Çıktıda:
* **`-` (Kırmızı/Eksi):** Silinen veya değiştirilmeden önceki satırları gösterir.
* **`+` (Yeşil/Artı):** Yeni eklenen satırları gösterir.

En altta görüldüğü üzere `+ giriş textine 2.cümleyi yazdım` cümlesi yeşil artı ile yeni ekleme olarak listelenir.

### Farklı `git diff` Kullanım Senaryoları

* **`git diff HEAD`**: Çalışma alanındaki değişiklikleri en son atılan commit (`HEAD`) ile karşılaştırır.
* **İki Commit Arasındaki Fark:** `git diff <commit1> <commit2>` veya aralarına iki nokta koyarak `git diff <commit1>..<commit2>` şeklinde çalıştırılır.
* **İki Branch Arasındaki Fark:** `git diff <branch1> <branch2>` şeklinde iki dal arasındaki tüm dosya farkları listelenir.

---

## `git rebase` Komutu

`git rebase`, bir branch'in temel aldığı başlangıç noktasını (base) alıp başka bir commit noktasına taşıyarak doğrusal (lineer), tertemiz ve merge commit'i içermeyen bir geçmiş oluşturmayı sağlar.

### `git merge` ile `git rebase` Arasındaki Sıralama Farkı

`main` dalında `m1, m2, m3` ve `feat` dalında `f1, f2` commit'leri varken:

* **Standart Merge Yapıldığında:** Geçmiş dallanıp budaklanır ve arada gereksiz bir `Merge commit` oluşur (`m1 -> m2 -> f1 -> m3 -> [Merge Commit] -> f2`).
* **`git rebase main` Yapıldığında:** `feat` dalındaki commit'ler geçici olarak kenara alınır, `main` dalının güncel commit'leri tabana yerleştirilir ve bizim commit'lerimiz sanki `main`'in en son halinden sonra tek tek yazılmış gibi ardına eklenir:
  $$\text{m1} \rightarrow \text{m2} \rightarrow \text{m3} \rightarrow \text{f1} \rightarrow \text{f2}$$

> **Önemli Not (Rebase ve Commit Kimlikleri):** Rebase yapıldığında commit mesajları ve içerikleri korunsa da commit'lerin SHA-1 hash kodları baştan hesaplanarak **tamamen değişir** (yani teknik olarak yepyeni commit'ler üretilir).

### Altın Kural: Rebase Ne Zaman Kullanılmaz?

Eğer bir branch'i uzak repoya (`GitHub`) push ettiyseniz ve o dal üzerinde **başkaları da çalışıyorsa**, asla `git rebase` yapılmamalıdır. Çünkü rebase geçmişteki commit ID'lerini değiştirip tarihi yeniden yazdığı için, aynı branch'i çeken ekip arkadaşlarının geçmişiyle uyuşmazlık çıkacak ve ciddi senkronizasyon/çakışma sorunları oluşacaktır. Rebase sadece henüz push edilmemiş, yerel (local) branch'leri temizlemek için tercih edilmelidir.