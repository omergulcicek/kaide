---
description: "Sadece commit mesaj standartlarını belirler. AI'nın her işlem sonunda uygun commit mesajı üretmesini sağlar."
globs: "**/*"
alwaysApply: false
---

# Git Commit Standartları

Tüm commit mesajlarını **Conventional Commits** standartlarına göre ve **İngilizce** olarak oluştur.

## 1. Mesaj Formatı
Format: `<tip>(<kapsam>): <açıklama>`

### Zorunlu Tipler (Types):
- **feat**: Yeni bir özellik ekleme.
- **fix**: Hata düzeltme.
- **docs**: Dokümantasyon değişikliği.
- **style**: Kodun anlamını etkilemeyen format değişikliği (boşluk, virgül vb.).
- **refactor**: Ne hata düzelten ne de özellik ekleyen kod değişikliği.
- **perf**: Performans iyileştirmesi.
- **test**: Test ekleme veya düzeltme.
- **chore**: Bakım, bağımlılık güncelleme veya build işlemleri.

## 2. Kurallar
- Tip (type) ve kapsam (scope) her zaman **küçük harf** olmalıdır.
- Açıklama kısmının sonuna **nokta koyma**.
- Mesajlar kısa, öz ve teknik terimlere uygun olmalıdır.
- Kapsam (scope) olarak ilgili `feature` adını veya `ui`, `api`, `deps` gibi genel alanları kullan.
