# Yapılandırma Görevi: Modern Altyapı ve Sunucu Sıkılaştırma

## 1. Giriş
Bu görev kapsamında; Next.js full-stack uygulamaları, Docker konteynerizasyon süreçleri, Nginx Reverse Proxy katmanı ve cPanel tabanlı VPS (Sanal Özel Sunucu) mimarilerinin siber savunma derinliği (Defense-in-Depth) prensiplerine göre sıkılaştırılması incelenmiştir.

## 2. Mimari Katmanlar ve Güvenlik Konfigürasyonları

### 2.1 Nginx Reverse Proxy Sıkılaştırması
* **Sürüm Gizleme:** `nginx.conf` içerisinde `server_tokens off;` yapılandırılarak saldırganların ayak izi (fingerprinting) toplama aşaması baltalanmıştır.
* **Güvenlik Başlıkları (Security Headers):** HTTP yanıtlarına `X-Frame-Options: DENY` (Clickjacking koruması), `X-Content-Type-Options: nosniff` (MIME-Sniffing koruması) ve `Content-Security-Policy` (XSS koruması) katı kurallarla eklenmiştir.

### 2.2 Docker ve Konteyner İzolasyonu
* **Rootless Execution:** Konteyner içerisindeki süreçlerin ana sistemde (host) root yetkisiyle çalışmasını engellemek için `USER node` veya `USER nonroot` direktifleri zorunlu kılınmıştır.
* **Ağ İzolasyonu (Network Isolation):** Veritabanı ve Redis servisleri dış dünyaya kapatılarak sadece iç ağda (`internal bridge network`) uygulama konteyneriyle haberleşecek şekilde izole edilmiştir.

### 2.3 Next.js Güvenlik Dinamikleri
* **Ortam Değişkenleri Güvenliği:** Sadece istemciye gitmesi gereken değişkenler `NEXT_PUBLIC_` önekiyle sınırlandırılmış, kritik API anahtarları sunucu tarafında (Server-side runtime) kilitlenmiştir.

### 2.4 VPS ve cPanel Güvenlik Mimarisi
* **SSH Sıkılaştırma:** Varsayılan port (22) değiştirilmiş, şifreli girişler kapatılarak yalnızca 4096-bit RSA anahtarlarıyla erişim izni verilmiştir.
* **Brute Force ve Keşif Engelleme:** Fail2Ban entegrasyonu ile ardışık hatalı istek atan IP adresleri güvenlik duvarı (iptables/UFW) seviyesinde otomatik olarak engellenmiştir. cPanel/WHM üzerinde iki faktörlü kimlik doğrulama (2FA) aktif edilmiştir.

---
## 📊 Laboratuvar Kanıtları ve Ekran Görüntüleri
Bu analizdeki defansif kodlama ve anomali tespit motorunun laboratuvar ortamındaki başarı kanıtları aşağıda listelenmiştir:

*(Görseller GitHub uzak deposu üzerinden güvenli ve güncel versiyonlarıyla senkronize edilmiştir).*
