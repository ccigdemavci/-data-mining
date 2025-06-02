# 🌊 Coastal Springs Analysis Project

Bu proje, **Data in Brief** dergisinde yayınlanan **“Kıyıya yakın ve denizaltı yaylarının küresel veri kümesi”** veri seti üzerinde sınıflandırma ve regresyon analizleri gerçekleştirmek amacıyla hazırlanmıştır. Projenin temel amacı, **spring\_count** (kaynak sayısı) değişkeninin kıyı havzalarının çevresel ve jeolojik özellikleriyle nasıl ilişkilendiğini anlamak ve farklı modelleme yaklaşımlarını kullanarak bu değişkenin tahminini veya segmentasyonunu yapmaktır.

---

## 📌 Proje Hedefleri

✅ Spring\_count değişkenini tahmin etmek için regresyon modellerini uygulamak.
✅ PCA + KMeans Clustering + Decision Tree Classifier kullanarak segmentasyon yapmak.
✅ Modellerin performanslarını detaylı değerlendirme metrikleri ile karşılaştırmak.
✅ Akademik literatürde benzer çalışmaları inceleyip sonuçları değerlendirmek.

---

## 📁 Proje Yapısı

```
📁 CWD.xlsx — Coastal Watershed Dataset (Veri seti)
📁 rf.ipynb — Regresyon modelleri ile spring_count tahmini
📁 pca.ipynb — PCA, KMeans ve Decision Tree ile segmentasyon
📁 README.md — Proje dokümantasyonu
```

---

## 📊 Kullanılan Yöntemler ve Kütüphaneler

* **Python 3.11+**

  * pandas, numpy: Veri analizi ve işlemeleri
  * scikit-learn: Regresyon, sınıflandırma ve kümeleme modelleri
  * matplotlib, seaborn: Görselleştirme

* **Regresyon Yöntemleri**

  * Decision Tree Regressor
  * Random Forest Regressor
  * Gradient Boosting Regressor
  * Extra Trees Regressor

* **Segmentasyon Yöntemi**

  * Principal Component Analysis (PCA)
  * KMeans Clustering
  * Decision Tree Classifier

---

## 🔍 Projenin Adımları

### 1️⃣ Veri Seti Hakkında

