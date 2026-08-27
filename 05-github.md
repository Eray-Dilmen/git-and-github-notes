# Git Projesini GitHub'a Bağlama Yöntemleri

Bir Git projesini GitHub'daki uzak depoya (remote repository) bağlamak ve senkronize etmek için temelde **2 farklı yöntem** kullanılır:

---

## 1. Yöntem: `git clone` ile Başlamak (Uzak Depodan Lokale)

* **Kullanım Senaryosu:** Proje/repo önce GitHub üzerinde oluşturulur.
* **Nasıl Çalışır?** `git clone <repo-url>` komutu kullanıldığında Git arka planda şu işlemleri otomatik yapar:
  1. Uzak depoyu (`origin`) tanıtır.
  2. Varsayılan dalı (`main`) belirler.
  3. Lokal dal ile uzak dalı birbirine bağlar (`upstream tracking`).
* **Sonuç:** Ekstra bağlantı komutları yazmadan doğrudan commit atıp `git push` yapabilirsiniz.

---

## 2. Yöntem: `git init` ve Remote Ekleme (Lokalden Uzak Depoya)

* **Kullanım Senaryosu:** Proje önce yerel bilgisayarda başlatılır (`git init`), sonradan GitHub'daki boş bir depoya bağlanmak istenir.
* **Gereken Komutlar ve Anlamları:**

```bash
git remote add origin [https://github.com/kullanici/repo.git](https://github.com/kullanici/repo.git)
git branch -M main
git push -u origin main
```

* `git remote add origin <URL>`: GitHub deposunun adresini projeye kaydeder ve bu adrese `origin` takma adını (alias) verir.
* `git branch -M main`: Mevcut dalın adını standart olarak `main` yapar.
* `git push -u origin main`: Lokal `main` dalını `origin` üzerindeki `main` dalına yükler ve `-u` (`--set-upstream`) parametresi ile bu iki dalı birbirine bağlar. Böylece sonraki adımlarda sadece `git push` yazmak yeterli olur (yani en başta ilk push işleminde 1 kez bu şekilde eşleme yapmak, sonrakilerde yalnızca `git push` yazarak göndermenizi sağlar).

---

## Önemli Not: Kapsam ve Çalışma Mantığı (Depo vs Klasör)

* **Tüm Projeyi Kapsar:** Git klasör bazlı değil, **depo (repository) bazlı** çalışır. `git init` çalıştırılan ana dizin ve onun altındaki tüm alt klasörler/dosyalar tek bir Git deposuna aittir.
* **Tek Seferlik Kurulum:** `git remote add` ve `git push -u origin main` komutları proje genelinde yalnızca **bir defa** çalıştırılır. Alt klasörler için ayrı ayrı tekrar çalıştırılmaz.
* **Otomatik Gönderim Olmaz:** Commit atmak değişiklikleri sadece lokalde kaydeder; GitHub'a aktarmak için her zaman `git push` çalıştırılmalıdır.

---

# İkinci Bir Branch'e Commit Atma ve Push İşlemleri

### İlgili Branch'e Geçip Commit Atma ve Push Komutları:

```bash
# 1. feat branch'ine geç (yoksa '-c' ile oluşturup geçer)
git switch -c feat

# 2. Değişiklikleri sahneye ekle ve commit at
git add .
git commit -m "mesajınız"

# 3. GitHub'daki feat branch'ine gönder (-u ile upstream bağlanabilir)
git push -u origin feat
```

---

# Pull Request (PR) Nedir?

Pull Request (PR), bir daldaki (`branch`) değişikliklerin ana dala (`main`) birleştirilmesini talep etmek ve bu kodları ekip arkadaşlarıyla birlikte incelemek (Code Review) için GitHub üzerinden açılan bir istek/onay mekanizmasıdır.

GitHub üzerinde farklı bir branch'teyken **"Compare & pull request"** butonuna basıldığında:
* Dallarda çakışma yoksa **"Able to merge"** uyarısı gelir.
* Değişiklikleri açıklayan bir başlık ve yorum ekleyebileceğimiz bir arayüz açılır.
* PR onaylanıp GitHub üzerinde merge edildiğinde, birleştirme işlemi uzak repoda (`remote`) tamamlanmış olur.

---

## Uzak Repodaki Değişiklikleri Lokale Çekme (`fetch` ve `pull`)

GitHub üzerinde PR merge edildiğinde uzak depo (`origin/main`) bir adım öne geçer; ancak lokal çalışma alanımızdaki `main` dalı henüz bu değişikliklerden haberdar değildir ve `git log` çıktısında bu son commit görünmez.

Lokal ve uzak repoyu eşitlemek için `git fetch` ve `git pull` komutları kullanılır:

### 1. `git fetch` (Güvenli Kontrol)

`git fetch origin main`, uzak depodaki (`origin`) en son değişiklikleri ve commit geçmişini yerel Git veritabanına indirir; ancak **çalışma alanınızdaki kodları doğrudan güncellemez veya birleştirmez**.

`git fetch` yaptıktan sonra `git status` çalıştırdığımızda şu uyarıyı alırız:

