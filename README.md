# ตั้งค่า NGINX เพื่อให้รองรับ OWASP เบื้องต้น
```bash
server {
    listen 80;

    # ส่ง header X-XSS-Protection เพื่อป้องกันการโจมตี XSS
    add_header X-XSS-Protection "1; mode=block";

    # ป้องกันการแสดงไฟล์ HTML ใน iframe
    add_header X-Frame-Options "SAMEORIGIN";

    # ป้องกันการโหลดไฟล์จาก external domain ที่ไม่น่าเชื่อถือ
    add_header X-Content-Type-Options "nosniff";
    
    # นโยบายเนื้อหาเพื่อป้องกันการโจมตีหลายรูปแบบ
    add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; object-src 'none'; frame-ancestors 'self';";

    # เสิร์ฟไฟล์ที่มีการเข้ารหัส HTTP Strict Transport Security (HSTS)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
}
```

## คำอธิบายของ header ที่ถูกตั้งค่า:
1. `X-XSS-Protection`: ป้องกันเบราว์เซอร์จากการทำงานของสคริปต์ที่ไม่ปลอดภัย
2. `X-Frame-Options`: ป้องกันการโจมตีแบบ Clickjacking โดยไม่อนุญาตให้ iframe แสดงเนื้อหาของเว็บไซต์
3. `X-Content-Type-Options`: ป้องกันไม่ให้เบราว์เซอร์เดา MIME types ของไฟล์
4. `Content-Security-Policy (CSP)`: นโยบายการรักษาความปลอดภัยที่จำกัดแหล่งที่สามารถโหลดทรัพยากรได้ (เช่น CSS, JavaScript)
5. `Strict-Transport-Security (HSTS)`: บังคับใช้ HTTPS ตลอดเวลา
