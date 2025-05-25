#  Sosyal Medya Trend Analizi – NLP Projesi

Bu proje, Twitter veri seti kullanılarak metin madenciliği (text mining) ve doğal dil işleme (NLP) teknikleriyle sosyal medya trendlerinin analizini amaçlamaktadır. Özellikle günlük atılan tweet’ler içerisinde geçen metinler ön işleme tekniklerinden geçirilmiş, ardından TF-IDF ve Word2Vec yöntemleriyle vektörleştirilmiştir. Böylece, benzer içeriklerin gruplanması ve olası “trend başlıklarının” belirlenmesi hedeflenmiştir.

**Proje dosyaları boyut olarak büyük olduğundan GitHub’a yüklenemediği için Google Drive bağlantısı üzerinden erişime açılmıştır:**
https://drive.google.com/file/d/11hg224EK3BNok19t1M236c86rolmO6Fs/view?usp=sharing

##  Proje Amacı

Proje kapsamında aşağıdaki adımlar gerçekleştirilmiştir:

- Twitter veri setinin temizlenmesi ve ön işleme tekniklerinin uygulanması
- Zipf yasası çerçevesinde ham ve işlenmiş verilerin kelime dağılımlarının analizi
- Lemmatization ve stemming yöntemlerinin karşılaştırmalı analizi
- TF-IDF ve Word2Vec gibi vektörleştirme tekniklerinin uygulanması
- Elde edilen vektörlerin içerik benzerliğini ölçmede kullanılması

##  Veri Seti Bilgileri

