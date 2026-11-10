# AIRLAB Alım Görevi 2026 - Bölüm II: Agentic AI

Bu depo, SKY LAB - AIRLAB yapay zeka ekibi alım görevi kapsamında hazırlanan "Bölüm II: Uzmanlık Alanı (Agentic AI)" uygulamalı mini projesini içermektedir.

## Proje Özeti
Bu çalışmada, dış araçları (tool) kullanan bir yapay zeka ajanının (LLM Agent) aynı görevi çözerken mükerrer araç çağrıları yaparak kaynak (token/zaman) israfı yaratması problemi ele alınmıştır. 

Sisteme basit bir **Önbellek (Cache Interceptor)** mekanizması entegre edilerek, ajanın daha önce elde ettiği verilere tekrar ihtiyaç duyduğunda araçları baştan çalıştırmak yerine hafızadaki sonucu kullanması sağlanmıştır.

## Tekrar Üretilebilirlik (Reproducibility)
Projedeki deneylerin sonuçları tamamen deterministiktir ve test setindeki rastgelelikler sabitlenmiştir. Kodun sizin tarafınızdan birebir aynı sonuçları üretebilmesi için aşağıdaki parametreler kullanılmıştır:
* **Seed:** `42` (Random modülü için sabitlenmiştir)
* **Kütüphaneler:** Gerekli bağımlılıklar `requirements.txt` dosyasında versiyonlarıyla birlikte belirtilmiştir. `pip install -r requirements.txt` komutuyla kurabilirsiniz.

## Kullanılan Sahte (Mock) Araçlar
Gerçek servislerin (API) kota aşımı, ağ kesintisi veya değişen veriler gibi deneyi bozacak dış etkenlerini izole etmek amacıyla 3 adet deterministik mock araç tasarlanmıştır:
1. `mock_web_search`: Güncel bilgi sorgulama aracı.
2. `mock_calculator`: Regex filtreli güvenli matematiksel hesaplama aracı.
3. `mock_database`: JSON formatında müşteri/ürün verisi getiren yapılandırılmış veri aracı.

## Deney Bulguları (Baseline vs. Cache Müdahalesi)
10 görevden (3'ü bilerek tekrar tetikleyicili) oluşan test seti, her bir konfigürasyon için 3'er kez çalıştırılarak ortalama sonuçlar elde edilmiştir.

* **Toplam Araç Çağrısı:** 19.0'dan -> 16.0'a düşürüldü.
* **Gereksiz Çağrı (Redundant Calls):** 3.0'dan -> 0.0'a indirildi.
* **Görev Başarı Oranı:** Müdahale öncesinde ve sonrasında %100.0 olarak korundu.
* **Token Tasarrufu:** Yaklaşık %20 oranında token / hesaplama israfı engellendi.

Deneye ait detaylı görselleştirmeler, grafikler ve kod blokları `Bölum_II_Agentic_AI.ipynb` dosyası içerisinde yer almaktadır.