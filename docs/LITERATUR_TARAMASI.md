# Literatür Taraması

## Uydu Görüntülerinden Bina Hasarı Tespiti ve Afet Sonrası Önceliklendirme İçin Web Tabanlı Karar Destek Sistemi

> **Bu belgenin amacı:** Projenin web sistemi kısmının (İP2–İP5) dayandığı bilimsel ve teknik birikimi özetlemek, her iş paketinin literatürde hangi çalışmaya yaslandığını göstermek ve projenin doldurmayı hedeflediği boşluğu açıkça ortaya koymaktır. Model geliştirme ve Kahramanmaraş ince ayarı (İP1) ayrı yürütülmektedir; burada yalnızca İP1'i gerekçelendiren bulgulara değinilmiştir.
>
> **Hazırlanma tarihi:** Kasım 2026

---

## İçindekiler

1. [Tarama Yöntemi](#1-tarama-yöntemi)
2. [Problem Bağlamı: Neden Hızlı Hasar Haritası?](#2-problem-bağlamı-neden-hızlı-hasar-haritası)
3. [Veri Setleri ve Hasar Ölçekleri](#3-veri-setleri-ve-hasar-ölçekleri)
4. [Derin Öğrenme Yöntemleri](#4-derin-öğrenme-yöntemleri)
5. [Genelleme Sorunu: Yeni Bir Afette Model Ne Kadar Başarılı?](#5-genelleme-sorunu-yeni-bir-afette-model-ne-kadar-başarılı)
6. [2023 Kahramanmaraş Depremleri Üzerine Yapılan Çalışmalar](#6-2023-kahramanmaraş-depremleri-üzerine-yapılan-çalışmalar)
7. [Açık Veri Kaynakları ve Altyapı](#7-açık-veri-kaynakları-ve-altyapı)
8. [Sahada Kullanılan Sistemler ve Kullanıcı Odaklı Tasarım](#8-sahada-kullanılan-sistemler-ve-kullanıcı-odaklı-tasarım)
9. [Kullanılabilirliğin Ölçülmesi](#9-kullanılabilirliğin-ölçülmesi)
10. [Literatürdeki Boşluk ve Projenin Katkısı](#10-literatürdeki-boşluk-ve-projenin-katkısı)
11. [İş Paketleri ile Literatür Eşleştirmesi](#11-iş-paketleri-ile-literatür-eşleştirmesi)
12. [Kaynakça](#12-kaynakça)

---

## 1. Tarama Yöntemi

**Aranan kaynaklar:** arXiv, IEEE/CVF (CVPR, ICCV atölyeleri), Elsevier (Remote Sensing of Environment), MDPI (Remote Sensing), Springer (International Journal of Disaster Risk Science), ISPRS Archives, The Seismic Record; ayrıca kurumsal kaynaklar (Copernicus EMS, Maxar/Vantor Açık Veri, AWS Open Data, Microsoft AI for Good, Google Research / BM WFP, Humanitarian OpenStreetMap Team).

**Anahtar kelimeler:** *building damage assessment*, *xBD*, *xView2*, *Siamese U-Net*, *change detection*, *out-of-domain generalization*, *Kahramanmaraş / Türkiye earthquake 2023*, *Maxar Open Data*, *Copernicus EMSR648*, *rapid mapping*, *operational deployment*, *System Usability Scale*.

**Dahil etme ölçütleri:**
- Afet öncesi/sonrası yüksek çözünürlüklü (≤ 1 m) uydu görüntüsüyle **bina düzeyinde** hasar tespiti yapan çalışmalar
- 2023 Kahramanmaraş depremlerine uygulanmış çalışmalar
- Sahada gerçekten kullanılmış (operasyonel) sistemleri ve bu kullanımdan çıkarılan dersleri raporlayan çalışmalar
- Sistemin değerlendirilmesinde kullanılacak ölçütlerin (hasar ölçekleri, SUS) dayanağı olan temel kaynaklar

**Kapsam dışı:** İHA (drone) görüntüsüne dayalı yöntemler bu projenin kapsamında değildir; yalnızca operasyonel derslerin aktarılabildiği yerlerde anılmıştır (bkz. Bölüm 8).

---

## 2. Problem Bağlamı: Neden Hızlı Hasar Haritası?

6 Şubat 2023'te Pazarcık (Mw 7,8) ve Elbistan (Mw 7,5) merkezli depremler on bir ili etkiledi. Afet yönetiminde ilk günlerdeki temel soru şudur: **"Ekipleri önce nereye göndermeliyiz?"** Bu soruya yanıt veren uzman hasar haritalarının hazırlanması zaman alır.

**Copernicus EMS deneyimi (EMSR648):** Avrupa Birliği'nin Copernicus Acil Durum Yönetimi Servisi, ilk depremden 4 saat içinde Hızlı Haritalama modülüyle devreye alındı ve 20 ilgi alanı tanımlandı. Buna karşın **ilk hasar derecelendirme (grading) ürünü, devreye girişten 40 saat sonra** Afşin için teslim edildi; 10 Şubat'a kadar toplam 53 harita üretildi (Copernicus EMS, 2023). Yani en hızlı uzman haritası bile ilk iki günü kaçırmıştır. Bu çalışmada otomatik sistemin hedefi, bu süreyi görüntü elde edildikten sonra **dakikalar** mertebesine indirmektir.

**Uydu görüntüsünün erişilebilirliği:** USGS'nin yüzey kırığı haritalama çalışması, Maxar WorldView görüntülerinin depremden bir gün sonra (7 Şubat) gelmeye başladığını ve haritalamanın görüntüler geldikçe sürdürüldüğünü raporlar (Reitman vd., 2023). Aynı ekip, bulut ve kar örtüsünün bazı görüntülerde yüzeyi kapattığını ve Çardak fayının bazı kesimleri için bir aydan uzun süre yüksek çözünürlüklü görüntü bulunamadığını belirtmiştir. Bu iki bulgu projeye iki şey söyler:
1. Yüksek çözünürlüklü uydu görüntüsü büyük afetlerde **1–2 gün içinde** ulaşılabilir hâle gelebilmektedir; darboğaz görüntünün analizidir.
2. Kapsama her yerde aynı değildir; sistem **görüntü olmayan bölgeler için kullanıcıyı açıkça uyarmalıdır** (yol haritası Faz 3'teki uyarı gereksinimi buradan gelir).

> **Not:** Reitman vd. (2023) bina hasarı değil, **fay yüzey kırığı** haritalaması üzerinedir. Başvuru metninde bu kaynak, "Maxar görüntülerinin depremden sonra ne zaman erişilebilir olduğu" bilgisi için kullanılmalıdır; bina hasarı kaynağı olarak gösterilmemelidir.

---

## 3. Veri Setleri ve Hasar Ölçekleri

### 3.1 xBD veri seti

Uydu tabanlı bina hasarı araştırmalarında fiilî standart, xView2 yarışması için yayımlanan **xBD** veri setidir (Gupta vd., 2019a, 2019b). Özellikleri:
- Afet öncesi ve sonrası **eşleştirilmiş** uydu görüntüleri (Maxar), bina poligonları ve sıralı hasar etiketleri
- 850.736 bina etiketi, 45.362 km² alan; birden çok afet türü (deprem, kasırga, sel, yangın, volkan vb.) ve 15 ülke
- Görüntüler 1024×1024 piksel, yaklaşık 0,5 m/piksel çözünürlükte (Dietrich vd., 2025)
- Dört hasar sınıfı: hasarsız, az hasarlı, ağır hasarlı, yıkık (*Joint Damage Scale*); piksel maskelerinde bunlara arka plan sınıfı eklenerek 5 sınıf elde edilir (Benson & Ecker, 2020)

**Projeyle ilişkisi:** İP1'deki modelin temel eğitimi ve İP3'teki öncelik sıralamasının ilk doğrulaması (Spearman ρ) xBD test kümesinde yapılacaktır. Projenin model arayüzündeki 5 sınıflı çıktı tanımı (0–4) doğrudan bu yapıdan gelir.

### 3.2 Copernicus EMS hasar sınıfları

Copernicus EMS, görüntü yorumuna dayalı hasar sınıflarını Avrupa Makrosismik Ölçeği'nden (EMS-98) türetmiştir. Mevcut sınıflar: **yıkık** (EMS-98 derece 4–5), **hasarlı** (derece 1–3), **muhtemelen hasarlı** (yorum güveninin düşük olduğu durumlar) ve **görünür hasar yok** (Copernicus EMS, t.y.-a; t.y.-b). Copernicus, kendi hasar bilgisinin **yer gerçeği değil, yaklaşık ve hızlı bir tahmin** olarak değerlendirilmesi gerektiğini açıkça belirtir.

**Projeyle ilişkisi:** İP3 ve İP5'te Kahramanmaraş doğrulaması Copernicus vektörleriyle yapılacaktır. xBD'nin 4 sınıfı ile Copernicus'un 4 sınıfı **birebir örtüşmez**; bu nedenle karşılaştırmadan önce bir eşleme tablosu yazılması gerekir (yol haritası Faz 2.3). Önerilen başlangıç eşlemesi:

| Copernicus EMS | Projenin renk sınıfı | Gerekçe |
|---|---|---|
| Yıkık (Destroyed) | Kırmızı | EMS-98 derece 4–5 ↔ xBD "yıkık" ve "ağır hasarlı" |
| Hasarlı (Damaged) | Sarı / Kırmızı (karar verilecek) | EMS-98 derece 1–3'ü birlikte kapsar; tek renge düşmez |
| Muhtemelen hasarlı | Sarı | Belirsizlik sınıfı |
| Görünür hasar yok | Yeşil | — |

"Hasarlı" sınıfının geniş olması, Copernicus ile bina düzeyinde birebir karşılaştırmayı zorlaştırır; bu nedenle **blok/hücre düzeyinde sıralama korelasyonu** daha sağlam bir doğrulama ölçütüdür.

### 3.3 ATC-20 renk kodlaması

ABD Uygulamalı Teknoloji Konseyi'nin **ATC-20** prosedürü, deprem sonrası bina güvenlik değerlendirmesinin fiilî standardıdır ve binaları üç renkli etiketle sınıflandırır: **yeşil** (incelendi, görünür güvenlik sorunu yok), **sarı** (sınırlı kullanım) ve **kırmızı** (güvensiz) (ATC, t.y.). ATC bu etiketlerin Türkçe sürümlerini de yayımlamıştır.

**Projeyle ilişkisi:** Projenin yeşil/sarı/kırmızı harita gösterimi bu üçlü yapıya **paralel** kurulmuştur, çünkü saha personeli bu renk diline zaten aşinadır. Ancak bir ayrım vurgulanmalıdır: ATC-20 yerinde mühendis incelemesine dayanır; uydudan yapılan sınıflandırma yalnızca **çatıdan görünen** hasarı yakalar ve yerinde incelemenin yerini tutmaz, yalnızca hangi binaların önce inceleneceğini sıralar.

---

## 4. Derin Öğrenme Yöntemleri

### 4.1 Siamese U-Net: xView2 kazanan çözümü

xView2 yarışmasının birincisi olan çözüm, **Siamese (ikiz) U-Net** mimarisine dayanır: önce bina yerelleştirme modeli eğitilir, ardından bu model hem afet öncesi hem afet sonrası görüntüye **ağırlıklarını paylaşarak** uygulanır ve hasar sınıflandırması yapılır (Durnov, 2020). Bu iki aşamalı yaklaşım daha sonra pek çok çalışmada benimsenmiştir (Dietrich vd., 2025).

Aynı yarışmanın üst sıralarındaki diğer çözümler de benzer bir yola gitmiştir:
- **Khvedchenya & Gabruseva (2021):** Paylaşılan kodlayıcılı Siamese kodlayıcı-çözücü ağ; farklı kodlayıcı, kayıp fonksiyonu ve iki görüntüyü birleştirme yöntemlerini karşılaştıran kapsamlı bir ablasyon çalışması sunar. Siamese yapıyı, iki görüntünün 6 kanallı tek tensörde birleştirilmesiyle karşılaştırır.
- **Weber & Kané (2020):** Afet öncesi ve sonrası görüntüleri ağırlıkları paylaşılan bir CNN'den geçirip özellikleri birleştiren (*multi-temporal fusion*) bir yaklaşım; tek zamanlı ve çok zamanlı kurgunun önemini karşılaştırır ve hasar sınıflarına ağırlık vermenin etkisini gösterir.

**Projeyle ilişkisi:** İP1'deki model (Siamese U-Net, sınıf ağırlıklı çapraz entropi) doğrudan bu çizgidedir. Sistem tasarımında belirleyici olan nokta, modelin **iki görüntü birden** istemesidir: veri hattı (İP2) her adımda afet öncesi ve sonrası görüntüyü aynı piksel ızgarasında birlikte taşımak zorundadır.

### 4.2 Nesne tabanlı yaklaşım: ChangeOS

Zheng vd. (2021), piksel tabanlı Siamese ağların güçlü özellik çıkarımına karşın **anlamsal tutarsızlık** üretebildiğini (aynı binanın farklı piksellerine farklı sınıf verilmesi) göstermiş ve bunu çözmek için nesne tabanlı görüntü analizini derin öğrenmeyle birleştiren **ChangeOS** çerçevesini önermiştir. Model önce bina nesnelerini çıkarır, ardından her nesne için hasarı sınıflandırır ve nesne tabanlı bir son işlemle her binanın tek bir sınıf almasını garanti eder. Yazarlar, şehir ölçeğinde bir analizin bir dakika içinde yapılabildiğini raporlamıştır.

**Projeyle ilişkisi:** İP3'teki "her poligon içindeki piksellerin **çoğunluk sınıfını** binanın sınıfı yap" adımı, ChangeOS'un nesne tabanlı son işlem fikrinin basit bir uygulamasıdır. Bu adım, piksel maskesini belediyenin anlayacağı bina listesine çevirmenin anahtarıdır.

### 4.3 Basitleştirilmiş güçlü temel model

Gerard vd. (2024), yarışmayı kazanan çözümün yarışma için aşırı mühendislik içerdiğini ve genişletilmesinin zor olduğunu belirterek bileşenleri adım adım çıkarmış ve performansın büyük kısmını koruyan daha basit bir temel model yayımlamıştır. Çalışmanın asıl önemli bulgusu bir sonraki bölümün konusudur.

---

## 5. Genelleme Sorunu: Yeni Bir Afette Model Ne Kadar Başarılı?

Bu bölüm, projenin neden Kahramanmaraş'a özel ince ayar (İP1) ve bağımsız doğrulama (İP3, İP5) içerdiğini açıklar.

- **Benson & Ecker (2020):** Her afetin farklı (yeni coğrafya, farklı koşullar) olduğunu ve modellerin eğitimde görülmemiş bir afette değerlendirilmesi (*out-of-domain*, OOD) gerektiğini savunur. Mevcut modellerin büyük ölçüde gerçekçi olmayan dağılım-içi (IID) kurguda test edildiğini, OOD'da performansın belirgin biçimde düştüğünü ve **IID başarısının OOD başarısını öngörmediğini** göstermiştir.
- **Valentijn vd. (2020):** CNN'lerin deneysel ortamda iyi sonuç verdiğini ancak görülmemiş veri ve zaman baskısı altındaki operasyonel koşullarda nasıl çalıştığının yeterince çalışılmadığını belirterek çok-tehlikeli ve mekânsal aktarılabilirliği incelemiştir. Operasyonel koşullarda yeni afetten etiketli veri zamanında elde edilemeyeceği için modelin yeniden eğitilmesinin genellikle bir seçenek olmadığını vurgular.
- **Gerard vd. (2024):** xBD bölmelerini test konumları eğitimde görülmeyecek şekilde yeniden düzenlediğinde, hem kazanan çözümün hem de basitleştirilmiş modelin **"az hasarlı" ve "ağır hasarlı"** sınıflarında güçlü bir genelleme başarısızlığı yaşadığını göstermiştir.
- **Robinson vd. (2023):** Bina hasarı modellerinin dağılım dışı veride ciddi biçimde bozulduğunu belirterek, az sayıda etiketle insan-döngüde (*human-in-the-loop*) hızlı eğitim yapan bir iş akışı önermiştir (bkz. Bölüm 8).

**Projeye çıkarımlar:**
1. Modelin xBD test skoru (0,720), Kahramanmaraş'taki başarıyı **garanti etmez**; bu yüzden Kahramanmaraş verisinde Copernicus ile bağımsız doğrulama zorunludur.
2. Ara sınıflar (az/ağır hasarlı) en kırılgan sınıflardır. Bu, projenin **üç renkli** gösterimi seçmesini destekler: ağır hasarlı ve yıkık birleştirilerek "kırmızı" yapıldığında, modelin en zorlandığı ayrım kullanıcı kararını etkilemez hâle gelir.
3. Bina düzeyindeki hatalar, **hücre/bölge düzeyine toplandığında** kısmen dengelenebilir. Projenin ana çıktısının tek tek bina etiketleri değil, **bölge öncelik sıralaması** olması bu nedenle bilinçli bir tasarım tercihidir; doğrulama da sıralama korelasyonu (Spearman ρ) ile yapılır.

---

## 6. 2023 Kahramanmaraş Depremleri Üzerine Yapılan Çalışmalar

| Çalışma | Veri | Yöntem | Öne çıkan bulgu | Projeye anlamı |
|---|---|---|---|---|
| Karlberg (2023), Uppsala Üniv. yüksek lisans tezi | Maxar öncesi/sonrası, Kahramanmaraş ve Antakya | xBD ile eğitilmiş çift görevli U-Net, **ince ayar yok** | Hasarsız binalarda F1 yüksek (Kahramanmaraş 0,93; Antakya 0,83); yıkık binalarda Kahramanmaraş'ta F1 yalnızca **0,16** | xBD modelinin doğrudan aktarımının yetersiz kaldığının Türkiye'den somut kanıtı → İP1 ince ayarının gerekçesi |
| Xia vd. (2023), *IJDRS* | WorldView-2, İslahiye | İki aşamalı BDANet (öncesi görüntüden bina sınırı, öncesi+sonrası ile hasar) | Bina kenarları iyileştirilmiş; hasar sonuçları nüfus verisiyle birleştirilerek **kurtarma aciliyeti** tahmin edilmiş | Hasar haritasını önceliklendirmeye çevirme fikrinin en yakın örneği; projenin öncelik skoruna doğrudan emsal |
| Ilmak, Iban & Şeker (2024), *ISPRS Archives* | Maxar Açık Veri, yalnız afet sonrası; Antakya ve İskenderun | Sahne (karo) sınıflandırması, ResNet-101 vb. | Bina içeren sahneleri yüksek doğrulukla ayırmış; segmentasyonun veri ve hesaplama maliyetine pratik bir alternatif önermiş | Bölge/karo düzeyinde düşünmenin yerli bir örneği |
| Russo vd. (2025), arXiv | COSMO-SkyMed SAR + OSM + DSM + GEM | Yalnız afet sonrası, çok kaynaklı CNN | Bulut ve afet öncesi görüntü yokluğu sorununu SAR ile aşmayı hedefler | Optik görüntünün sınırlılığının farkında olunduğunu gösterir; gelecek çalışma için yol |
| Robinson vd. (2023b), Microsoft AI for Good raporu | Planet, Maxar vb. | Yapay zekâ ile geniş alan hasar tespiti | Planet'in açıklamasına göre çalışma AFAD'ın müdahale çalışmalarını desteklemek üzere çok paydaşlı olarak yürütülmüş | Bu tür çıktıların gerçekten afet yönetimine aktarıldığını gösterir |
| Google SKAI / BM WFP | Uydu görüntüsü | Açık kaynak hasar tespit aracı | WFP, SKAI'nin Türkiye depreminde yardım kuruluşlarının hızlı müdahalesini desteklediğini raporlamıştır | Açık kaynaklı, operasyonel bir karşılaştırma noktası |
| Ramachandran vd. (2025), *PLOS Digital Health* | Microsoft ve Google model çıktıları | Model çıktısını sağlık tesisi konumlarıyla çakıştırma | Bina düzeyi hasar tahminlerinden hızlı sağlık tesisi hasar raporu üretilmiş | Model çıktısını **karar verilebilir bir listeye** çevirme örneği; projenin "ilk 10 bölge" çıktısıyla aynı mantık |

**Genel değerlendirme:** Türkiye depremi üzerine yöntem odaklı çok sayıda çalışma bulunmaktadır; ancak bunların büyük çoğunluğu **model doğruluğunu** raporlamakta, sonucun bir yerel yöneticinin kullanabileceği bir araca nasıl dönüştürüleceğini ele almamaktadır. Xia vd. (2023) ve Ramachandran vd. (2025) bu yönde adım atan istisnalardır.

---

## 7. Açık Veri Kaynakları ve Altyapı

### 7.1 Maxar (Vantor) Açık Veri Programı

- Maxar Intelligence, Ekim 2025'te **Vantor** adını almıştır. Açık Veri Programı Vantor adı altında sürmektedir.
- Program, büyük afetlerde afet öncesi ve sonrası yüksek çözünürlüklü görüntüleri **CC BY-NC 4.0** lisansıyla (ticari olmayan kullanım serbest, kaynak gösterilmeli) yayımlar.
- Görüntüler AWS Açık Veri üzerinde **STAC kataloğu** ile düzenlenmiş, **bulut için optimize edilmiş GeoTIFF (COG)** biçimindedir ve AWS hesabı gerektirmeden erişilebilir. Aynı veri Source Cooperative üzerinde de aynalanmaktadır (AWS, t.y.; Source Cooperative, t.y.).
- Maxar, deprem sonrası İslahiye, Nurdağı, Kahramanmaraş ve Antakya için öncesi/sonrası görüntüleri programa eklemiştir.

**Projeyle ilişkisi:** COG biçimi, sistemin büyük görüntünün tamamını indirmeden yalnızca kullanıcının seçtiği bölgeyi okumasına olanak verir; "yüklemeden haritaya ≤ 10 dakika" hedefinin teknik dayanağı budur. STAC kataloğu, "haritadan bölge seç → görüntüyü otomatik bul" akışını (yol haritası Faz 3, Yol 1) mümkün kılar. Marka değişikliği nedeniyle katalog adreslerinin değişme riski, risk tablosunda yer almaktadır.

### 7.2 OpenStreetMap ve HOT aktivasyonu

Deprem sonrası Humanitarian OpenStreetMap Team (HOT), yerel ortağı Yer Çizenler ile birlikte bölgedeki eksik bina ve yol verisini haritalamak için aktivasyon başlatmıştır (HOT / Yer Çizenler, 2023). HeiGIT'in analizine göre 21 Şubat 2023 itibarıyla yaklaşık 1,4 milyon bina ve 67 bin km yol, yaklaşık 7 bin katkıcı tarafından haritalanmıştır (HeiGIT, 2023).

**Projeyle ilişkisi:** İP3'te bina poligonları için iki seçenek vardır: (a) model maskesinden poligon üretmek, (b) OSM bina sınırlarını kullanmak. OSM'nin deprem bölgesinde bu denli yoğun güncellenmiş olması, (b) seçeneğini Kahramanmaraş için gerçekçi bir B planı yapar. Robinson vd. (2023a) da benzer şekilde hazır bina ayak izi veri setini (Microsoft Building Footprints) kullanmıştır.

### 7.3 Copernicus EMSR648 vektör verileri

EMSR648 aktivasyonundaki tüm haritalar ve vektör paketleri (SHP, KML) Copernicus EMS sitesinden indirilebilir (Copernicus EMS, 2023). Bu, Kahramanmaraş için bağımsız, uzman yorumuna dayalı bir karşılaştırma verisi sağlar.

---

## 8. Sahada Kullanılan Sistemler ve Kullanıcı Odaklı Tasarım

Model doğruluğu üzerine geniş bir literatür olmasına karşın, **hasar tespit modellerinin gerçek bir afette son kullanıcıya nasıl ulaştırıldığını** raporlayan çalışma sayısı azdır. Bu, projenin web sistemi kısmının doğrudan konumlandığı alandır.

- **Robinson vd. (2023a) – Rolling Fork (ABD) kasırgası:** Microsoft ekibi, Amerikan Kızılhaçı ile birlikte, afetten bir gün sonra elde edilen uydu görüntüsü üzerinde az sayıda etiketle insan-döngüde bir iş akışı çalıştırmış ve bina ayak izleri üzerinde hasar tahminlerini teslim etmiştir. Sonuçlar sahadaki yer gerçeğiyle karşılaştırılmış; hasarlı binalar için kesinlik 0,86, duyarlılık 0,80 bulunmuştur. İş akışı her uydu sahnesi için **2 saatin altında** uçtan uca tamamlanmıştır. Bu çalışma, projenin "≤ 10 dakika" hedefi için bir referans noktası ve **çıktıyı bina poligonu + hasar sınıfı olarak teslim etme** biçiminin bir emsalidir.
- **SKAI ve BM DISHA:** Google Research ile BM'nin geliştirdiği çözümde, analistler bina ayak izlerini ve görüntüleri elde ettikten sonra hızlı bir ön tahmin modeli çalıştırarak en çok hasarın olduğu alanları önden görebilmektedir. BM Global Pulse, hasarlı binaların hasarsızlara göre azlığının bina düzeyindeki başarımı düşürebildiğini gözlemlemiştir. Bu gözlem, projenin bina yerine **bölge** öncelik listesine odaklanmasını destekler.
- **Manzini vd. (2025a, 2025b) – Debby ve Helene kasırgaları (ABD):** Bu çalışmalar İHA görüntüsüne dayanır ve bu nedenle projenin teknik kapsamı dışındadır; ancak operasyonel dersleri aktarılabilir. Yazarlar, bina hasarı tespitine yönelik bir yapay zekâ sistemini resmî olarak ilan edilmiş afetlerde ilk kez sahada kullandıklarını ve 91 afet uygulayıcısını sistemin kullanımı için eğittiklerini raporlar (Manzini vd., 2025a). Eşlik eden çalışmada, operasyon sırasında veri ürünlerinin teslimini **engelleyen, bozan ya da geciktiren dört temel güçlük** ve gelecekteki saha kullanımları için araştırma yönleri tartışılmıştır (Manzini vd., 2025b). Projeye aktarılan ders: modelin doğruluğu kadar, **çıktının doğru zamanda, doğru biçimde ve mevcut iş akışını bozmadan** kullanıcıya ulaşması önemlidir.

> **Not (başvuru metni için):** Yol haritasında yer alan "sahadaki kullanıcıların doğruluktan çok mevcut iş akışlarına uyumu ve hızlı karar vermeyi önemsediği" ifadesi, Manzini vd. çalışmalarının genel yönelimiyle uyumludur; ancak bu ifade başvuruda doğrudan atıfla kullanılacaksa, makalenin tam metnindeki ilgili bölümle sayfa numarası düzeyinde teyit edilmelidir.

**Tasarım ilkelerine çevrilmesi (İP4):**
- Az sayıda düğme, Türkçe etiketler, ATC-20'ye benzer renk dili
- Çıktının ArcGIS/QGIS gibi kurumların zaten kullandığı araçlarda açılabilmesi (GeoJSON, CSV)
- Modelin belirsiz olduğu yerlerin gösterilmesi (Copernicus'un "muhtemelen hasarlı" sınıfına benzer bir yaklaşım)
- Görüntü kapsamının olmadığı bölgeler için açık uyarı

---

## 9. Kullanılabilirliğin Ölçülmesi

**Sistem Kullanılabilirlik Ölçeği (SUS)**, Brooke (1996) tarafından geliştirilmiş, 10 maddeden oluşan ve 0–100 arası tek bir puan üreten bir ankettir. Bangor, Kortum ve Miller (2008), on yıl boyunca toplanan 2.324 SUS anketini analiz ederek ölçeğin güvenilirliğini (Cronbach α ≈ 0,91) göstermiştir; Bangor vd. (2009) ise puanların yorumlanması için sıfat tabanlı bir derecelendirme ölçeği eklemiştir. Uygulamada **68 puan**, çok sayıda çalışmaya dayanan ortalama değer olarak kabul edilir ve "ortalamanın üstü" eşiği olarak kullanılır.

**Projeyle ilişkisi:** İP5'teki SUS ≥ 68 hedefi bu birikimden gelir. Katılımcı sayısının küçük olacağı (5–10 kişi) öngörüldüğünden, SUS'un küçük örneklemlerde de kullanılabilen kısa bir ölçek olması avantajdır; ancak sonuçlar bir sınırlılık notuyla birlikte raporlanmalıdır.

---

## 10. Literatürdeki Boşluk ve Projenin Katkısı

Literatürden çıkan tablo şöyledir:

1. **Model tarafı olgun, sistem tarafı zayıf.** Uydu görüntüsünden bina hasarı tespiti için güçlü yöntemler ve açık kaynaklı kodlar mevcuttur (Durnov, 2020; Zheng vd., 2021; Gerard vd., 2024). Buna karşın bu modelleri **yerel bir yöneticinin tek başına kullanabileceği**, uçtan uca çalışan, açık kaynaklı ve Türkçe bir araç bulunmamaktadır.
2. **Genelleme sorunu bilinmektedir, ancak Türkiye için çözülmemiştir.** xBD modellerinin yeni afetlerde başarısı düşmektedir (Benson & Ecker, 2020; Gerard vd., 2024) ve Türkiye'de bu düşüş somut olarak gösterilmiştir (Karlberg, 2023).
3. **Çıktı biçimi kullanıcı kararına göre tasarlanmamaktadır.** Çalışmaların büyük kısmı piksel ya da bina düzeyi F1 raporlar; **"önce nereye gidilmeli?"** sorusuna yanıt veren bölge düzeyinde bir önceliklendirme ve bunun uzman haritasıyla sıralama düzeyinde doğrulanması nadirdir (istisnalar: Xia vd., 2023; Ramachandran vd., 2025).
4. **Hız ile uzman kalitesi arasında bir boşluk vardır.** Kahramanmaraş'ta ilk uzman derecelendirme haritası 40 saatte gelmiştir (Copernicus EMS, 2023); görüntüler ise 1. günden itibaren gelmeye başlamıştır (Reitman vd., 2023).
5. **Kullanılabilirlik nadiren ölçülmektedir.** Operasyonel sistemleri raporlayan az sayıdaki çalışma (Robinson vd., 2023a; Manzini vd., 2025a, 2025b) bile standart bir kullanılabilirlik ölçütü raporlamamaktadır.

**Bu projenin katkısı:**
- Açık veriden (Maxar/Vantor) otomatik görüntü çeken, Kahramanmaraş'a uyarlanmış bir modeli kullanan ve sonucu **ATC-20 renk diline paralel** bina haritası ile **bölge öncelik listesi** olarak sunan, açık kaynaklı, Türkçe bir web sistemi
- Öncelik sıralamasının hem xBD'de hem de Kahramanmaraş'ta Copernicus uzman haritasıyla **sıralama korelasyonu** üzerinden doğrulanması
- Sistemin uçtan uca süresinin ve kullanılabilirliğinin (SUS) potansiyel kullanıcılarla ölçülmesi

---

## 11. İş Paketleri ile Literatür Eşleştirmesi

| İş paketi | Ne yapılıyor? | Dayandığı literatür |
|---|---|---|
| **İP1** (Azra) | Siamese U-Net, Kahramanmaraş ince ayarı | Durnov (2020); Weber & Kané (2020); Khvedchenya & Gabruseva (2021); Benson & Ecker (2020); Gerard vd. (2024); Karlberg (2023) |
| **İP2** Veri hattı | Öncesi/sonrası eşleştirme, karolama, tahmin | Gupta vd. (2019a, b) — 1024×1024, ~0,5 m; AWS Maxar Açık Veri (STAC, COG); Reitman vd. (2023) — görüntü erişim süresi ve kapsama boşlukları |
| **İP3** Harita ve öncelik | Maskeden binaya, renk, öncelik skoru, doğrulama | Zheng vd. (2021) — nesne tabanlı son işlem; ATC (t.y.) — renk kodu; Copernicus EMS (t.y.) — hasar sınıfları; HOT/HeiGIT (2023) — OSM binaları; Xia vd. (2023) — aciliyet tahmini |
| **İP4** Web platformu | Bölge seçimi, harita, liste, indirme | Robinson vd. (2023a); SKAI/DISHA; Manzini vd. (2025a, b); AWS Maxar Açık Veri |
| **İP5** Test ve değerlendirme | 3 senaryo, süre, SUS | Copernicus EMS (2023) — 40 saat referansı ve vektörler; Brooke (1996); Bangor vd. (2008, 2009) |

---

## 12. Kaynakça

**Veri setleri ve yöntemler**

- Benson, V., & Ecker, A. (2020). *Assessing out-of-domain generalization for robust building damage detection*. NeurIPS 2020 Workshop on AI for Humanitarian Assistance and Disaster Response. https://arxiv.org/abs/2011.10328
- Dietrich, O. vd. (2025). *The potential of Copernicus satellites for disaster response: Retrieving building damage from Sentinel-1 and Sentinel-2*. arXiv:2511.05461. https://arxiv.org/abs/2511.05461
- Durnov, V. (2020). *xView2 1st place solution* [Yazılım]. https://github.com/DIUx-xView/xView2_first_place
- Gerard, S., Borne-Pons, P., & Sullivan, J. (2024). *A simple, strong baseline for building damage detection on the xBD dataset*. arXiv:2401.17271. https://arxiv.org/abs/2401.17271
- Gupta, R., Goodman, B., Patel, N., Hosfelt, R., Sajeev, S., Heim, E., Doshi, J., Lucas, K., Choset, H., & Gaston, M. (2019a). Creating xBD: A dataset for assessing building damage from satellite imagery. *Proceedings of the IEEE/CVF CVPR Workshops*, 10–17.
- Gupta, R., Hosfelt, R., Sajeev, S., Patel, N., Goodman, B., Doshi, J., Heim, E., Choset, H., & Gaston, M. (2019b). *xBD: A dataset for assessing building damage from satellite imagery*. arXiv:1911.09296. https://arxiv.org/abs/1911.09296
- Khvedchenya, E., & Gabruseva, T. (2021). *Fully convolutional Siamese neural networks for buildings damage assessment from satellite images*. arXiv:2111.00508. https://arxiv.org/abs/2111.00508
- Valentijn, T., Margutti, J., van den Homberg, M., & Laaksonen, J. (2020). Multi-hazard and spatial transferability of a CNN for automated building damage assessment. *Remote Sensing, 12*(17), 2839. https://doi.org/10.3390/rs12172839
- Weber, E., & Kané, H. (2020). *Building disaster damage assessment in satellite imagery with multi-temporal fusion*. ICLR 2020 AI for Earth Sciences Workshop. https://arxiv.org/abs/2004.05525
- Zheng, Z., Zhong, Y., Wang, J., Ma, A., & Zhang, L. (2021). Building damage assessment for rapid disaster response with a deep object-based semantic change detection framework: From natural disasters to man-made disasters. *Remote Sensing of Environment, 265*, 112636. https://doi.org/10.1016/j.rse.2021.112636

**2023 Kahramanmaraş depremleri**

- Ilmak, D., Iban, M. C., & Şeker, D. Z. (2024). Deep learning-based scene classification of very high-resolution satellite imagery for post-earthquake damage assessment: A case study of the 2023 Kahramanmaraş earthquakes. *ISPRS Archives, XLVIII-4/W9-2024*, 249. https://isprs-archives.copernicus.org/articles/XLVIII-4-W9-2024/249/2024/
- Karlberg, [ad] vd. (2023). *Deep learning for building damage assessment of the 2023 Turkey earthquakes: A comparison of two remote sensing methods* [Yüksek lisans tezi]. https://www.uppsatser.se/uppsats/0ddd9bebc3/ — *(yazarın tam adı ve üniversitesi tez sayfasından teyit edilmelidir)*
- Ramachandran, A., Yadav, A., & Schroeder, A. (2025). Implementation of remote-sensing models to identify post-disaster health facility damage: Comparative approaches to the 2023 earthquake in Turkey. *PLOS Digital Health*. https://pmc.ncbi.nlm.nih.gov/articles/PMC12558478
- Reitman, N. G., Briggs, R. W., Barnhart, W. D., Hatem, A. E., Thompson Jobe, J. A., DuRoss, C. B., Gold, R. D., Mejstrik, J. D., Collett, C., Koehler, R. D., & Akçiz, S. (2023). Rapid surface rupture mapping from satellite data: The 2023 Kahramanmaraş, Turkey (Türkiye), earthquake sequence. *The Seismic Record, 3*(4), 289–298. https://doi.org/10.1785/0320230029
- Robinson, C., Gupta, R., Fobi Nsutezo, S., Pound, E., Ortiz, A., Rosa, M., White, K., Dodhia, R., Zolli, A., Birge, C., & Lavista Ferres, J. M. (2023b). *Turkey earthquake report* (MSR-TR-2023-7). Microsoft Research. https://www.microsoft.com/en-us/research/publication/turkey-earthquake-report/
- Russo, L., Tapete, D., Ullo, S. L., & Gamba, P. (2025). *A deep learning framework for building damage assessment using VHR SAR and geospatial data: Demonstration on the 2023 Türkiye earthquake*. arXiv:2506.22338. https://arxiv.org/abs/2506.22338
- Xia, H., Wu, J., Yao, J., Zhu, H., Gong, A., Yang, J., Hu, L., & Mo, F. (2023). A deep learning application for building damage assessment using ultra-high-resolution remote sensing imagery in Turkey earthquake. *International Journal of Disaster Risk Science, 14*(6), 947–962. https://doi.org/10.1007/s13753-023-00526-6

**Operasyonel sistemler**

- Manzini, T., Perali, P., & Murphy, R. R. (2025a). *Deploying rapid damage assessments from sUAS imagery for disaster response*. arXiv:2511.03132. https://arxiv.org/abs/2511.03132
- Manzini, T., Perali, P., Murphy, R. R., & Merrick, D. (2025b). Challenges and research directions from the operational use of a machine learning damage assessment system via small uncrewed aerial systems at Hurricanes Debby and Helene. *IEEE RO-MAN 2025*. https://arxiv.org/abs/2506.15890
- Robinson, C., Fobi Nsutezo, S., Ortiz, A., Sederholm, T., Dodhia, R., Birge, C., Richards, K., Pitcher, K., Duarte, P., & Lavista Ferres, J. M. (2023a). Rapid building damage assessment workflow: An implementation for the 2023 Rolling Fork, Mississippi tornado event. *Proceedings of the IEEE/CVF ICCV Workshops*, 3760–3764. https://arxiv.org/abs/2306.12589
- Google Research. (t.y.). *SKAI* [Yazılım]. https://github.com/google-research/skai
- UN Global Pulse. (t.y.). *AI from Google Research and UN boosts humanitarian disaster response*. https://www.unglobalpulse.org/ai-from-google-research-and-un-boosts-humanitarian-disaster-response-wider-coverage-faster-damage-assessments
- WFP Innovation Accelerator. (t.y.). *SKAI*. https://innovation.wfp.org/node/57

**Kurumsal kaynaklar ve veri**

- Amazon Web Services. (t.y.). *Maxar Open Data Program* — Registry of Open Data on AWS. https://registry.opendata.aws/maxar-open-data/
- Applied Technology Council. (t.y.). *ATC-20: Procedures for postearthquake safety evaluation of buildings*. https://atcouncil.org/atc-20
- Copernicus EMS. (2023). *EMSR648 – Earthquake in East Anatolian Fault Zone*. https://mapping.emergency.copernicus.eu/activations/EMSR648
- Copernicus EMS. (2023). *Information Bulletin 165 – The Copernicus Emergency Management Service assesses the damage caused by the earthquake in Türkiye*. https://e.copernicus.eu/CEMS_IB165
- Copernicus EMS. (t.y.-a). *Rapid Mapping manual: Detection methods – damage assessment*. https://mapping.emergency.copernicus.eu/about/rapid-mapping-manual/detection-methods-damage-assessment/
- Copernicus EMS. (t.y.-b). *Data set description (damage grading field)*. https://emergency.copernicus.eu/mapping/ems/facilities
- HeiGIT. (2023). *Analysing changes in OSM after the earthquake in Syria and Türkiye in February 2023*. https://giscienceblog.uni-heidelberg.de/?p=18385
- OpenStreetMap Wiki. (2023). *2023 Turkey Earthquakes*. https://wiki.openstreetmap.org/wiki/2023_Turkey_Earthquakes
- Source Cooperative. (t.y.). *Maxar Open Data (ayna)*. https://source.coop/maxar/maxar-opendata
- Vantor. (t.y.). *Open Data Program*. https://vantor.com/company/open-data-program/

**Kullanılabilirlik**

- Bangor, A., Kortum, P. T., & Miller, J. T. (2008). An empirical evaluation of the System Usability Scale. *International Journal of Human–Computer Interaction, 24*(6), 574–594. https://doi.org/10.1080/10447310802205776
- Bangor, A., Kortum, P., & Miller, J. (2009). Determining what individual SUS scores mean: Adding an adjective rating scale. *Journal of Usability Studies, 4*(3), 114–123.
- Brooke, J. (1996). SUS: A "quick and dirty" usability scale. In P. W. Jordan vd. (Ed.), *Usability evaluation in industry* (ss. 189–194). Taylor & Francis.

---

## Ek: Başvuru Metni İçin Doğrulama Notları

Literatür taraması sırasında yol haritasındaki bazı atıfların netleştirilmesi gerektiği görüldü:

1. **Reitman vd. (2023)** bina hasarı değil fay yüzey kırığı çalışmasıdır. Başvuruda yalnızca "Maxar görüntülerinin depremden ne kadar sonra erişilebilir olduğu" için kullanılmalıdır.
2. **Manzini vd. (2025)**, İHA görüntüsüne dayanır. Drone kapsam dışına çıkarıldığı için yalnızca operasyonel dersler bağlamında anılmalı; "kullanıcılar doğruluktan çok iş akışı uyumunu önemsiyor" ifadesi tam metinden teyit edilmelidir.
3. **Manzini vd. (2024), CRASAR-U-DROIDs** veri seti, drone kapsam dışı kaldığı için metinden ve kaynakçadan çıkarılabilir.
4. **Karlberg (2023)** hakemli bir dergi makalesi değil, yüksek lisans tezidir; başvuruda bu şekilde belirtilmelidir.
5. **Copernicus "40 saat"** bilgisi Bilgi Bülteni 165'ten doğrulanmıştır ve başvuruda bu kaynakla atıflanabilir.