```plaintext
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

Bu uyarı, lokal dalımızın uzak daldan 1 commit geride olduğunu ve `git pull` ile hızlıca güncellenebileceğini belirtir.

#### Değişiklikleri Birleştirmeden Önce İncelemek:
1. `git checkout origin/main` (veya `git switch --detach origin/main`) diyerek uzak dalın son haline geçici olarak göz atabiliriz (bu aşamada `detached HEAD` durumuna geçer).
2. `git log` ve `git diff` ile uzakta neler yapılmış, kodlarda bir sorun var mı inceleriz.
3. İnceleme bittikten sonra kendi yerel ana dalımıza geri dönmek için `git switch main` komutunu çalıştırırız.

---

### 2. `git pull` (İndir ve Birleştir)

```plaintext
git pull = git fetch + git merge
```

`git pull origin main`, uzak depodaki değişiklikleri indirir ve doğrudan bulunduğunuz yerel dala birleştirir (`merge`). Değişikliklerden eminseniz en pratik yöntemdir.

---

## Lokal ve Remote Eşitleme Akışı

1. Uzaktaki değişiklikleri çekmek için:
   ```bash
   git pull origin main
   ```
2. Lokal `main` dalında yeni bir `3.dosya` oluşturup commit attığımızda bu kez lokal dal GitHub'dan 1 commit öne geçer (`Your branch is ahead of 'origin/main' by 1 commit`).
3. Lokalde yapılan bu yeni commit'i GitHub'a göndermek için:
   ```bash
   git push origin main
   ```

> **Her Seferinde `git push origin main` Yazmak Zorunda mıyız?**  
> Hayır. İlk push sırasında `git push -u origin main` komutunu kullandıysanız (yani `-u` / `--set-upstream` ile takip bağlantısı kurulduysa), sonraki tüm gönderimlerde sadece **`git push`** ve tüm çekme işlemlerinde sadece **`git pull`** yazmanız yeterlidir.

---

# `git clone` Nedir?

`git clone`, GitHub veya herhangi bir uzak sunucuda (remote) bulunan bir deponun tüm commit geçmişini, branch'lerini ve dosyalarını yerel bilgisayarınıza eksiksiz bir şekilde indirmeyi sağlar.

### Nasıl Kullanılır?
1. GitHub deposunda yer alan yeşil **"Code"** butonuna basarak deponun HTTPS/SSH linkini kopyalayın.
2. Terminalde projeyi indirmek istediğiniz dizine gidin:
   ```bash
   cd istediginiz_klasor
   ```
3. Klonlama komutunu çalıştırın:
   ```bash
   git clone [https://github.com/kullanici/repo.git](https://github.com/kullanici/repo.git)
   ```

---

## Fork Mantığı ve Açık Kaynak Projelere Katkı (Contribution)

Sahibi veya yetkili bir üyesi olmadığınız bir GitHub projesini doğrudan `git clone` ile indirip çalışabilirsiniz; ancak yetkiniz olmadığı için yaptığınız değişiklikleri ana repoya doğrudan `git push` edemezsiniz (hata alırsınız). Sadece `git fetch` ve `git pull` ile değişiklikleri indirebilirsiniz.

Projeye katkı sağlayıp değişiklikleri ana projeye göndermek için **Fork** mekanizması kullanılır:

### 1. Projeyi Fork'lamak
* GitHub üzerinde projenin sağ üstündeki **"Fork"** butonuna basılır.
* Projenin birebir kopyası sizin kendi GitHub hesabınıza aktarılır. Artık o kopyanın sahibi sizsinizdir.

### 2. Lokalde Geliştirme Yapmak ve Kendi Reponuza Push'lamak
* Kendi GitHub hesabınızdaki forkladığınız repoyu `git clone` ile bilgisayara indirirsiniz.
* Değişiklikleri yapıp commit attıktan sonra kendi GitHub reponuza push edersiniz:
  ```bash
  git push origin feat
  ```

### 3. Pull Request (PR) Açmak
* Kendi forkladığınız GitHub sayfasına girdiğinizde yukarıda *"This branch is 1 commit ahead of original_owner:main"* şeklinde ana projeden 1 commit önde olduğunuz belirtilir.
* Yanındaki **"Contribute"** butonuna basıp **"Open pull request"** seçilerek ana repo sahibine yaptığınız değişiklikleri özetleyen bir açıklama/yorum ile birlikte birleştirme isteği gönderilir.

---

## Code Review ve PR İnceleme Süreci (Proje Sahibi Açısından)

Proje sahibi kendi reposuna girdiğinde yukarıda **"Pull requests: 1"** bildirimi görür. PR'ın içerisine girdiğinde gönderen kişinin adını, mesajını ve değiştirilen kodları (`Files changed`) satır satır inceleyebilir.

İnceleme ekranında **"Review changes"** butonuna tıklandığında 3 seçenek sunulur:

1. **Comment (Yorum):** Kodu ne onaylar ne de reddeder; sadece belirli satırlar veya genel akış hakkında soru sorar ya da geri bildirimde bulunur.
2. **Approve (Onaylama):** Yapılan değişiklikleri tamamen onaylar ve kodun ana projeye birleştirilmeye hazır olduğunu belirtir.
3. **Request Changes (Değişiklik Talep Etme):** Kodda tespit edilen hatalar, standartlara uymayan kısımlar veya eksikler nedeniyle PR'ın birleştirilmesini **engeller**. Gönderen kişiden belirtilen eksikleri düzeltip aynı dala yeni commit atmasını ister (Yeni commit atıldığında PR otomatik güncellenir).

### Pull Request (PR) Sonuçlandırma:
* **Merge:** İnceleme başarılıysa **"Merge pull request"** butonuna basılarak kodlar ana projeye dahil edilir.
* **Close (Kapatma):** Eğer değişiklik proje için uygun değilse veya gereksiz görülürse bir açıklama yazılarak **"Close pull request"** butonu ile PR birleştirilmeden kapatılabilir.

---

# Private Repoya Pull Request Gönderme ve Ekip Çalışması

Gizli (Private) bir repoda dışarıdan fork ile katkı sağlanamaz; çünkü repoyu sadece erişim yetkisi olanlar görebilir. Bir arkadaşınızla özel bir repo üzerinde birlikte çalışmak için şu adımlar izlenir:

### 1. İşbirlikçi (Collaborator) Ekleme
1. GitHub'da ilgili reponun **Settings** sekmesine gelinir.
2. Sol menüdeki *Access* bölümünün altında yer alan **Collaborators** seçeneğine tıklanır.
3. **Add people** butonu ile arkadaşınızın kullanıcı adı, tam adı veya e-posta adresi aratılarak davet gönderilir.
4. Karşı taraf gelen e-posta veya GitHub bildiriminden daveti kabul ettiğinde repoya doğrudan okuma ve yazma (push access) yetkisine sahip olur.

### 2. Güvenli Geliştirme ve PR Akışı
Her iki taraf da `push` yetkisine sahip olsa bile doğrudan `main` dalına commit atmak çakışmalara (conflict) ve kod bozulmalarına yol açabilir. Bu nedenle en sağlıklı yöntem:

* Geliştirme yapacak kişi projeyi kendi yereline klonlar (`git clone`).
* Doğrudan `main` yerine yeni bir branch açarak çalışır (`git switch -c feat/yeni-ozellik`).
* Geliştirmeler tamamlandığında bu branch'i GitHub'a gönderir (`git push -u origin feat/yeni-ozellik`).
* GitHub arayüzünden `main` dalına doğru bir **Pull Request (PR)** açar.
* Repo sahibi veya ekip arkadaşı kodları inceler (Code Review), onaylar (Approve) ve güvenli bir şekilde `main` dalına birleştirir (Merge).

---

# GitHub'ın Diğer Önemli Özellikleri

## GitHub Actions

GitHub Actions, yazılım geliştirme süreçlerini otomatikleştiren bir **CI/CD (Continuous Integration / Continuous Delivery)** platformudur. 

* Depoda belirli bir olay gerçekleştiğinde (örneğin `main` dalına `push` atıldığında veya bir `Pull Request` açıldığında) otomatik olarak çalışacak iş akışları (workflows) tanımlamanızı sağlar.
* **Kullanım Alanları:**
  * Kod her push edildiğinde otomatik testlerin çalıştırılması ve hata varsa bildirilmesi.
  * Kod standartlarının ve linter kontrollerinin yapılması.
  * Projenin otomatik olarak derlenip canlı sunucuya (AWS, Vercel, Docker vb.) dağıtılması (deploy).

---

## GitHub Projects

GitHub Projects, yazılım ekiplerinin görevleri, hataları (issues) ve geliştirme süreçlerini takip etmesini sağlayan esnek bir **proje yönetim aracıdır** (Trello veya Jira benzeri).

* **Kanban Panosu:** Görevleri *Todo* (Yapılacaklar), *In Progress* (Devam Edenler) ve *Done* (Tamamlananlar) sütunlarına ayırarak görsel bir iş akışı sunar.
* **Entegrasyon:** Açılan issue ve pull request'ler doğrudan projedeki kartlara bağlanabilir; bir PR merge edildiğinde ilgili görev kartı otomatik olarak *Done* sütununa taşınabilir.

---

## `README.md` Nedir ve Ne İşe Yarar?

`README.md`, bir GitHub reposunun ana sayfasında en altta doğrudan görüntülenen, Markdown formatında yazılmış projenin **vitrini ve kullanım kılavuzudur**.

* **Ne İşe Yarar?**
  * Projenin ne amaçla yapıldığını ve hangi problemi çözdüğünü açıklar.
  * Kullanılan teknolojileri, kütüphaneleri ve gereksinimleri belirtir.
  * Projeyi bilgisayara indirip yerelde çalıştırmak için gereken adımları (kurulum ve çalıştırma komutları) listeler.
  * Açık kaynak projelerde başkalarının projeye nasıl katkı sağlayabileceğine (contributing kuralları) dair rehberlik eder.
