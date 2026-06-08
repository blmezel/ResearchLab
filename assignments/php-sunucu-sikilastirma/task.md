# Yapılandırma Görevi: PHP Sunucu Sıkılaştırma ve Güvenli Dosya Yönetimi (Vize Entegrasyonu)

## 1. Giriş
Bu rapor; üretim (production) ortamındaki bir PHP web sunucusunun siber güvenlik standartlarına (OWASP, CIS) göre nasıl sıkılaştırılacağını ve vize projemiz olan **Appwrite Web Security Audit** kapsamında tespit edilen güvensiz nesne referansı (IDOR) ve yetkisiz dosya erişim zafiyetlerinin sunucu seviyesinde nasıl engelleneceğini mimari olarak analiz etmektedir.

## 2. php.ini Güvenlik Konfigürasyonları ve Vize IDOR Çözümü
* **`open_basedir = "/home/cybella/public_files/"` (Final Sıkılaştırması):** PHP betiklerinin erişebileceği dizin sınırını katı bir şekilde izole eder. Bu kalkan, olası bir kod enjeksiyonunda saldırganın `../../etc/passwd` gibi kritik sistem dosyalarına erişmesini (Path Traversal) engeller.
* **Directory Listing & IDOR Kalkanı (Vize Entegrasyonu):** Vize projesinin "Güvenli Dosya Yönetimi" aşamasında tespit edildiği üzere; pasaport dökümanlarının barındığı depolama alanında "Directory Listing" (Dizin Listeleme) açığı bulunmaktaydı ve saldırganlar IDOR yöntemiyle yetkisiz erişim sağlayabiliyordu. Final projesindeki `open_basedir` izolasyonu ve vizede çözüm olarak geliştirilen **Presigned-URL (Zaman Sınırlı Güvenli Link)** mimarisi birleştirilerek, hassas dosyaların doğrudan erişime kapatılması ve sunucu seviyesinde tam izolasyonu sağlanmıştır.
* **`disable_functions = exec, passthru, shell_exec, system, proc_open`**: İşletim sistemi seviyesinde doğrudan komut çalıştırabilen ve "Reverse Shell" bağlantılarına sebebiyet veren tüm tehlikeli fonksiyonları global olarak devre dışı bırakır.

## 3. İzleme, Loglama ve Sürüm Gizleme
* **`expose_php = Off`**: HTTP yanıt başlıklarında PHP sürüm bilgisini gizleyerek saldırganların keşif (Reconnaissance) aşamasını baltalar.
* **Güvenli Log Yönetimi**: Hataların ekrana basılması engellenmiş (`display_errors = Off`), tüm yetkisiz erişim denemeleri güvenli bir log dosyasına (`log_errors = On`, `error_log = /var/log/php_errors.log`) yönlendirilmiştir.

---
## 📊 Laboratuvar Kanıtları
Bu analizdeki dizin izolasyonu, tehlikeli fonksiyon bloklamaları ve vize projesindeki IDOR koruma mekanizmalarının başarı kanıtları, bu dizin altındaki ilgili ekran görüntüleriyle sunulmuştur.
