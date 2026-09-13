# 🎯 Gömülü Sistem Üzerinde Yapay Zekâ Destekli Dost-Düşman Hedef Tespit ve Otonom Takip Sistemi

Bu proje, **Raspberry Pi 5** gömülü platformu üzerinde çalışan; hava sahasındaki tehditleri (drone/İHA) dost unsurlardan (kuş) ayırt eden, NCNN ile optimize edilmiş YOLOv8 mimarisi ve 2 eksenli elektromekanik Pan-Tilt yönlendirmesini bir araya getiren uçtan uca kapalı çevrim bir otonom hava savunma/takip prototipidir.

---

## 📌 Temel Özellikler ve Yenilikçi Yaklaşımlar

* **Uçta Yapay Zekâ (Edge AI):** YOLOv8n modeli, ARM NEON SIMD komut setlerini doğrudan kullanan **NCNN** formatına dönüştürülerek harici GPU olmadan Raspberry Pi 5 üzerinde optimize edilmiştir[cite: 7].
* **Gelişmiş Takip Hattı (Vision-Only ROI):** Uzak mesafe ve küçük hedef tespitinde yaşanan süreksizlikleri engellemek için Kalman filtresi ve Optik Akışın sınırlılıkları aşılmış; dinamik **Bölge Odaklı Arama (ROI - Optik Yakınlaştırma)** mimarisi geliştirilmiştir[cite: 7].
* **Zamansal Kararlılık (Temporal Majority Voting):** Ardışık video kareleri ($N=7$) üzerinden zamansal oy çoğunluğu uygulanarak tekil karelik sınıflandırma dalgalanmaları (kuş/drone geçişleri) tamamen filtrelenmiştir[cite: 7].
* **Çoklu Hedef Takibi (MOT) & Önceliklendirme:** Görüş alanındaki tüm hedeflere benzersiz ID atanır; ekran merkezine olan Öklid uzaklığına göre en kritik tehdit otomatik seçilerek kilitlenilir (`LCK`)[cite: 7].
* **Taktiksel HUD Arayüzü:** OpenCV ile askeri standartlarda yeşil monokrom nişangah, telemetri verileri ve durum bilgileri canlı video üzerine işlenir[cite: 7].
* **Kapalı Çevrim Elektromekanik Yönlendirme:** Hesaplanan piksel sapma hataları (Error X/Y), **I2C protokolü** üzerinden **PCA9685 (12-bit PWM)** sürücüsüne aktarılır ve Pan-Tilt mekanizmasıyla kamera sürekli hedefe yönlendirilir (Ölü Bölge / Dead Zone filtresi ile motor titremesi engellenmiştir)[cite: 7].

---

## 🛠️ Sistem Mimarisi & Donanım Bileşenleri

| Bileşen | Görevi / Rolü |
| :--- | :--- |
| **Raspberry Pi 5 (4/8 GB)** | Merkezi işlem birimi (Edge AI çıkarımı, takip ve kontrol algoritmaları)[cite: 7] |
| **Raspberry Pi Camera 3 Wide** | Sony IMX708 (120° FOV, HDR, PDAF) ile canlı hava sahası taraması[cite: 7] |
| **PCA9685 Sürücü Kartı** | I2C üzerinden 16-kanal 12-bit bağımsız PWM motor kontrolü[cite: 7] |
| **2 Eksenli Pan-Tilt Düzeneği** | Kamerayı hedefe yönlendiren yatay (Pan) ve dikey (Tilt) servo sistemi[cite: 7] |
| **Yazılım & Çerçeveler** | Python, NCNN, YOLOv8, OpenCV, Roboflow[cite: 7] |

---

## 📊 Deneysel Sonuçlar ve Saha Performansı

* **Veri Seti:** Roboflow üzerinde 10.752 görüntüye genişletilmiş, zorlu gökyüzü arka planları ve negatif örneklerle dengelenmiş veri seti[cite: 7].
* **Model Doğruluğu:**
  * **Genel mAP50:** `%97.9`[cite: 7]
  * **Genel mAP50-95:** `%67.4`[cite: 7]
  * **Drone Sınıfı Precision / Recall:** `%96.9` / `%96.2`[cite: 7]
* **Saha Testi Çıkarım Hızı:** Gerçek dünya dış ortam testlerinde derin öğrenme, ROI takibi, HUD çizimi ve kapalı çevrim servo kontrolü eş zamanlı çalışırken **8.0 – 10.5 FPS** stabil operasyonel hız elde edilmiştir[cite: 7].


