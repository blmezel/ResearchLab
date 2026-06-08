# Araştırma Görevi: 2026-04 Vercel Supply Chain Hack ve Appwrite Kurulum Analizi

## 1. Olay Özeti (Executive Summary)
Nisan 2026 tarihinde Vercel altyapısını hedef alan tedarik zinciri (Supply Chain) saldırısı, modern CI/CD boru hatlarındaki dışa bağımlılık risklerini göz önüne sermiştir. Bu rapor, Vercel vakası ile Vize projemiz olan **Appwrite Web Security Audit** kapsamındaki kurulum betiği (`install.sh`) zafiyetlerini karşılaştırmalı olarak analiz etmektedir.

## 2. Teknik Analiz ve Zafiyet Vektörleri (Vize Entegrasyonu)
* **Bağımlılık Manipülasyonu (Vercel Vakası):** Saldırganlar, organizasyon içi paket isimlerini taklit ederek (Typo-Squatting) genel depolara zararlı kodlar enjekte etmiş ve derleme aşamasında `.env` sırlarını dışarı sızdırmıştır.
* **Körlemesine Çalıştırma Riski (Appwrite Vize Bulgusu):** Vize projesi "1-Setup-Analysis" aşamasında tespit edildiği üzere; Appwrite kurulum betiklerinde `curl | bash` mantığı kullanılmış, ancak indirilen paketlerin **SHA-256 Checksum doğrulaması** yapılmamıştır. Bu durum, kurulum aşamasında doğrudan bir Ortadaki Adam (MiTM) ve tedarik zinciri zehirlenmesi riskine yol açmaktadır.

## 3. Defansif Sıkılaştırma Kuralları ve DevSecOps Çözümleri
Her iki tedarik zinciri zafiyetini (Vercel NPM ve Appwrite Bash) engellemek için laboratuvarımızda şu standartlar zorunlu kılınmıştır:
* **Strict Dependency Locking:** Node.js projelerinde `package-lock.json` katı bir şekilde kilitlenmeli, dışarıdan çekilen bash scriptleri çalıştırılmadan önce `sha256sum -c` ile bütünlük testinden (Integrity Check) geçirilmelidir.
* **SCA ve Pipeline Otomasyonu:** Vize projesinde uygulanan GitHub Actions (`security.yml`) tabanlı otomatik güvenlik taramaları (Security Scan), final projesi CI/CD süreçlerine standart olarak entegre edilmiştir.

---
## 📊 Laboratuvar Kanıtları
Bu analizdeki tedarik zinciri koruma mantığının ve bütünlük doğrulama mekanizmalarının laboratuvar ortamındaki başarı kanıtları, bu dizin altındaki ilgili ekran görüntüleriyle sunulmuştur.
