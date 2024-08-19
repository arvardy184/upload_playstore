# upload_playstore
Tentu, saya akan merapikan dan memformat ulang dokumentasi Anda dalam format Markdown (.md). Berikut hasilnya:

```markdown
# Panduan Publikasi Aplikasi Flutter di Google Play Store

## 1. Persiapan Aplikasi

- Pastikan aplikasi sudah siap untuk dirilis (fitur lengkap, bebas bug, dll).
- Siapkan aset grafis (ikon, screenshot, banner) sesuai persyaratan Play Store.
- Generate ikon dengan package `flutter_launcher_icons`.

## 2. Pengaturan App Signing

Buat keystore untuk signing aplikasi:

### macOS atau Linux:
```bash
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA \
-keysize 2048 -validity 10000 -alias upload
```

### Windows (PowerShell):
```powershell
keytool -genkey -v -keystore $env:USERPROFILE\upload-keystore.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

**Catatan:** Simpan keystore dan password dengan aman.

## 3. Membuat File `android/key.properties`

Buat file `key.properties` di folder `android` dengan isi:

```properties
storePassword=your_keystore_password
keyPassword=your_key_password
keyAlias=your_key_alias
storeFile=/path_to_your_keystore/your_key_name.jks
```

## 4. Konfigurasi Gradle

Edit file `android/app/build.gradle`:

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

serta ubah isi kode buildTypes dari : 

 buildTypes {
      release {
         // TODO: Add your own signing config for the release build.
         // Signing with the debug keys for now,
         // so `flutter run --release` works.
           signingConfig signingConfigs.debug
      }
   }

 menjadi :

android {
    ...
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

## 5. Persiapkan App Bundle

```bash
flutter clean
flutter build appbundle
```

## 6. Buat Akun Google Play Developer

- Daftar di [Google Play Console](https://play.google.com/console/).
- Bayar biaya pendaftaran one-time sebesar $25.

## 7. Buat Aplikasi Baru di Play Console

- Masuk ke Play Console.
- Klik "Create app".
- Isi informasi dasar aplikasi.

## 8. Isi Informasi Store Listing

Lengkapi informasi berikut di dashboard:
- Privacy policy
- App access
- Ads
- Content rating
- Target audience
- News apps
- COVID-19 contact tracing and status apps
- Data safety
- Government apps
- Financial features
- App category
- Set up store listing (nama aplikasi, deskripsi, ikon, screenshot, dll)

## 9. Review Kebijakan

- Pastikan aplikasi mematuhi kebijakan program developer Google Play.
- Isi formulir "App content" untuk mendeklarasikan konten dan fitur aplikasi.

## 10. Test Aplikasi

- Pilih internal testing dan upload app bundle.
- Undang minimal 20 tester.
- Jalankan pengujian selama 2 minggu.

## 11. Buat Production Release

- Di bagian "App releases", pilih track production.
- Upload file App Bundle (`.aab`) yang telah dibuat beserta release notes.
- Tunggu sampai aplikasi selesai di-review dan berhasil dipublikasi di Play Store.

## 12. Publikasikan Aplikasi

- Setelah semua langkah selesai, klik "Review and publish".
- Google akan mereview aplikasi Anda (biasanya memakan waktu beberapa jam hingga beberapa hari).

## 13. Monitoring dan Update

- Pantau statistik dan ulasan pengguna di Play Console.
- Perbarui aplikasi secara berkala.

## Update Versi Aplikasi

Untuk memperbarui versi aplikasi, edit file `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

- Angka pertama: versi utama
- Angka kedua: versi minor
- Angka ketiga: versi patch
- Angka setelah '+': nomor build
