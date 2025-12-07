# librtsplink

IP kameralardan RTSP ile düşük gecikmeli video akışı almak için yazdığım C++ kütüphanesi.

## Ne İşe Yarıyor?

Bu kütüphane ile IP kameralarınıza RTSP protokolü üzerinden bağlanabilir ve gerçek zamanlı görüntü alabilirsiniz. Arka planda ayrı bir thread çalışır ve sürekli kamera bağlantısını kontrol eder - bağlantı koparsa otomatik yeniden bağlanmaya çalışır.

GStreamer ve FFmpeg backend desteği var, hangisini tercih ederseniz onu kullanabilirsiniz.

## Gereksinimler

- C++11 destekleyen bir derleyici (GCC 5+, Clang 3.5+)
- OpenCV 3.x veya 4.x
- GStreamer 1.0 veya FFmpeg (en az biri gerekli)

## Kurulum

Önce bağımlılıkları kurun:

```bash
sudo apt install libopencv-dev libgstreamer1.0-dev libavformat-dev libswscale-dev
```

Sonra derleyin ve kurun:

```bash
git clone https://github.com/uzunenes/librtsplink.git
cd librtsplink
make
sudo make install
```

## Kullanım

Temel kullanım oldukça basit:

```cpp
#include <librtsplink/librtsplink.h>

// Kameraya bağlan (arka planda thread başlatır)
rtsplink_connect_and_follow_ip_camera_connection_background(
    "192.168.1.100",    // IP adresi
    "admin",            // kullanıcı adı
    "password",         // şifre
    554,                // port
    "/stream1",         // mount point (opsiyonel, nullptr olabilir)
    e_rtsplink_rtsp_backend_lib_GSTREAMER  // veya FFMPEG
);

// Görüntü al
double read_time;
int err_code;
cv::Mat* frame = rtsplink_get_image_from_ip_camera(&read_time, 5000, &err_code);

if (frame != nullptr && err_code == 0) {
    // frame ile istediğinizi yapın
    delete frame;  // kullandıktan sonra silmeyi unutmayın
}

// İşiniz bitince bağlantıyı kapatın
rtsplink_release_and_close_ip_camera();
```

Daha detaylı örnek için `examples/main.cpp` dosyasına bakabilirsiniz.

## Örneği Çalıştırma

```bash
cd examples
# main.cpp içindeki IP, kullanıcı adı ve şifreyi düzenleyin
make
./camera_example
```

## API Fonksiyonları

| Fonksiyon | Açıklama |
|-----------|----------|
| `rtsplink_connect_and_follow_ip_camera_connection_background()` | Kameraya bağlanır, arka planda bağlantıyı takip eder |
| `rtsplink_get_image_from_ip_camera()` | Kuyruktan bir frame alır |
| `rtsplink_get_ip_camera_fps()` | Kameranın FPS değerini döndürür |
| `rtsplink_get_ip_camera_connection_status()` | Bağlantı durumunu sorgular |
| `rtsplink_release_and_close_ip_camera()` | Bağlantıyı kapatır ve kaynakları temizler |

## Katkıda Bulunanlar

Bu projenin geliştirilmesinde emeği geçen ekip arkadaşlarıma teşekkürler:

- Ahmet Selim Demirel
- Doğan Mehmet Başoğlu  
- Elif Cansu Ada
- Mevlüt Ardıç
- Serhat Karaca

## Lisans

Bu proje açık kaynaklıdır. Detaylar için `License` dosyasına bakın.