* **Veri kaynağı**: [Bouimouass et al. (2025)](https://data.mendeley.com/datasets/z33d3c5d8n/1)
* Coastal Springs Dataset (CSD) ve Coastal Watersheds Dataset (CWD)
* CWD verisinde 432 kıyı havzasına ait çevresel ve jeolojik değişkenler ile **spring\_count** bilgisi yer almakta.

---

### 2️⃣ Veri Hazırlama ve Temizleme

* **Sütun Temizliği**: Kullanılmayan veya modellemeye katkısı olmayan sütunlar çıkarıldı (ör. X centroid, Y centroid vb.).
* **Spring Count Hesaplama**: “Springs within watershed” sütunundaki virgülle ayrılmış kaynak sayıları sayılarak **spring\_count** oluşturuldu.
* **Eksik Değer İşlemleri**:

  * Eksik değerler **SimpleImputer** kullanılarak ortalama ile dolduruldu.
  * Özellikle PCA ve Decision Tree modelleri için eksik veri kalmaması sağlandı.
* **Aykırı Değerler**:

  * spring\_count > 10 olan satırlar çıkarıldı çünkü bu değerlerin çoğu model üzerinde sapmaya neden oluyordu.
* **Özellik Mühendisliği**:

  * precipitation\_slope\_ratio: Yağış / Eğim oranı
  * log\_watershed\_area: Havza alanının logaritmik dönüşümü
  * sqrt\_cropland\_area: Tarım alanlarının karekökü
  * pop\_precip\_interaction: Nüfus yoğunluğu ve yağışın çarpımı

---

### 3️⃣ Spring\_Count Tahmini — Regresyon Modelleri

* **Amaç**: spring\_count değişkenini tahmin etmek.
* **Log1p Dönüşümü**: Hedef değişkenin sağa çarpık dağılımını normalize etmek için kullanıldı.
* **Train-Test Split**: Verinin %70’i eğitim, %30’u test olarak ayrıldı.
* **Özellik Ölçeklendirme**: StandardScaler ile özellikler ölçeklendirildi.
* **Kullanılan Modeller**:

  * **Decision Tree Regressor** 
  * **Random Forest Regressor** 
  * **Gradient Boosting Regressor** 
  * **Extra Trees Regressor** 


  * MAE, MSE ve R² değerleri ile modeller karşılaştırıldı.
    Model            	MAE	     MSE	     R2
Random Forest       1.497	    5.214	   0.160
Gradient Boosting   1.513	    5.384    0.133
Extra Trees	        1.543	    5.686	   0.085
Decision Tree      	1.539	    5.691  	 0.084

  * En iyi performansı Random Forest (Yeni Özelliklerle) gösterdi (R² ≈ 0.218).

---

### 4️⃣ PCA + KMeans + Decision Tree Classifier ile Segmentasyon

* **Amaç**: spring\_count’u doğrudan tahmin etmek yerine, havzaları benzer özelliklere göre gruplandırmak ve hangi değişkenlerin segmentasyonda önemli olduğunu ortaya koymak.
* **Adımlar**:

  * SimpleImputer ile eksik değerler ortalama ile dolduruldu.
  * StandardScaler ile tüm özellikler ölçeklendirildi.
  * PCA ile boyut indirimi (PC1 ≈ %29, PC2 ≈ %16 açıklanan varyans).
  * KMeans (n\_clusters=3) ile veriyi üç kümeye ayırdık.
  * Decision Tree Classifier (max\_depth=5, min\_samples\_split=5) ile kümeleri sınıflandırdık ve öznitelik önem sıralaması çıkardık.
* **Sonuçlar**:

  * Model Accuracy: 0.80
  * En önemli özellikler: watertable\_depth\_avg, watershed\_area, cropland\_area.

---

## 📈 Sonuçlar ve Karşılaştırmalar

| Yöntem                                  | Amaç                        | Performans     | Yorum                                                    |
| --------------------------------------- | --------------------------- | -------------- | -------------------------------------------------------- |
| Regresyon Modelleri                     | spring_count tahmini        | R²: 0.218      | Tahmin başarısı sınırlı;
                                                                                           karmaşık etkileşimler bulunuyor.                         |
| PCA + KMeans + Decision Tree Classifier | Segmentasyon (clustering)   | Accuracy: 0.80 | Segmentasyon daha iyi genel ayrım sağladı,
                                                                                           öznitelikler önemli.|


---

## 📌 Projenin Genel Yorumları

* **Spring\_Count tahmini**, Random Forest modelinde bile R²≈0.218 ile sınırlı kaldı. Bu, veride önemli karmaşık etkileşimler olduğunu ve belki de ek özellik mühendisliği veya farklı algoritmalar (ör. XGBoost, Neural Networks) ile daha iyi sonuçlar alınabileceğini gösteriyor.
* **Segmentasyon yaklaşımı**, Decision Tree Classifier ile grupların belirlenmesini sağladı ve verinin iç yapısını daha iyi ortaya koydu.
* PCA, görselleştirmede boyut indirimi için etkili oldu, veri içindeki kümelerin benzerliğini görsel olarak da incelemeyi mümkün kıldı.

---

## 🔗 Kaynaklar

* **Makale**: Bouimouass, H. et al. (2025). *Kıyıya yakın ve denizaltı yaylarının küresel veri kümesi.* [Data in Brief](https://doi.org/10.1016/j.dib.2025.111319)
* **Veri Seti**: [Mendeley Datasets](https://data.mendeley.com/datasets/z33d3c5d8n/1)

---

## 📥 Kullanım

1. `CWD.xlsx` dosyasını proje klasörüne yerleştirin.
2. Jupyter Notebook veya Google Colab ortamında:

   * `Spring_Count_Regression.ipynb` veya
   * `PCA_KMeans_Classification.ipynb` dosyasını çalıştırın.
3. Gereken kütüphaneleri yükleyin:

   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn
   ```
4. Adım adım kodları çalıştırarak analizleri gözlemleyin.

---

