# Uydu Görüntüsünden Hasar Haritasına Web Sistemi

Afet öncesi ve afet sonrası uydu görüntülerinden (Maxar/Vantor Açık Veri) bina hasarını tespit eden, sonucu **yeşil / sarı / kırmızı bina haritası** ve **bölge öncelik listesi** olarak sunan açık kaynaklı, Türkçe bir web sistemi. Çalışma alanı: 2023 Kahramanmaraş depremleri.

```
Afet öncesi + afet sonrası GeoTIFF
        │
        ▼
[İP2] Veri hattı: eşleştir → 1024×1024 karolara böl
        │
        ▼
Siamese U-Net (Kahramanmaraş ince ayarlı) → piksel maskesi
        │
        ▼
[İP3] Maske → bina poligonları → renk → GeoJSON → öncelik skoru
        │
        ▼
[İP4] Web sitesi: bölge seç / görüntü yükle → harita → öncelik listesi → indir
        │
        ▼
[İP5] Test: 3 senaryo + süre ölçümü + SUS anketi
```

## Belgeler

| Belge | İçerik |
|---|---|
| [Literatür Taraması](docs/LITERATUR_TARAMASI.md) | Projenin dayandığı çalışmalar, literatürdeki boşluk ve iş paketleriyle eşleştirme |

## Başarı ölçütleri

| Ölçüt | Hedef |
|---|---|
| Öncelik sıralaması (model ↔ gerçek etiket) | Spearman ρ ≥ 0,7 |
| Yüklemeden haritaya süre (tek bölge) | ≤ 10 dakika |
| Simüle senaryo | ≥ 3 (Kahramanmaraş merkez, Antakya, İslahiye/Nurdağı) |
| Kullanılabilirlik | SUS ≥ 68 |

## Planlanan klasör yapısı

```
docs/        Belgeler (literatür taraması, model arayüzü)
notebooks/   Deneme not defterleri
src/
├── inference/   Eşleştirme, karolama, tahmin
├── geo/         Bina poligonları, öncelik skoru
└── sources/     Vantor/Maxar Açık Veri erişimi
tests/       Birim testleri
web/
├── backend/     FastAPI
└── frontend/    HTML + Leaflet
```

## Veri lisansı

Maxar/Vantor Açık Veri görüntüleri **CC BY-NC 4.0** lisanslıdır (ticari olmayan kullanım, kaynak gösterilerek). Doğrulama için Copernicus EMS EMSR648 hasar derecelendirme verileri kullanılmaktadır.

## Durum

Faz 0 (hazırlık ve ilk deneme) — başlamadı.
