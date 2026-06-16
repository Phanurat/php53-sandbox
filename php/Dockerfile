FROM debian:wheezy

# ล้างโครงสร้างเก่าออกให้หมด แล้วชี้ตรงไปที่ Archive Server ที่ถูกต้อง
RUN echo "deb http://archive.debian.org/debian/ wheezy main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian/ wheezy main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://archive.debian.org/debian-security/ wheezy/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian-security/ wheezy/updates main contrib non-free" >> /etc/apt/sources.list

# ปิดระบบตรวจสอบเวลา (Valid-Until) ของแพ็กเกจ เนื่องจากคลังเก็บนี้ไม่มีการอัปเดตแล้ว
RUN echo "Acquire::Check-Valid-Until \"false\";" > /etc/apt/apt.conf.d/99no-check-valid-until

# เริ่มต้นอัปเดต และติดตั้งโดยใช้ --force-yes เพื่อข้าม Error เรื่องคีย์หมดอายุ
RUN apt-get update && apt-get install -y --force-yes \
    php5-fpm \
    php5-curl \
    php5-cli \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ตั้งค่าคอนฟิกให้ PHP-FPM ฟังพอร์ต 9000 เพื่อรอรับข้อมูลจาก Nginx
RUN sed -i 's|listen = /var/run/php5-fpm.sock|listen = 0.0.0.0:9000|g' /etc/php5/fpm/pool.d/www.conf

EXPOSE 9000

CMD ["php5-fpm", "-F"]