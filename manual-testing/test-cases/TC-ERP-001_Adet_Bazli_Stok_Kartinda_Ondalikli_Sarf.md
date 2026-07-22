# TC-ERP-001 — Adet Bazlı Stok Kartında Ondalıklı Sarf İşlemi

## Genel Bilgiler

| Alan | Değer |
|---|---|
| Test Case ID | TC-ERP-001 |
| Modül | Depo |
| Ekran | Manuel Sarf |
| Test Türü | Fonksiyonel Test / Negatif Test / Karşılaştırmalı Test |
| Severity | High |
| Priority | High / Urgent |
| Durum | Fail |

## Başlık

**Adet bazında takip edilen stok kartında ondalıklı sarf yapılabilmesi**

## Kısa Açıklama

Manuel Sarf ekranında, stok birimi **adet** olan bir ürün için ondalıklı miktar girilebilmekte ve sistem bu işlemi başarıyla tamamlamaktadır. Adet bazlı ürünlerde yalnızca tam sayı kabul edilmesi beklenmektedir.

## Ön Koşullar

1. Sistemde stok birimi **adet** olan ve yeterli stok miktarına sahip aktif bir stok kartı bulunmalıdır.
2. Sistemde ondalıklı miktarla takip edilen aktif bir stok kartı bulunmalıdır.
3. Kullanıcının **Manuel Sarf** ekranına erişim ve işlem yapma yetkisi olmalıdır.
4. Her iki stok kartı da sarf işlemine uygun durumda olmalıdır.

## Test Verileri

| Stok Kartı Türü | Mevcut Miktar | Girilecek Sarf Miktarı | Amaç |
|---|---:|---:|---|
| Adet bazlı stok kartı | 100 adet | 20 adet | Geçerli tam sayı kontrolü |
| Adet bazlı stok kartı | 100 adet | 2,4 adet | Geçersiz ondalıklı değer kontrolü |
| Ondalıklı stok kartı | 16,8 birim | 2,4 birim | Geçerli ondalıklı değer kontrolü |

## Test Adımları

1. **Manuel Sarf** ekranını aç.
2. Stok birimi **adet** olan stok kartını seç.
3. Sarf miktarı alanına `20` gir ve işlemi tamamla.
4. İşlemin başarıyla gerçekleştiğini doğrula.
5. Aynı stok kartı için sarf miktarı alanına `2,4` gir.
6. İşlemi tamamlamayı dene.
7. Sistemin verdiği tepkiyi kaydet.
8. Ondalıklı takip edilen stok kartını seç.
9. Sarf miktarı alanına `2,4` gir ve işlemi tamamla.
10. İşlemin başarıyla gerçekleştiğini doğrula.

## Beklenen Sonuç

1. Stok birimi **adet** olan kart için sarf miktarı alanına `2,4` girildiğinde sistem işlemi gerçekleştirmemelidir.
2. Kullanıcıya, adet bazlı ürünlerde yalnızca tam sayı girilebileceğini belirten anlaşılır bir doğrulama mesajı gösterilmelidir.
3. Stok miktarında herhangi bir değişiklik oluşmamalı ve başarısız işlem kaydedilmemelidir.
4. Ondalıklı takip edilen stok kartında `2,4` değeri kabul edilmeli ve sarf işlemi başarıyla tamamlanmalıdır.

## Gerçekleşen Sonuç

1. Adet bazlı stok kartı için sarf miktarı alanına `2,4` girildiğinde sistem işlemi kabul etmektedir.
2. Sistem, sarf işleminin başarıyla tamamlandığını belirten olumlu bir mesaj göstermektedir.
3. Stok miktarı `100` adetten `97,6` adede düşmekte ve ondalıklı sarf işlemi sisteme kaydedilmektedir.
4. Ondalıklı takip edilen stok kartında `2,4` değeri de başarıyla işlenmektedir.

## İş Etkisi

Hata, adet bazında takip edilmesi gereken stokların ondalıklı miktarlara düşmesine ve stok verilerinin güvenilirliğini kaybetmesine neden olabilir.

Yanlış stok miktarları:

- Gereksiz satın alma yapılmasına,
- İhtiyaçların geç fark edilmesine,
- Üretim planlarının malzeme eksikliği nedeniyle aksamasına,
- Müşteri teslim tarihlerinin gecikmesine

yol açabilir. Hatanın stok, satın alma, planlama, üretim ve teslimat süreçlerini etkileyebilmesi nedeniyle iş etkisi yüksektir.

## Test Ortamı

### Masaüstü

- Cihaz: Dizüstü bilgisayar
- İşletim sistemi: Windows 11
- Tarayıcı: Microsoft Edge

### Mobil

- Cihaz: Realme 12 Pro 5G
- İşletim sistemi: Android 16
- Tarayıcı: Microsoft Edge Mobile

## Platformlar Arası Davranış Farkı

Masaüstü ortamında nokta karakteri kabul edilmemekte, virgül ondalık ayırıcı olarak kullanılabilmektedir.

Mobil ortamda da nokta kabul edilmemektedir; ancak virgülün sayının başında girilmesine izin verilmekte ve giriş ondalıklı bir değere dönüştürülmektedir.

Masaüstü ve mobil alan doğrulamalarının farklı davranması, platformlar arasında tutarsızlık oluşturmaktadır.

## İyileştirme Önerisi

Sistemde ürün veya stok kartı bazında miktar tipini belirleyen bir doğrulama kuralı bulunmalıdır.

- **Adet bazlı ürünlerde:** yalnızca tam sayı kabul edilmelidir.
- **Ondalıklı takip edilen ürünlerde:** tanımlı ondalık hassasiyet kadar girişe izin verilmelidir.
- Aynı doğrulama kuralları masaüstü ve mobil platformlarda tutarlı şekilde uygulanmalıdır.

## Ekler

- Ekran görüntüsü
- İşlem öncesi stok miktarı
- İşlem sonrası stok miktarı
- Başarı mesajı
- Mobil ve masaüstü davranış karşılaştırması
