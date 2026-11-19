# Kelime listelerinin kaynakları

Bu klasördeki dosyalar `tool/build_words.dart` ile üretilir. Ham dosyalar depoya girmez (`tool/raw/`).
İndirme tarihi: 2026-11-16.

| Ham dosya | Kaynak | Lisans | Kullanım |
|---|---|---|---|
| `tr_master.dict` | Zemberek-NLP, `morphology/src/main/resources/tr/master-dictionary.dict` — https://github.com/ahmetaa/zemberek-nlp | Apache License 2.0 | Türkçe geçerli kelimeler (sözlük maddeleri) |
| `enable1.txt` | ENABLE kelime listesi, https://github.com/dolph/dictionary üzerinden | Kamu malı | İngilizce geçerli kelimeler |
| `tr-extract.jsonl.gz` | Vikisözlük (tr.wiktionary.org), kaikki.org / wiktextract çıkarımı — https://kaikki.org/trwiktionary/ | CC BY-SA 4.0 (kaikki ayrıca GFDL belirtir) | Türkçe kelime anlamları |
| `english-wordnet-2025-json.zip` | Open English WordNet 2025 — https://github.com/globalwordnet/english-wordnet | CC BY 4.0 | İngilizce kelime anlamları |
| `tr_50k.txt`, `en_50k.txt` | FrequencyWords (OpenSubtitles 2018) — https://github.com/hermitdave/FrequencyWords | İçerik CC BY-SA 4.0 | Cevapların yaygınlığa göre seçilmesi |

## Üretim kuralları

- Büyük harfe çevrilir; 4–7 harf dışındaki ve modülün alfabesinde olmayan harf içeren kelimeler atılır.
- Türkçe: özel adlar, kısaltmalar ve noktalama girdileri atılır; `â î û` → `a i u`.
- `valid_<n>.txt`: sözlükteki tüm uygun kelimeler.
- `answers_<n>.txt`: sıklık listesindeki sırayla, sözlükte de bulunan ilk 2000 kelime. İngilizcede yalın çoğullar (kökü sözlükte olan `-S`) cevap olmaz.
- `tool/blocklist_<tr|en>.txt` varsa içindeki kelimeler cevap olmaz.

### Anlamlar (`tool/build_defs.dart`)

- İngilizce arşiv önce `tool/raw/oewn/` klasörüne açılır; betik oradan okur.
- `defs_<n>.txt`: satır başına `KELİME<TAB>tanım 1<TAB>tanım 2`. Yalnızca `valid_<n>.txt` içindeki kelimeler; tanımı olmayan kelime yazılmaz.
- Kelime başına en çok iki tanım, tanım başına en çok 160 karakter.
- Türkçe: yalnızca Türkçe başlıklar; özel adlar, öbekler, yalnızca çekim bildiren anlamlar ve "X kavramının yanlış kullanımı" türü yazım notları atılır. Tanımlarda kalan vikimetin işaretleri (`[[…|`, `]]`, `{{…}}`) temizlenir.
- İngilizce: kelimenin kendi tanımı yoksa köküne indirgenir (`-S`, `-ED`, `-ING` vb. ve küçük bir düzensiz biçim tablosu) ve kökün tanımı kullanılır.
- Betik, cevap kelimelerinin Türkçede %95'inden, İngilizcede %88'inden azı tanımlıysa hata verir.
- Cevap kelimelerinin kapsamı (2026-11-17): Türkçe %96,9–98,0; İngilizce %90,6–97,8.
- Tanım dosyalarının toplam boyutu: 4295 KB (2026-11-17).

## Lisans notu

`answers_*.txt` dosyaları CC BY-SA 4.0 lisanslı sıklık verisinden türetildiği için aynı lisansla paylaşılır.
`tr/defs_*.txt` dosyaları Vikisözlük'ten türetilmiştir ve CC BY-SA 4.0 ile paylaşılır.
`en/defs_*.txt` dosyaları Open English WordNet'ten türetilmiştir ve CC BY 4.0 ile paylaşılır.
Vikisözlük'ün telif sayfası politika metninde CC BY 2.5, sayfa altlığında CC BY-SA 4.0 der; daha kısıtlayıcı olan esas alınmıştır.
Uygulama içinde kaynaklar belirtilmelidir (Ayarlar → Kaynaklar ve lisanslar).

Açık risk: CC BY-SA, içeriğe kopya koruması uygulanmasını yasaklar. Gömülü CC BY-SA verisinin App Store'un korumasıyla uyumu belirsizdir; mağazaya çıkmadan önce (Aşama 3) karara bağlanmalıdır.

## Bilinen sınırlar

- Türkçe sözlük madde başlarını içerir; çekimli biçimlerin çoğu (ör. çoğul ekli kelimeler) tahmin olarak kabul edilmez.
- İngilizcede köke indirgenen kelimeler kökün ilk tanımını alır; tanımın sözcük türü çekimli biçimle uyuşmayabilir (ör. `RATED` için "rate" adının tanımı).
- Vikisözlük tanımları gönüllü katkısıdır; elle ayıklanmamıştır.
- Sıklık verisi film altyazılarından geldiği için cevaplar arasında argo kelimeler bulunabilir; `tool/blocklist_*.txt` ile ayıklanabilir.
