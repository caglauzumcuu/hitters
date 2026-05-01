# Makine Öğrenmesi ile Beyzbol Oyuncusu Maaş Tahmini

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![sklearn](https://img.shields.io/badge/sklearn-1.3%2B-orange)
![Optuna](https://img.shields.io/badge/Optuna-3.0%2B-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## Proje Hakkında

1986-1987 sezonuna ait kariyer istatistikleri kullanılarak beyzbol oyuncularının
maaşlarını tahmin eden uçtan uca bir makine öğrenmesi projesi.

- **Veri Kaynağı:** Carnegie Mellon Üniversitesi — StatLib
- **Gözlem:** 322 oyuncu → 263 (temizleme sonrası)
- **Özellik:** 20 orijinal → 39 (feature engineering sonrası)
- **Hedef Değişken:** Salary (bin $ cinsinden)
- **En İyi Model:** Random Forest — RMSE: 233.19

---

## Proje Adımları

### 1. Keşifçi Veri Analizi (EDA)
- Veri seti genel özeti
- Kategorik & sayısal değişken analizi
- Hedef değişken (Salary) dağılımı
- Korelasyon analizi

### 2. Veri Ön İşleme
- **Aykırı Değer:** IQR yöntemiyle baskılama
- **Eksik Değer:** Salary'si boş 59 satır silindi

### 3. Özellik Mühendisliği
20 orijinal değişkenden 20 yeni özellik türetildi:

| Kategori | Örnek | Mantık |
|----------|-------|--------|
| Sezon/Kariyer Oranı | NEW_RBI_Ratio | Bu sezon kariyerine göre ne kadar iyi? |
| Yıllık Ortalama | NEW_CRuns_per_Year | Yıllık üretkenlik |
| Kariyer Oranı | NEW_Career_Hit_Rate | Genel kariyer verimliliği |
| Etkileşim | NEW_RBI_x_Walks | İki değişkenin birlikte etkisi |
| Fark | NEW_Diff_Hits | Bu sezon vs kariyer ortalaması |

### 4. Encoding & Ölçeklendirme
- **One-Hot Encoding** (drop_first=True): League, Division, NewLeague
- **StandardScaler:** Tüm sayısal değişkenler (Salary hariç)

### 5. Model Karşılaştırması
12 farklı algoritma varsayılan ayarlarla karşılaştırıldı:

| Model | Base RMSE |
|-------|-----------|
| GBM | 240.34 |
| Random Forest | 240.76 |
| CatBoost | 243.66 |
| LightGBM | 264.36 |
| Ridge | 267.18 |

### 6. Hiperparametre Optimizasyonu (Optuna)
GridSearch yerine Optuna kullanıldı:

| Model | Base RMSE | Optuna RMSE | İyileşme |
|-------|-----------|-------------|----------|
| **Random Forest** | 240.76 | **233.19** | +7.57 |
| GBM | 240.34 | 235.14 | +5.20 |
| CatBoost | 243.66 | 238.55 | +5.11 |
| LightGBM | 264.36 | 255.79 | +8.57 |

### 7. Feature Importance
- 4 modelin konsensüs feature importance'ı hesaplandı
- Top 20'de **12/20 türetilen özellik** — feature engineering işe yaradı!
- En kritik özellikler: CHits, NEW_RBI_x_Walks, CRBI

---

## Sonuçlar

| Metrik | Değer |
|--------|-------|
| En İyi Model | Random Forest |
| RMSE | 233.19 bin $ |
| Ortalama Maaş | 535.9 bin $ |
| Veri Seti | 263 gözlem, 39 özellik |

---

## Kurulum

```bash
git clone https://github.com/kullanici_adin/baseball-salary-prediction.git
cd Hitters
pip install -r requirements.txt
jupyter notebook Hitters.ipynb
```

---

## Gereksinimler
```
pandas>=2.0
numpy>=1.24
scikit-learn>=1.3
xgboost>=1.7
lightgbm>=4.0
catboost>=1.2
optuna>=3.0
matplotlib>=3.7
seaborn>=0.12
joblib>=1.3
```
---

## Lisans
MIT License
