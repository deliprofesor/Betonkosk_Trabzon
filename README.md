# Smart Monitoring of Beton Köşk Systems: Time-Series & Anomaly Analysis (Mart 2025)

---

## ⚠️ Önemli Not (Etik ve Veri Kaynağı)

Bu proje kapsamında kullanılan veri **gerçek bir firmaya ait değildir**.  
Veri, **etik kurallar ve gizlilik gereklilikleri** nedeniyle **yapay olarak üretilmiştir** ve herhangi bir ticari veya kişisel bilgi içermemektedir.  
Projede kullanılan veri ve analiz yöntemleri tamamen eğitim ve portföy amaçlıdır.

##  Proje Hedefi

Bu proje, Trabzon’daki beton köşk sistemlerinin sensör verilerini analiz ederek:

1. **Zaman serisi analizi** ile trend ve mevsimsellikleri incelemek,  
2. **Anomali tespiti** ile olağandışı durumları belirlemek (IsolationForest ve RandomForest),  
3. **Köşk ve ilçe bazlı karşılaştırmalı analiz** ile bölgeler arası farklılıkları görselleştirmek amacını taşır.

---

##  Kullanılan Veri

- `trabzon_betonkosk_anomali_verisi_mart2025.csv`  
  İçerik: Sicaklik, nem, akım, gerilim, topraklama direnci gibi sensör ölçümleri  
  Sütun örnekleri: `kosk_id`, `tarih`, `sicaklik_C`, `nem_orani`, `akim_A`, `trafo_gerilimi_kV`, `topraklama_direnci`, `ariza_var`

---

##  Analiz Adımları

###  Veri Hazırlığı
- Tarih sütunu `datetime` tipine çevrildi ve köşk bazında sıralandı.
- Eksik ve bozuk veriler kontrol edildi.
- Feature engineering uygulandı:  
  - `sicaklik_nem = sicaklik_C * nem_orani`  
  - `akim_gerilim_orani = akim_A / trafo_gerilimi_kV`  
  - `direnc_flag = topraklama_direnci > 2.5` (binary)  
- Gün, saat, hafta günü sütunları çıkarıldı.

###  Zaman Serisi Analizi
- En çok kayıtlı 3 köşk seçildi.
- **STL decomposition** ile trend, sezon ve residual analiz edildi.
- **Prophet** ile 30 günlük ileriye dönük tahmin yapıldı.
- Çıktılar: Trend grafikleri, tahmin grafikleri, mevsimsellik analizi.

###  Anomali Tespiti
#### A) Denetimsiz (Unsupervised)
- Model: IsolationForest (%2 anomali oranı)  
- Özellikler: `akim_A`, `sicaklik_C`, `nem_orani`, `topraklama_direnci`, `akim_gerilim_orani`, `sicaklik_nem`  
- Çıktılar: `top_isolationforest_anomalies.csv`, skor ve bayrak sütunları.

#### B) Denetimli (Supervised)
- Model: RandomForestClassifier  
- Hedef: `ariza_var`  
- Değerlendirme: Precision, Recall, F1, ROC-AUC  
- Feature importance ve confusion matrix raporları.

###  Köşk ve İlçe Bazlı Karşılaştırma
- İlçe bazında ortalama değer tablosu ve arıza oranı  
- Basit **KMeans clustering** ile benzer köşk grupları  
- Heatmap ve scatter görselleştirmeleri

---

##  Çıktılar
- Temiz veri örneği: `df_head10.csv`  
- IsolationForest anomalileri: `top_isolationforest_anomalies.csv`  
- RandomForest feature importance: `randomforest_feature_importance.csv`  
- STL ve Prophet görselleri: `stl_kosk_X.png`, `prophet_forecast_kosk_X.png`  
- İlçe özet ve cluster: `ilce_aggregation_clusters.csv`  
- Model dosyaları: `isolation_forest_model.pkl`, `scaler.pkl`, `random_forest_ariza_var.pkl`

---

##  Kullanılan Teknolojiler
- Python 3.x  
- Kütüphaneler: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `prophet`, `statsmodels`, `pickle`  
- Analiz araçları: STL, Prophet, IsolationForest, RandomForest, KMeans

---

##  Sonuç ve Çıkarımlar
- Sensör değerlerindeki anomaliler tespit edildi ve kosk_id bazında izlendi.  
- Trend ve mevsimsellik gözlemlendi; belirli saatlerde anomali yoğunluğu saptandı.  
- İlçe bazlı karşılaştırma ile riskli bölgeler belirlendi.  
- Modelleme ile önleyici bakım ve erken uyarı sistemleri önerilebilir.

---

##  Geliştirme Fikirleri
- Köşk konumlarını ekleyerek coğrafi harita analizi (folium, geopandas)  
- MQTT veya IoT platformlarından gerçek zamanlı veri çekmek  
- Power BI dashboard ile otomatik güncellenen raporlar

---

##  Lisans
Bu proje MIT Lisansı altında lisanslanmıştır.  