- **Veri kaynağı**: [Twitter 2022 - Kaggle](https://www.kaggle.com/datasets/amirhosseinnaghshzan/twitter-2022)
- **İndirilen veri formatı**: `.csv`
- **Ham veri boyutu**: 66.89 MB
- **Toplam tweet (döküman) sayısı**: 547,496
- **Kullanılan sütunlar**: `date`, `content`
- **Veri aralığı**: 01 Ocak 2022 – 31 Aralık 2022
- **Günlük ortalama tweet sayısı**: ~1500

Ham veri içeriği örnek:

| date                         | content                                                |
|-----------------------------|--------------------------------------------------------|
| 2022-01-01 23:59:59+00:00   | I’m getting a sugar daddy this year!                  |
| 2022-01-01 23:59:59+00:00   | might make em french toast after and spoon a little.  |
| 2022-01-01 23:59:59+00:00   | Our platform can be matched to your company’s brand.  |

Veri analizinde yalnızca metin içeriğini ve tarih bilgisini içeren bu iki sütun kullanılmaktadır.

##  Uygulanan Pre-processing Adımları

Tüm tweet metinlerine aşağıdaki sıralı işlemler uygulanmıştır:

1. **Lowercasing**: Tüm kelimeler küçük harfe çevrildi.
2. **HTML/URL/Emoji temizliği**: Emoji, link ve mention'lar regex ile silindi.
3. **Tokenization**: Kelime bazlı parçalara ayrıldı (word_tokenize).
4. **Stopword Removal**: NLTK İngilizce stopword listesiyle filtreleme yapıldı.
5. **Punctuation/Sayı Temizliği**: Noktalama işaretleri ve sayılar kaldırıldı.
6. **Lemmatization**: WordNetLemmatizer kullanılarak kök kelimeler elde edildi.
7. **Stemming**: PorterStemmer ile kelimeler kök forma indirildi.

 ## Projede Kullanılan/Kaydedilen Tüm Dosyalar
**1. dataset_temiz.csv**

Konum: C:\Users\hukus\Downloads\datasett\dataset_temiz.csv

Açıklama: Orijinal ham veriden yalnızca date ve content sütunları bırakılarak sadeleştirilmiş versiyon.

İçerik: Ham tweet metinleri ve tarih bilgisi.

Format: .csv

**2. content_clean_versiyon.csv**

Konum: C:\Users\hukus\Downloads\datasett\content_clean_versiyon.csv

Açıklama: Tweet'lerden özel karakter, URL, mention, stopword ve kısa kelimeler temizlenmiş 
hali. content_clean sütunu eklenmiştir.

Format: .csv

**3. lemma_dataset.csv**

Konum: C:\Users\hukus\Downloads\datasett\lemma_dataset.csv

Açıklama: content_clean sütununa lemmatization işlemi uygulanarak content_lemma sütunu eklenmiş versiyondur.

Format: .csv

**4. lemma_stem_dataset.csv**

Konum: C:\Users\hukus\Downloads\datasett\lemma_stem_dataset.csv

Açıklama: Lemmatization ve stemming işlemlerinin her ikisi de uygulanmış ve sonuçlar sırasıyla content_lemma ve content_stem sütunlarında saklanmıştır. Bu veri, tüm model eğitimlerinde temel olarak kullanılmıştır.

Format: .csv

 **5. models/ klasörü**
 
Konum: C:\Users\hukus\Downloads\datasett\models\

Açıklama: Gensim Word2Vec kütüphanesi ile eğitilen toplam 16 model bu klasörde saklanır.

Alt Dosyalar:

lemmatized_model_cbow_window2_dim100.model

stemmed_model_skipgram_window4_dim100.model

... ve diğer 14 model

Format: .model

**6. tf_idf_models/ klasörü**

Konum: Proje ana klasörü içerisinde

Açıklama: TF-IDF işlemi sırasında oluşturulan vektörizer ve sparse matrisler bu klasörde saklanır.

Alt Dosyalar:

lemma_tf_idf_vectorizer.pkl

stem_tf_idf_vectorizer.pkl

lemma_tf_idf_matrix.pkl

stem_tf_idf_matrix.pkl

Format: .pkl (pickle)


##  Zipf Yasası Analizi

Ham veri ile lemmatize ve stem edilmiş verilerin Zipf yasasına göre dağılımları analiz edilmiştir.

### Ham Veri:
- Toplam kelime: 8,471,651  
- Farklı kelime: 594,344  
- En sık geçen kelimeler: `the`, `to`, `i`, `a`, `and`...

### Lemmatized:
- Toplam kelime: 4,273,435  
- Farklı kelime: 179,385  
- En sık geçen kelimeler: `like`, `get`, `one`, `love`, `day`...

### Stemmed:
- Toplam kelime: 4,273,435  
- Farklı kelime: 153,032  
- En sık geçen kökler: `like`, `get`, `time`, `daddi`, `make`...

Sonuç olarak veri boyutu ön işleme sonrası %50'den fazla azaltılmış, kelime çeşitliliği (vocabulary) ise büyük ölçüde sadeleştirilmiştir.

##  TF-IDF Vektörleştirme

Hem `content_lemma` hem de `content_stem` sütunları kullanılarak TF-IDF matrisleri oluşturulmuştur. Aşırı boyut nedeniyle **sparse matrix** (seyrek matris) formatında saklanmış ve `.pkl` dosyalarına kaydedilmiştir.

### TF-IDF Örnek Çıktı (Lemmatized):
İlk tweet'te en yüksek skorlu kelimeler:
sugar: 0.6150
daddy: 0.5845
getting: 0.4014
year: 0.3451

bash
 
**Cosine similarity ile 'year' kelimesine en yakın terimler:**
year: 1.0000
ago: 0.1816
old: 0.1375
new: 0.1234
happy: 0.0975 

### TF-IDF Çıktı Dosyaları:
- `tf_idf_models/lemma_tf_idf_vectorizer.pkl`
- `tf_idf_models/stem_tf_idf_vectorizer.pkl`
- `tf_idf_models/lemma_tf_idf_matrix.pkl`
- `tf_idf_models/stem_tf_idf_matrix.pkl`

## Word2Vec Modelleme

Her veri seti için aşağıdaki 8 parametre kombinasyonuyla toplam 16 model eğitilmiştir.

### Örnek Analiz (Kelime: `year`)
- Lemmatized CBOW (100 dim):
  - En yakın: `decade`, `month`, `yr`
- Stemmed Skipgram (100 dim):
  - En yakın: `month`, `decad`, `yr`

### Çıktı Yolu:
Tüm model dosyaları: `models/` klasöründe `.model` formatında yer almaktadır.

## Benzerlik Analizi ve Değerlendirme Süreci

Bu çalışmanın devamında, vektörleştirilmiş metinler üzerinde benzerlik analizi gerçekleştirilmiştir. Bu süreçte temel amaç, farklı vektörleme modellerinin içerik benzerliğini ne kadar isabetli yansıttığını analiz etmektir.

### Kullanılan Yöntemler:
**1) Cosine Similarity (Sayısal Benzerlik)**

Her model için belirli bir giriş tweet’i (örneğin doc117) temel alınarak en benzer 5 tweet belirlenmiştir. Benzerlik, vektörler arası cosine benzerliği üzerinden hesaplanmıştır.

