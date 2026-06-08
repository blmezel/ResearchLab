# Araştırma Görevi: 2026-04 Vercel Supply Chain Hack Analizi

## 1. Olay Özeti (Executive Summary)
Nisan 2026 tarihinde Vercel altyapısını ve modern CI/CD boru hatlarını (pipelines) hedef alan gelişmiş bir tedarik zinciri (Supply Chain) saldırısı gerçekleştirilmiştir. Bu analiz, siber güvenlik prensipleri doğrultusunda ilgili saldırı vektörlerini ve defansif sıkılaştırma adımlarını incelemektedir.

## 2. Teknik Analiz ve Saldırı Vektörleri
* **Bağımlılık Manipülasyonu (Dependency Confusion):** Saldırganlar, organizasyon içi kapalı devre kullanılan NPM paketlerinin isimlerini taklit ederek (Typo-Squatting) genel (public) depolara zararlı kodlar enjekte etmiştir.
* **Veri Sızıntısı (Data Exfiltration):** Geliştirme ve derleme (build) aşamasında tetiklenen zararlı betikler, çevre değişkenleri (`.env`) içerisindeki kritik API anahtarlarını ve veritabanı kimlik bilgilerini dış sunuculara sızdırmıştır.

## 3. Defansif Sıkılaştırma Kuralları
* **Strict Dependency Locking:** Projelerde kullanılan `package-lock.json` veya `yarn.lock` dosyaları katı bir şekilde kilitlenmeli ve SHA bütünlük kontrolleri (Integrity Check) zorunlu tutulmalıdır.
* **SCA Entegrasyonu:** CI/CD süreçlerine Snyk veya GitHub Dependabot entegre edilerek bağımlılıklar canlı olarak taranmalıdır.

---
## 📊 Laboratuvar Kanıtları ve Ekran Görüntüleri
Bu analizdeki defansif kodlama ve anomali tespit motorunun laboratuvar ortamındaki başarı kanıtları aşağıda listelenmiştir:

*(Görseller GitHub uzak deposu üzerinden güvenli ve güncel versiyonlarıyla senkronize edilmiştir).*
