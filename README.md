# 🚗 Otonom Araç Simülasyonu — Yapay Zeka Proje Ödevi

> **Pygame tabanlı 2D otonom araç simülasyonu.** Araç, farklı arama algoritmalarını (BFS, UCS, A*, IDA*, Greedy, DFS) kullanarak dinamik engellerin bulunduğu şehir haritasında hedefe en uygun rotayı bulur ve otonom olarak ilerler.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Pygame](https://img.shields.io/badge/Pygame-2.x-green?logo=python&logoColor=white)
![License](https://img.shields.io/badge/Lisans-MIT-yellow)

---

## 📋 İçindekiler

- [Proje Hakkında](#-proje-hakkında)
- [Özellikler](#-özellikler)
- [Algoritmalar](#-algoritmalar)
- [Kurulum](#-kurulum)
- [Kullanım](#-kullanım)
- [Proje Yapısı](#-proje-yapısı)
- [Engel Türleri](#-engel-türleri)
- [Ekran Görüntüleri](#-ekran-görüntüleri)

---

## 🎯 Proje Hakkında

Bu proje, yapay zeka arama algoritmalarının gerçek dünya problemlerine uygulanmasını gösteren interaktif bir **otonom araç simülasyonudur**. 

Simülasyon ortamında:
- Çift şeritli yollar, döner kavşaklar ve ızgara tabanlı bir şehir haritası bulunur.
- Araç, seçilen algoritmaya göre başlangıç noktasından hedef noktaya (düğüm 136) en uygun rotayı hesaplar.
- Yolda karşılaşılan engeller (trafik ışıkları, yaya geçitleri, yol çalışmaları, kasiler, kaygan zemin) rota hesaplamasında maliyet olarak dikkate alınır.
- NPC (bilgisayar kontrollü) araçlar haritada bağımsız hareket eder ve trafik akışı oluşturur.
- Araç, sensör sistemi ile çevresindeki engelleri ve diğer araçları algılar.

---

## ✨ Özellikler

### Arama Algoritmaları
| Algoritma | Tür | Özellik |
|-----------|-----|---------|
| **BFS** | Sezgisel Olmayan | En az adımlı yolu bulur |
| **UCS** | Sezgisel Olmayan | En düşük maliyetli yolu bulur |
| **A*** | Optimal Sezgisel | Heuristik ile yönlendirilmiş optimal arama |
| **IDA*** | Optimal Sezgisel | Bellek verimli iteratif derinleşen A* |
| **Greedy** | Sezgisel | Heuristik bazlı hızlı ama optimal olmayan arama |
| **DFS** | Sezgisel Olmayan | Derinlik öncelikli arama, dal budama ile |

### Simülasyon Ortamı
- 🗺️ **Izgara tabanlı şehir haritası** — 20×12 grid yapısı, çift şeritli yollar
- 🔄 **Döner kavşaklar** — İki adet döner kavşak (roundabout)
- 🚦 **Dinamik engeller** — Trafik ışıkları, yaya geçitleri, yol çalışmaları, kasiler, kaygan zemin
- 🚙 **NPC trafik** — Bağımsız hareket eden 4 adet NPC araç
- 📡 **Sensör sistemi** — 3 yönlü (sol 15°, ön 0°, sağ 15°) mesafe sensörleri
- 🛤️ **Sağ şerit kuralı** — Ana araç sağ şerit tercihli sürüş yapar
- ⚡ **Dinamik rota güncelleme** — Yeni engel eklendiğinde rota otomatik yenilenir

### Kullanıcı Arayüzü
- 🎮 **Algoritma seçim paneli** — 6 algoritma arasında geçiş
- 📊 **Sonuç paneli** — Maliyet, adım sayısı, geçen süre, düğüm patikası
- 🔧 **Engel yönetim paneli** — Engel ekleme, silme, döndürme, uzatma/kısaltma
- 🏁 **Başlangıç noktası seçimi** — A, B, C olmak üzere 3 farklı başlangıç noktası
- ⏩ **Hız kontrolü** — Slider ile araç hızı ayarlama
- 🐞 **Debug modu** — Düğüm ve maliyet görüntüleme (D tuşu)

---

## 🧠 Algoritmalar

### Sezgisel Olmayan Algoritmalar

#### BFS (Breadth-First Search — Genişlik Öncelikli Arama)
- Kuyruk (FIFO) yapısı kullanır.
- En az adımlı yolu garanti eder.
- Aynı adım sayısında en düşük maliyetli rotayı tercih eder.
- **Dosya:** `sezgisel_olmayan.py` → `bfs_yol()`

#### UCS (Uniform Cost Search — Birörnek Maliyet Araması)
- Öncelik kuyruğu (min-heap) kullanır.
- En düşük **toplam maliyetli** yolu garanti eder.
- Engel cezaları maliyet hesabına dahil edilir.
- **Dosya:** `sezgisel_olmayan.py` → `ucs_yol()`

#### DFS (Depth-First Search — Derinlik Öncelikli Arama)
- Yığın (stack) yapısı kullanır.
- Dal bazlı erken budama (pruning) uygular.
- Engel maliyeti eşik değeri (17.0) aşan dalları keser.
- **Dosya:** `dfs.py` → `dfs_yol()`

### Sezgisel Algoritmalar

#### A* (A-Star)
- `f(n) = g(n) + h(n)` değerlendirme fonksiyonu kullanır.
  - `g(n)`: Başlangıçtan n düğümüne kadar gerçek birikimli maliyet
  - `h(n)`: n düğümünden hedefe tahmini maliyet (heuristik)
- Hem optimal hem de tam (complete) arama algoritmasıdır.
- Heuristik olarak önceden hesaplanmış tablo veya Öklid mesafesi kullanır.
- **Dosya:** `optimal_sezgisel.py` → `astar_yol()`

#### IDA* (Iterative Deepening A*)
- A*'ın bellek verimli versiyonudur.
- f-sınırı (threshold) ile DFS yapar; her iterasyonda sınırı artırır.
- Komşular heuristik değerine göre sıralanır (hedefe yakın olanı önce dener).
- Döngü kırma ve güvenlik limiti (max 200.000 genişletme) içerir.
- **Dosya:** `optimal_sezgisel.py` → `ida_star_yol()`

#### Greedy Best-First Search
- Sadece heuristik değeri `h(n)` ile yönlenir.
- Engel cezası toleransı ile filtreleme uygular.
- Hızlı ama optimal sonuç garantisi yoktur.
- **Dosya:** `greedy.py` → `greedy_yol()`

### Heuristik Fonksiyon
- **Hedef düğüm 136 için:** Önceden hesaplanmış heuristik tablo (`heuristik_136.txt`) kullanılır.
- **Genel durumda:** Öklid (kuş uçuşu) mesafe hesaplanır.

### Maliyet Sistemi
Kenarlar arasındaki maliyet, **taban yol maliyeti + engel cezası** olarak hesaplanır:

| Engel Türü | A*/IDA* Cezası | BFS/UCS Katsayısı | DFS Sabit Cezası |
|------------|---------------|-------------------|------------------|
| Trafik Işığı | `yol_kalınlığı × 0.7` | `×0.7` | `7.0` |
| Yaya Geçidi | `yol_kalınlığı × 0.5` | `×0.6` | `6.0` |
| Hız Kesici (Kasis) | `yol_kalınlığı × 0.6` | `×0.5` | `5.0` |
| Kaygan Zemin | `yol_kalınlığı × 0.4` | `×0.4` | `4.0` |
| Yol Çalışması | ∞ (kenar kapalı) | ∞ (kenar kapalı) | ∞ (kenar kapalı) |

---

## 🛠️ Kurulum

### Gereksinimler
- **Python** 3.8 veya üstü
- **Pygame** 2.x

### Adımlar

```bash
# 1. Projeyi klonlayın
git clone https://github.com/AsmaliEmirhan/OTONOM-ARAC.git
cd OTONOM-ARAC

# 2. Sanal ortam oluşturun (önerilir)
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate

# 3. Gerekli kütüphaneyi yükleyin
pip install pygame

# 4. Simülasyonu başlatın
python main.py
```

---

## 🎮 Kullanım

### Simülasyonu Başlatma
```bash
python main.py
```

### Kontroller

| Tuş / İşlem | Açıklama |
|-------------|----------|
| **Algoritma butonları** | Sol panelden BFS, UCS, A*, IDA*, Greedy veya DFS seçin |
| **A / B / C butonları** | Başlangıç noktasını değiştirin |
| **D tuşu** | Debug modunu aç/kapat (düğümleri ve maliyetleri görüntüler) |
| **Sağ panel oku (►)** | Engel yönetim panelini aç/kapat |
| **Panel butonları** | Engel ekle, sil, döndür, uzat/kısalt |
| **Fare sürükleme** | Engelleri harita üzerinde taşı |
| **Hız slider'ı** | Araç hızını ayarla |

### İş Akışı
1. Simülasyon başladığında araç haritada bekler.
2. Sol panelden bir **başlangıç noktası** (A/B/C) seçin.
3. Bir **algoritma** seçin — araç otomatik olarak rota hesaplayıp hedefe doğru hareket eder.
4. Sağ panelden **engeller** ekleyerek rotayı etkileyebilirsiniz.
5. Sonuç panelinde **maliyet, adım sayısı ve süre** bilgilerini görüntüleyin.

---

## 📁 Proje Yapısı

```
OTONOM-ARAC/
│
├── main.py                  # Ana oyun döngüsü, GUI ve simülasyon yönetimi
├── harita.py                # Harita sınıfı: ızgara, yollar, kavşaklar, graf yapısı
├── araba.py                 # Ana araç sınıfı: rota takibi, engel etkileşimi, sensörler
├── npc_araba.py             # NPC araç sınıfı: otonom trafik, çarpışma kontrolü
├── engel.py                 # Engel sınıfları: yaya geçidi, trafik ışığı, kasis, vb.
│
├── sezgisel_olmayan.py      # BFS ve UCS algoritmaları
├── optimal_sezgisel.py      # A* ve IDA* algoritmaları
├── greedy.py                # Greedy Best-First Search algoritması
├── dfs.py                   # DFS algoritması (dal budama ile)
│
├── manuel_dugumler.json     # Harita düğüm ve bağlantı verileri
├── heuristik_136.txt        # Düğüm→hedef(136) heuristik değerleri
│
├── mainaraba.png            # Ana araç görseli
├── araba1.png ~ araba4.png  # NPC araç görselleri
├── yaya2.png                # Yaya görseli
├── buzlu.png                # Kaygan zemin görseli
├── agac.png                 # Ağaç dekor görseli
├── oyunpark.png             # Oyun parkı dekor görseli
├── cardak.png               # Çardak dekor görseli
├── dekor_sol.png            # Sol dekor görseli
└── dekor_sag.png            # Sağ dekor görseli
```

---

## 🚧 Engel Türleri

### 🚶 Yaya Geçidi (`YayaGecidi`)
- Periyodik olarak aktif/pasif olur (5sn aktif, 8sn pasif).
- Aktifken yayalar geçer ve araç durur.
- Yolun dik yönünde otomatik hizalanır.

### 🚧 Yol Çalışması (`YolCalismasi`)
- Yolu **tamamen kapatır**, araç alternatif rota arar.
- Turuncu-siyah şeritli görünüm.

### ⚡ Hız Kesici / Kasis (`HizKesici`)
- Aracın hızını düşürür (`normal_hız × 0.55`).
- Sarı-siyah şeritli görünüm.

### 🚦 Trafik Işığı (`TrafikIsigi`)
- **Kırmızı → Yeşil → Sarı → Kırmızı** döngüsü.
- Kırmızıda araç durur, sarıda yavaşlar.
- Stop çizgisi kırmızıda görünür.
- Süreleri ayarlanabilir.

### 🧊 Kaygan Zemin (`KayganZemin`)
- Aracın hızını artırır (`normal_hız × 1.2`).
- Buzlu yol görseli ile temsil edilir.

---

## 🏗️ Teknik Detaylar

### Graf Yapısı
- **Harita grafı:** Grid koordinatları `(sütun, satır)` ile tanımlı çift yönlü graf
- **İkili yol grafı:** Piksel koordinatları ile tanımlı yönlü ağırlıklı graf (146 düğüm)
- Kenar maliyetleri JSON formatında önceden tanımlı

### Sensör Sistemi
- 3 yönlü mesafe sensörü: Sol 15°, Ön 0°, Sağ 15°
- Maksimum algılama mesafesi: Ön 100px, Yanlar 50px
- Renk kodlu görselleştirme: 🔴 < 40px | 🟡 < 80px | 🟢 ≥ 80px

### NPC Davranışı
- Rastgele hedef noktaya UCS ile rota hesaplar
- Sağ/sol şerit tercihi
- Trafik kurallarına uyar (kırmızı ışık, yaya geçidi)
- Çarpışma önleme mekanizması

---


