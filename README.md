# Twinspor FC - Android uygulaması

Site, bir WebView içinde uygulama gibi açılır (tam ekran, kendi ikonu, geri tuşu, fotoğraf seçici, alert/onay pencereleri).
Site dosyaları `app/src/main/assets/www/` içine gömülü gelir.

## APK'yı almanın 3 yolu

### A) Bilgisayar yok - GitHub ile (en kolayı)
1. github.com'da ücretsiz hesap aç, yeni bir **boş repo** oluştur.
2. Bu klasörün **içindekileri** (settings.gradle, app/, .github/ ...) repoya yükle ("Add file > Upload files").
3. Repoda **Actions > "APK derle" > Run workflow** de. 3-5 dakika sonra bitince çalıştırmanın sayfasında **twinspor-fc-apk** dosyası çıkar. İndir, zip'ten çıkan `app-debug.apk` dosyasını telefona atıp yükle (bilinmeyen kaynaklara izin vermen gerekir).

### B) Android Studio ile
1. Android Studio'yu kur, bu klasörü **Open** ile aç, Gradle eşitlemesini bekle.
2. **Build > Build APK(s)** de. APK `app/build/outputs/apk/debug/` içinde çıkar.

### C) Komut satırı
`gradle assembleDebug` (Android SDK ve JDK 17 kurulu olmalı).

## Google Play için (imzalı sürüm)
1. Anahtar oluştur (bir kere, dosyayı ve şifreyi KAYBETME, güncellemeler hep bununla imzalanır):
   `keytool -genkey -v -keystore twinspor.jks -keyalg RSA -keysize 2048 -validity 10000 -alias twinspor`
2. Proje köküne `keystore.properties` oluştur:
```
storeFile=twinspor.jks
storePassword=...
keyAlias=twinspor
keyPassword=...
```
3. Android Studio: **Build > Generate Signed Bundle / APK > Android App Bundle (.aab)**. Play Console'a yükle. Mağaza ikonu: `play-store-icon-512.png`.
4. Play Console'da gizlilik politikası linki istenir.

## Siteyi güncelleyince
- **Gömülü mod (varsayılan):** yeni site dosyalarını `app/src/main/assets/www/` içine kopyala, `app/build.gradle` içinde `versionCode`'u 1 artır ve yeniden derle.
- **Canlı mod (önerilen, siten yayındaysa):** `app/build.gradle` içindeki `START_URL` satırına site adresini yaz, örn: `buildConfigField "String", "START_URL", "\"https://siten.com/index.html\""`. Uygulama o adresi açar; site güncellenince APK'yı yeniden yapmana gerek kalmaz.

## Önemli
- Gömülü modda site `https://appassets.androidplatform.net` adresinden açılır. Firebase Console > Authentication > Settings > **Authorized domains** listesine `appassets.androidplatform.net` ekle. Firebase API anahtarını alan adıyla kısıtladıysan (Google Cloud > Credentials) oraya da bu adresi ekle. Canlı modda kendi alan adın zaten yetkili olduğu için bu gerekmez.
- Google ile giriş kullanmıyorsan (e-posta/şifre) başka ayar gerekmez.
