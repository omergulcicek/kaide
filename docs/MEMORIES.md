# MEMORIES

Bu dosya, proje içinde alınan kritik teknik kararların, mimari tercihlerin ve önemli trade-off'ların kısa notlar halinde kaydedilmesi için kullanılır. AI, tekrar eden hataları önlemek ve tutarlı kararlar almak için bu kayıtları referans alır.

## Güncelleme periyodu

- **Ne zaman ekle:** Mimari değişiklik, geniş kapsamlı refactor, yeni dependency/stack kararı, tekrarlanan hatanın çözümü veya önemli trade-off sonrası hemen yeni kayıt eklenir.
- **Ne zaman güncelle:** Bir karar iptal edildiğinde veya geçersiz kaldığında ilgili madde güncellenir veya "Superseded by …" notu eklenir.
- **Periyodik kontrol:** Her büyük milestone veya planlama öncesi bu dosya gözden geçirilir; artık geçerli olmayan maddeler arşivlenir veya kaldırılır.

## Kayıt formatı

Her kayıt aşağıdaki yapıda tutulur. Tarih `YYYY-MM-DD`, başlık kısa ve kararın özünü yansıtacak şekilde yazılır.

```markdown
## [YYYY-MM-DD] - Kısa karar başlığı

- **Bağlam:** Kararın alındığı durum (opsiyonel, karmaşıksa yazılır).
- **Karar:** Ne yapıldığı / ne tercih edildiği.
- **Gerekçe:** Neden bu yol seçildi (tek satır yeterli).
```

---

<!-- Aşağıya kayıtlar eklenir -->
