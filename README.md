# STM32 Tabanlı Kargo Eğim Takip ve Uyarı Sistemi 📦⚠️

Bu proje, Gömülü Sistemler dersi kapsamında geliştirilmiş olup; hassas ölçüm cihazları, ilaç kutuları veya kargo içerikleri gibi belirli bir açıdan fazla eğildiğinde zarar görebilecek ürünleri korumak amacıyla tasarlanmıştır.

## 📌 Proje Özeti
STM32 mikrodenetleyicisi ve MPU6050 ivmeölçer/jiroskop sensörü kullanılarak kapalı bir kutunun 3 boyutlu uzaydaki konumu gerçek zamanlı olarak izlenmektedir. Eğer kutu yatay veya dikey eksende (Pitch veya Roll) **45 dereceyi (Kritik Açı) aşacak şekilde eğilirse**, sistem anında tepki verir:
1. Donanımsal uyarı olarak **LED yanar**.
2. Haberleşme arayüzü (UART) üzerinden bilgisayara **"ALARM! Kargo Devrildi"** mesajı ve anlık açı bilgileri gönderilir.

## ⚙️ Donanım Gereksinimleri
*   **Mikrodenetleyici:** STM32 Serisi (Örn: NUCLEO-F401RE)
*   **Sensör:** MPU6050 (I2C protokolü ile haberleşir)
*   **Uyarıcılar:** 1x LED, UART (USB üzerinden Seri Port)

## 💻 Yazılım ve Algoritma
Sistem, I2C hattı üzerinden MPU6050'nin `0x3B` (ACCEL_XOUT_H) kaydından başlayarak 6 bytelık ham ivme verisini okur. Okunan değerler G kuvvetine çevrildikten sonra trigonometrik fonksiyonlar (`atan2`) yardımıyla Pitch ve Roll açılarına dönüştürülür. Ana kodlar `Core/Src/main.c` dosyasında yer almaktadır.

## 🚀 Canlı Simülasyon (Wokwi)
Projenin sensör okuma, matematiksel açı hesaplama ve alarm tetikleme algoritmasını anında tarayıcınızda test edebilirsiniz. (Simülasyon prototipi Arduino platformuna uyarlanmıştır).
Projenin hedef donanımı ve asıl yazılımı STM32 (C/HAL Kütüphaneleri) altyapısına sahiptir. Ancak Wokwi web simülatörü STM32 HAL yapısını tam olarak desteklemediğinden, görsel prototipleme için Arduino tercih edilmiştir.

Buradaki amaç donanımdan ziyade, platform bağımsız algoritmanın (MPU6050'den I2C verisi okuma, G kuvveti dönüşümü ve Pitch/Roll açı hesaplamaları) sorunsuz çalıştığını kanıtlamaktır (Proof of Concept). Sistemin endüstriyel kullanıma hazır, gerçek STM32 kodları ise projedeki main.c dosyasında yer almaktadır.
🔗 **Simülasyonu Çalıştır:** [(https://wokwi.com/projects/463580996852741121)]

*Not: Simülasyon açıldığında 'Play' tuşuna basıp, mavi sensörün üzerine tıklayarak eğim açılarını değiştirebilirsiniz.*
