# Yapılandırma Görevi: Modern Altyapı Sıkılaştırma ve Konteyner İzolasyonu (Vize Entegrasyonu)

## 1. Giriş
Bu rapor; Next.js full-stack uygulamaları, Nginx Reverse Proxy katmanı, cPanel tabanlı VPS mimarileri ve Docker konteynerizasyon süreçlerinin siber savunma derinliği (Defense-in-Depth) prensiplerine göre sıkılaştırılmasını incelemektedir. Ayrıca vize projemiz olan Appwrite Web Security Audit kapsamında ele alınan Docker mimari analizi ve adli bilişim (forensics) temizlik süreçleri bu altyapıya mimari olarak entegre edilmiştir.

## 2. Mimari Katmanlar ve Güvenlik Konfigürasyonları

### 2.1 Docker ve Konteyner İzolasyonu (Vize Entegrasyonu)
* Ağ İzolasyonu (Vize 4. Aşama): Vize projesindeki Docker mimari analizinde uygulandığı üzere; mikroservislerin birbirine sıçramasını (lateral movement) engellemek için iç ağ köprüleri (internal bridge network) tasarlanmıştır. Final projesinde de veritabanı ve servis konteynerleri dış dünyaya kapatılarak sadece uygulama konteyneriyle izole ağda haberleşecek şekilde yapılandırılmıştır.
* Adli Bilisim ve Kalıntı Temizliği (Vize 2. Aşama): Olası bir sızma testi veya sistem ihlali sonrasında sunucuda iz bırakmamak ve konteyner güvenliğini doğrulamak adına vize projesinde kullanılan adli temizlik pratikleri sisteme dahil edilmiştir:
  docker compose down --volumes --rmi all --remove-orphans
  docker network prune -f
* Rootless Execution: Konteyner süreçlerinin ana sistemde root yetkisiyle çalışmasını engellemek için USER node veya USER nonroot direktifleri zorunlu kılınmıştır.

### 2.2 Nginx Reverse Proxy Sıkılaştırması
* Sürüm Gizleme: nginx.conf içerisinde server_tokens off; yapılandırılarak saldırganların ayak izi (fingerprinting) toplaması engellenmiştir.
* Güvenlik Başlıkları (Security Headers): HTTP yanıtlarına X-Frame-Options: DENY (Clickjacking), X-Content-Type-Options: nosniff (MIME-Sniffing) ve katı Content-Security-Policy (XSS) kuralları eklenmiştir.

### 2.3 Next.js ve VPS Güvenliği
* Ortam Değişkenleri: Kritik API anahtarları sadece sunucu tarafında (Server-side runtime) kilitlenmiş, sızmaları engellemek adına .env dosyası repodan uzak tutularak .env.example şablonu (Vize pratiklerine uygun olarak) sunulmuştur.
* VPS / SSH Sıkılaştırma: Varsayılan SSH portu (22) değiştirilmiş, şifreli girişler kapatılarak yalnızca 4096-bit RSA anahtarlarıyla erişim izni verilmiştir. Fail2Ban ile hatalı istek atan IP'ler otomatik olarak engellenmektedir.

---
## 📊 Laboratuvar Kanıtları
Bu analizdeki Nginx yapılandırmaları, Docker ağ izolasyon mimarisi ve vize projesinden aktarılan adli bilişim konteyner temizlik testlerinin başarı kanıtları, bu dizin altındaki ilgili ekran görüntüleriyle sunulmuştur.
