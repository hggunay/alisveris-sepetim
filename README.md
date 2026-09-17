# Alışveriş Sepetim

Ailenin ortak alışveriş listesi. Tek sayfa, Firebase Realtime Database.
**Anne ve baba da kullanıyor — sadelik bir tercih değil, şart.**

> 🤖 **Claude ile çalışıyorsan önce bunu oku.** Aşağıdaki kararlar Gökşin'in;
> "şunu da ekleyelim" önerisi yapmadan önce ⛔ listesine bak.

## Dosyalar

| Dosya | Ne |
|---|---|
| `index.html` | Uygulamanın **tamamı** (HTML + CSS + JS tek dosyada) |
| `manifest.json` | Ana ekrana "uygulama gibi" eklenmesi için. Simge altındaki ad: **Sepetim** |
| `icon-192.png`, `icon-512.png` | `alışveriş sepetim.jfif`'ten (1024×1024) üretildi |
| `firebase-kurallari.json` | Firebase → Realtime Database → **Rules** sekmesine yapıştırılan kurallar |
| `README.md` | bu dosya |

Yayın: GitHub `hggunay/alisveris-sepetim` → GitHub Pages. Güncelleme = GitHub web
arayüzünden dosya yükleme. Firebase projesi: `alisveris-sepetim-d5b10` (Belçika / europe-west1).

## ⛔ EKLENMEYECEKLER (Gökşin'in kararı, 2026-08-31)

Kategori · fiyat · market · ikiden fazla öncelik seviyesi · menü · sekme.

## Kararlar

- **Tek ekran.** Altta büyük **+**. Ürüne dokununca: ✅ Aldım / ✏️ Düzenle / 🗑️ Sil.
  Alınmış ürüne dokununca: ↩️ Alınmadı, geri al / 🗑️ Sil.
- **Alanlar:** ürün adı (zorunlu), not (isteğe bağlı), **Acil / Acil değil**. Başka alan yok.
  Öncelik yasağın bilinçli istisnası: *"mantı yapılırken yoğurt yok, biri de dışarıda"*.
- **Renk = durum:** yeşil = bugün eklendi · beyaz = normal · mavi = alındı. Acil = kırmızı sol çizgi + "🔴 Acil" bölümü.
- **Alındı:** üstü çizilir, **kim aldı + saat** görünür, **48 saat sonra silinir**
  (sunucu yok — silmeyi listeyi açan telefon yapıyor: `eskileriTemizle`).
- **Tekrar eden ürün:** Gökşin: *"herkes küçük harfle yazacak, klavyede yan tuşa basacak."*
  - Birebir aynı (büyük/küçük harf, Türkçe harf, noktalama yok sayılır) → "zaten listede"
  - Son 48 saatte alınmış aynısı → "Baba aldı · 18:40, yine de ekle?"
  - **Tek yazım farkı** (eksik/yanlış harf, yer değiştirme) → "Listede Yoğurt var. Aynısı mı?"
    Yalnızca **4 harf ve üstü** adlarda: süt/set, un/su bambaşka ürün.
  - Hiçbiri engellemiyor, SORUYOR.
- **Aile adı + 4 haneli PIN.** Gökşin: *"çok güvenlikli olmak zorunda değil, aile içinde ve 1-2 yakın arkadaş."*
  - **PIN hiçbir yere yazılmıyor.** Aile adı + PIN → SHA-256 → 64 harflik anahtar;
    liste `aileler/<anahtar>` altında. Kurallar tüm aileleri listelemeyi reddediyor —
    adı ve PIN'i bilmeyen listeyi **bulamaz**. Banka kasası değil: adı bilen biri
    10.000 PIN'i deneyebilir. Market listesi için yeterli bulundu.
  - Aile adı sadeleştiriliyor: "Günay" = "gunay" = " GÜNAY ".
  - **Yanlış yazılan ad sessizce yeni liste AÇMIYOR** — "bulunamadı, yeni liste mi?" diye soruyor.
    Sebep: Keeper's Log'da yanlış kullanıcı adı yeni hesap açıyordu (Gökşin fark etti).
- **Oturum açık kalır**, her girişte PIN sorulmaz. Telefonda saklanan: anahtar, aile adı, kişi adı.
- **Kişi adını herkes kendi yazar**; o ailede daha önce geçen adlar dokunulabilir öneri olarak çıkar.
  Misafir (ör. 1-2 ay kalan kardeş) aynı aile adı + PIN'le katılır.
- **Bildirim YOK.** Telefonu titretmek sunucu ister; kullanım "eve gelmeden bak" alışkanlığına dayanıyor.
- **Firebase kütüphanesi 9.23.0 compat** — Keeper's Log'da çalışan sürüm, bilerek aynısı.
- **Service worker YOK** (bilerek): eski sürümün telefonda takılı kalması riski, faydasından büyük.

## Deneme modu

Adresin sonuna `?deneme` eklenirse (`…/alisveris-sepetim/?deneme`) liste yalnızca o
tarayıcının hafızasında durur, **gerçek veriye dokunulmaz**, üstte mor şerit çıkar.
Yeni bir şey denerken önce böyle dene.

## Bilinen davranışlar

- Firebase **boş düğüm saklamıyor**: son ürün silinince `urunler` `null` döner (kod buna hazır).
- Açılışta Firebase bir an "bağlı değil" diyor — bağlantı şeridi 3 sn bekleyip öyle çıkıyor
  (hemen çıksaydı her açılışta yanıp sönerdi; Ashbless'te aynı kusur yaşandı).
- Firebase kuralları `ad` için en fazla 60 harf istiyor; formdaki `maxlength` da 60. Birini değiştirirsen ikisini de değiştir.
