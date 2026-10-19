# Pomodori QR Menü

Mobil öncelikli, 5 dilli (TR/EN/DE/IT/AR) statik menü. Derleme adımı yok; GitHub Pages'te çalışır.

## Fiyat veya ürün güncelleme
Güncellemeler Claude ile sohbet üzerinden yapılır: `data/` dosyaları düzenlenir ve `main`'e gönderilir (admin paneli ayrı arşivde, yayında değil).

- **Fiyat:** yalnızca `data/prices.json` (tek kaynak, tüm diller otomatik güncellenir).
- **Ürün metni:** `data/menu-<dil>.json`. Ürünler dillerde `id` ile eşleşir; yeni ürün için 5 dosyaya aynı `id` ile ekleyin ve `prices.json`'a fiyatı yazın.
- `main`'e push edince `.github/workflows/pages.yml` `data/version.json`'a commit SHA yazar. QR kod değişmez.

## Önbellek
GitHub Pages `Cache-Control` başlıklarını değiştirmeye izin vermez (varsayılan `max-age=600`).
Bu yüzden: `version.json` her açılışta doğrulanır (ETag → 304, birkaç bayt), diğer veri dosyaları `?v=<sürüm>` ile istenir.
Sürüm değişmediyse dosyalar tarayıcı önbelleğinden gelir; değiştiyse hemen yenilenir. `index.html` dosyasına veri gömülmez.
İlk açılışta yalnızca seçili dilin JSON'u + `prices.json` + `version.json` indirilir (≈ 6–8 KB/dil, sıkıştırılmadan).

## Fontlar (`fonts/`)
- **Copperplate** ticari bir fonttur; lisans sorumluluğu sizdedir. `fonts/copperplate.otf` (31 KB, tek ağırlık; kalın yazı Android'de sentetik) kullanılıyor. WOFF2'ye çevirip (fonttools+brotli) yalnızca Latin + Türkçe karakterlere subset etmek boyutu azaltır. iPhone/Mac'te cihazda kuruluysa (`local()`) hiç indirilmez.
  Dosya yoksa başlıklar Optima/Trebuchet/sans-serif yedeğine düşer (sayfa yine okunaklı).
- **Arapça:** `fonts/arabic-subset.woff2` (isteğe bağlı; yalnızca Arapça seçilince indirilir). Yoksa cihaz Arapça fontu kullanılır (iOS Geeza Pro, Android Noto Naskh/Sans Arabic).
  Lisansı uygun (ör. OFL) bir fontu yalnızca gerekli Arapça + rakam aralığıyla subset edip ekleyin.
- Logo: Word'de görsel yoktu; "POMODORI" Impact ile yazılmış metindi. Android'de Impact yoktur; marka SVG logonuz varsa `.logo` yerine koyun.

## İnsan kontrolü
TR dışındaki çeviriler **taslaktır** (`"reviewStatus":"draft-needs-human-review"`). Yayından önce anadil konuşuru gözden geçirmeli.
