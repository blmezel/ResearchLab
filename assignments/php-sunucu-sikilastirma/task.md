# Araştırma ve Sıkılaştırma Görevi: PHP Sunucu Güvenliği

## 1. Giriş
Bu döküman, üretim ortamındaki PHP sunucularının siber saldırılara, uzaktan kod çalıştırma (RCE) ve yerel dosya dahil etme (LFI) zafiyetlerine karşı sıkılaştırılması amacıyla hazırlanan operasyonel rehberdir.

## 2. php.ini Sıkılaştırma Parametreleri

### A. Bilgi İfşasının Engellenmesi
* `expose_php = Off` -> HTTP yanıt başlıklarındaki PHP imzasını gizler.
* `display_errors = Off` -> Hataların tarayıcıya yansıtılmasını engeller.
* `log_errors = On` -> Hataları güvenli bir log dosyasına kaydeder.

### B. Dosya Sistemi ve Fonksiyon Kısıtlamaları
* `open_basedir = "/var/www/html/:/tmp/"` -> PHP'nin sadece belirtilen dizinlerde çalışmasına izin verir.
* `disable_functions = "exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source"` -> Tehlikeli sistem fonksiyonlarını tamamen devre dışı bırakır.

### C. Uzaktan Dosya Erişim Kısıtlamaları
* `allow_url_fopen = Off` -> Dış sunuculardan dosya çekilmesini engeller.
* `allow_url_include = Off` -> RFI zafiyetlerini engellemek için dışarıdan kod dahil edilmesini kapatır.

## 3. Web Sunucusu İle Uyum ve İzinler
* Web dizinindeki tüm dosyaların sahipliği `www-data` kullanıcısına verilmeli ancak yazma izinleri kısıtlanmalıdır.
