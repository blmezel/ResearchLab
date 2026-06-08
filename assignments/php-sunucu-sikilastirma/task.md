# Yapılandırma Görevi: PHP Sunucu Sıkılaştırma (PHP Hardening)

## 1. Giriş
PHP tabanlı web mimarileri, varsayılan kurulum ayarlarında siber saldırganların hedefi olmaya oldukça müsaittir. Bu görev kapsamında, üretim (production) ortamındaki bir PHP sunucusunun siber güvenlik standartlarına (OWASP, CIS) uygun olarak nasıl sıkılaştırılacağı teknik parametrelerle analiz edilmiştir.

## 2. php.ini Güvenlik Konfigürasyonları
Sunucu güvenliğini üst seviyeye çıkarmak adına `php.ini` dosyası üzerinde şu kritik direktifler uygulanmıştır:

* **`expose_php = Off`**: HTTP yanıt başlıklarında (Headers) yer alan PHP sürüm bilgisini gizleyerek saldırganların "Reconnaissance" (Keşif) aşamasında hedef odaklı zafiyet taraması yapmasını engeller.
* **`disable_functions = exec, passthru, shell_exec, system, proc_open`**: İşletim sistemi seviyesinde doğrudan komut çalıştırabilen ve "Reverse Shell" bağlantılarına sebebiyet veren tüm tehlikeli fonksiyonları global olarak devre dışı bırakır.
* **`open_basedir = "/home/cybella/public_files/"`**: PHP betiklerinin erişebileceği dizin sınırını katı bir şekilde çizer. Bu sayede olası bir kod enjeksiyonu durumunda, saldırganın `../../etc/passwd` gibi kritik sistem dosyalarına erişmesini (Path Traversal zafiyetini) donanımsal/yazılımsal olarak engeller.

## 3. İzleme ve Log Yönetimi
Hataların ekrana basılması engellenmiş (`display_errors = Off`), bunun yerine tüm anomali ve yetkisiz erişim denemeleri güvenli bir log dosyasına (`log_errors = On`, `error_log = /var/log/php_errors.log`) yönlendirilmiştir.

---
## 📊 Laboratuvar Kanıtları ve Ekran Görüntüleri
Bu analizdeki defansif kodlama ve anomali tespit motorunun laboratuvar ortamındaki başarı kanıtları aşağıda listelenmiştir:

*(Görseller GitHub uzak deposu üzerinden güvenli ve güncel versiyonısıyla senkronize edilmiştir).*
