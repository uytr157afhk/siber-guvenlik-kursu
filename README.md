# siber-guvenlik-kursu

	###Linuxta Shell Scripti Servis olarak çalıştırma

1. Öncelikle bir Shell Script oluşturalım.
------> sudo nano /usr/bin/script.sh 
Sonrasında içerisine içine kod.
#!/bin/bash

# Yedek almak için fonksiyon
backup_files() {
    local source_dir="$1"
    local backup_dir="$2"
    local current_time=$(date +"%Y%m%d%H")
    local backup_filename="${current_time}_backup.tar.gz"

    tar -czf "${backup_dir}/${backup_filename}" -C "${source_dir}" .
    echo "Yedekleme tamamlandı: ${backup_dir}/${backup_filename}"
}

# Saat başı yedeklemeyi kontrol eder
while true; do
    # Şuanki dakika bilgisini al
    current_minute=$(date +"%M")

    # Eğer saat başı ise yedekleme işlemini gerçekleştir
    if [[ "$current_minute" == "00" ]]; then
        # Yedekleme işlemini gerçekleştir
        source_directory="/path/to/your/scripts"  # Scriptlerin olduğu dizin
        backup_directory="/path/to/your/backup"  # Yedeklerin kaydedileceği dizin
        backup_files "$source_directory" "$backup_directory"
    fi
done
Sonra bu kod kaydedildi.

2. Sonra koda çalışma izni verelim.
-------------> sudo chmod +x /usr/bin/script.sh 
3. Sonrasında sisteminizdeki systemd için bir servis ünite dosyası oluşturuldu. Bunun için aşağıdaki kod kullanıldı.
-------------> sudo nano /lib/systemd/system/shellscript.service 
4. Sonrasında ünit dosyasının içerisine aşağıdaki kodu yaz.
-------------> 
[Unit]
Description=My Shell Script

[Service]
ExecStart=/usr/bin/script.sh

[Install]
WantedBy=multi-user.target
Sonra dosyayı kaydet ve kapat.
5. Servisi aktif edip başlatmak için aşağıdaki kodlar yazıldı.
------------->
sudo systemctl enable shellscript.service 
sudo systemctl start shellscript.service 
6. Son olarak servisin durumuna bakalım.
------------>
sudo systemctl status shellscript.service 

Çıktı resmi komulcak.
