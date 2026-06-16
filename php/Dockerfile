FROM debian:wheezy

# เปลี่ยนไปใช้ Archive Repository เพราะ Debian Wheezy หมดอายุไปแล้ว
RUN sed -i 's/httpredir.debian.org/archive.debian.org/g' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/archive.debian.org\/debian-security/g' /etc/apt/sources.list && \
    sed -i '/wheezy-updates/d' /etc/apt/sources.list

# ติดตั้ง PHP 5.3 และส่วนเสริมที่จำเป็น
RUN apt-get update && apt-get install -y \
    php5-fpm \
    php5-curl \
    php5-cli \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ตั้งค่าคอนฟิกให้ PHP-FPM ฟังพอร์ต 9000 (เปิดรับการเชื่อมต่อจาก Nginx Container)
RUN sed -i 's|listen = /var/run/php5-fpm.sock|listen = 0.0.0.0:9000|g' /etc/php5/fpm/pool.d/www.conf

# เปิดพอร์ต 9000 สำหรับ Container
EXPOSE 9000

# สั่งให้รัน PHP-FPM แบบไม่ไปทำงานหลังบ้าน (Force Foreground) เพื่อให้ Docker คุมเซสชันได้
CMD ["php5-fpm", "-F"]