**2) Jaccard Benzerliği (Sıralama Tutarlılığı)**

Her modelin önerdiği en benzer 5 tweet sıralaması karşılaştırılarak, modeller arası benzerlik bir 18x18 Jaccard matrisi ile görselleştirilmiştir. Bu analiz ile model yapılandırmalarının sıralama tutarlılığına etkisi gözlenmiştir.

**3) Anlamsal Değerlendirme (İnsan Gözlemine Dayalı)**
Giriş tweet’i ile önerilen metinler arasındaki anlamsal benzerlik, 1–5 arasında puanlanmıştır:

1: Çok alakasız

5: Neredeyse aynı temada

Böylece, modellerin yalnızca sayısal olarak değil, aynı zamanda anlamsal düzeyde de başarısı analiz edilmiştir.

**Elde Edilen Sonuçlar:**

TF-IDF modelleri (özellikle TFIDF_Stem) anlamsal olarak en başarılı sonuçları üretmiştir. Ortalama skorları 4.4 ve 4.2 olarak ölçülmüştür.

Word2Vec modelleri (CBOW ve Skip-gram fark etmeksizin), cosine benzerlik skorları yüksek olmasına rağmen anlamca zayıf öneriler üretmiş ve semantik puanlamada tüm modellerin ortalaması 1.0 kalmıştır.

Jaccard matrisi sonuçlarına göre, Word2Vec modelleri kendi aralarında yüksek tutarlılığa sahiptir (ör. S_SG_2_100 ↔ S_SG_2_300: Jaccard = 1.00), ancak TF-IDF modelleri ile sıralama örtüşmesi göstermemiştir (Jaccard = 0.00).

Word2Vec yapılandırmalarında pencere boyutu ve vektör boyutunun sıralamayı etkilediği, ancak semantik başarıya doğrudan katkı sağlamadığı gözlenmiştir.

## Çalıştırma Talimatları

Bu proje Python (Jupyter Notebook) ortamında geliştirilmiş olup, veri ön işleme, vektörleştirme, benzerlik analizi ve değerlendirme süreçlerini içermektedir. Aşağıdaki adımları izleyerek projeyi başarıyla çalıştırabilirsiniz:

**Gereksinimler:**

Projeyi çalıştırmadan önce aşağıdaki Python kütüphanelerinin kurulu olduğundan emin olun:

pip install pandas numpy nltk gensim scikit-learn matplotlib seaborn openpyxl

Çalışmalar dataset klasörü içinde organize edilmiştir. Proje Dosya Yapısı:

datasett/

├── dataset.csv # Orijinal ham veri (kullanılmadı)

├── dataset_temiz.csv # "date" ve "content" sütunlarıyla sadeleştirilmiş sürüm

├── content_clean_versiyon.csv # Temizlenmiş içerik (stopword, özel karakter, vs. çıkarılmış)

├── lemma_dataset.csv # Lemmatizasyon uygulanmış (content_lemma sütunu içerir)

├── lemma_stem_dataset.csv # Hem lemmatization hem stemming uygulanmış veri

├── id_dataset.csv # Her tweet'e benzersiz bir id atanmış sürüm (ör: doc117).Bu sürümü elde etmek için nlp_dataset dosyasındaki adımları takip edebilirsiniz. Bu dosya drive da yok.

├── models/ # Toplam 16 Word2Vec modeli (.model uzantılı)

├── tf_idf_models/ # TF-IDF vectorizer ve sparse matris dosyaları (.pkl uzantılı)

├── jaccard_similarity_matrix.pdf # 18x18 Jaccard benzerlik matrisinin görselleştirilmiş hali

├── jaccard_similarity_matrix.xlsx # Jaccard skorlarının ham tablo hali (Excel)

└── nlp_dataset.ipynb # Tüm veri işleme, vektörleştirme ve analiz süreçlerini içeren Jupyter Notebook





Tüm işlemlerin bulunduğu son dosya id_dataset.csv dosyasıdır. Tüm analizler nlp_dataset.ipynb dosyası üzerinden çalıştırılmalıdır. Sıralama tutarlılığı için jaccard_similarity_matrix.xlsx dosyasındaki 18x18 Jaccard matrisi kullanılır. Anlamsal değerlendirme puanları id_dataset.xlsx üzerinden manuel olarak işlenmiştir.












