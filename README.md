# outfitplanner-
# 👔 OutfitPlanner - AI Destekli Akıllı Gardırop ve Kombin Asistanı

OutfitPlanner; kullanıcıların kıyafetlerini kamera çekimi veya galeriden toplu seçim yöntemiyle dijitalleştiren, fotoğrafların arka planını otomatik temizleyip kıyafet özelliklerini yapay zeka ile etiketleyen ve anlık hava durumu ile etkinlik türüne göre kişiselleştirilmiş kombinler öneren uçtan uca bir mobil platformdur.

---

## 🎯 Projenin Temel Hedefleri

1. **Zahmetsiz Dijitalleştirme:** Kullanıcıların dolabındaki kıyafetleri anlık tekli çekimle veya galeriden toplu yükleme (batch upload) ile saniyeler içinde sisteme aktarması.
2. **Otomatik Segmentasyon & AI Etiketleme:** Kıyafetlerin arka planının yerel açık kaynak kütüphanelerle temizlenmesi. Ardından renk, desen, kumaş türü, kategori ve mevsimlik uyum gibi etiketlerin Vision LLM'ler (Multimodal yapay zeka) tarafından otomatik çıkarılması.
3. **Akıllı & Bağlamsal Kombin Motoru:** Anlık hava durumu (sıcaklık, yağış) ve kullanıcının gideceği ortamı (iş, spor, gündelik, özel davet) baz alan, moda kurallarına uygun tutarlı stil önerileri sunması.
4. **Kişiselleştirme (Geri Bildirim Döngüsü):** Kullanıcının onayladığı ve reddettiği kombinlerden öğrenerek, zamanla kullanıcının zevkine daha çok hitap eden öneriler üretmesi.

---

## 🛠️ Teknoloji Yığını (Tech Stack)

### Mobil İstemci (Frontend)
- **Framework:** Flutter (Dart)
- **Kamera & Medya:** `camera` (anlık çekim), `image_picker` (çoklu galeri seçimi)
- **Durum Yönetimi (State Management):** BLoC veya Riverpod
- **Ağ İletişimi:** Dio / HTTP İstemcisi

### Backend API
- **Framework:** Python 3.11+, FastAPI (Asenkron, hızlı, AI entegrasyonuna en uygun yapı)
- **Görüntü İşleme:** `rembg` veya `birefnet` (arka plan temizleme), Pillow, OpenCV
- **AI & Multimodal Algoritma:** Vision-Language Modelleri (Örn: OpenAI GPT-4o-mini veya Google Gemini Flash)
- **Veritabanı:** PostgreSQL (SQLModel veya SQLAlchemy ORM ile ilişkisel veriler)
- **Harici Servisler:** OpenWeatherMap API (Konum bazlı anlık hava durumu verisi)

---

## 📱 Kullanıcı Akışı (User Flow)

1. **İçeri Aktarım:** Kullanıcı "Kıyafet Ekle" butonuna basar. Galeriden 5 farklı tişört/pantolon fotoğrafı seçer.
2. **İşleme (Backend):** Flutter görselleri FastAPI'ye yollar. Backend sırayla arka planları siler ve PNG olarak kaydeder.
3. **Etiketleme (Backend):** Temizlenen PNG'ler AI modeline verilir. AI her parça için Kategori, Renk, Desen, Kalıp ve Mevsim özelliklerini JSON olarak veritabanına kaydeder.
4. **Kombin İsteği:** Kullanıcı uygulamada "Bugün Ofise Gideceğim" seçeneğine tıklar. Uygulama hava durumunu çeker (Örn: 14°C, Yağmurlu).
5. **Sonuç:** AI Motoru kullanıcının dolabındaki uygun parçaları seçer ve neden bu kombini yaptığını açıklayan kısa bir metinle (Örn: "Yağmurlu havada su geçirmez botlarınla uyumlu bu koyu renkli kombini seçtim") ekrana yansıtır.

---

## 📊 Veri Modeli Taslağı

### Kıyafet Parçası (`WardrobeItem`)
```json
{
  "id": "item_101",
  "image_url": "https://storage.../clean_item_101.png",
  "category": "top", // top, bottom, outerwear, shoes, accessory
  "sub_category": "t-shirt",
  "colors": ["white", "navy-blue"],
  "pattern": "striped",
  "fit_style": "casual", // slim-fit, oversize, formal, casual, sport
  "season": ["spring", "summer"],
  "status": "active"
}
