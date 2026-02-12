Optimize Edilmiş Bağlam Mühendisliği Checklist'i
1. Teknik Standartlar (The Machine Layer)
[ ] Glob Spesifikasyonu: globs alanı sadece ilgili dosyaları hedefliyor mu? (Gereksiz her dosya, ajanın dikkatini dağıtan gürültüdür).

[ ] Karakter Sınırı: description alanı (AI'nın hangi kuralı ne zaman tetikleyeceğini anlaması için) kısa ve anahtar kelime odaklı mı?

[ ] Markdown Hiyerarşisi: Bilgiler rastgele metin yerine #, ## ve - şeklinde yapılandırılmış mı? (LLM'ler yapısal veriyi daha az token ile daha iyi anlar).

2. Operasyonel Protokoller (The Logic Layer)
[ ] Empirical Validation (Doğrulama Yasası): Ajanın bir görevi bitirdiğinde sadece "Bitti" demesi yerine, hangi komutla (örn: npm test, ls -R) çıktıyı doğrulaması gerektiği talimatı var mı?

[ ] Gereksiz Bilgi Ayıklama: Dosya içeriği sadece "kurallar" ve "yöntemler" mi içeriyor? (Teorik açıklamalar ve "neden" kısımları ana dosyadan docs/ altına taşınmalı).

[ ] Plan-Before-Action: Karmaşık değişiklikler öncesi ajanın önce bir PLAN.md oluşturması strateji kurması zorunlu mu?

3. Guardrails & Anti-Patterns (The Safety Layer)
[ ] "Asla" Listesi (Blacklist): Don't use 'any', Don't delete comments, Don't change @/constants gibi katı sınırlar mevcut mu?

[ ] Placeholder Yasağı: Kod bloklarında // ... existing code ... şeklinde yer tutucu bırakmaması ve tam kodu vermesi talimatı net mi?

[ ] Context Overload Koruması: Ajanın her seferinde tüm projeyi okumaya çalışması yerine, sadece ilgili modülleri @ ile çağırması gerektiği hatırlatılıyor mu?

4. Proje Hafızası (The Evolution Layer)
[ ] Memory Management: AGENTS.md veya MEMORIES.md dosyasına, projenin "tuhaflıklarını" (örneğin: "X kütüphanesi 2. versiyonda hata veriyor, 1.8 kullan") not etme görevi tanımlı mı?

[ ] Bağımlılık Zinciri: Bir kuralın başka bir .mdc dosyasını tetiklemesi gerekiyorsa, bu ilişki açıkça belirtilmiş mi?