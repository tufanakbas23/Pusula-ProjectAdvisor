# RAG-tabanl-yerel-yapay-zeka-sistemi-Pusula-
Türkiye'deki öğrenciler için TEKNOFEST ve TÜBİTAK proje süreçlerinde yapay zeka destekli rehberlik sistemi.

Bu proje, Türkiye'deki lisans ve lisansüstü öğrencilerin TEKNOFEST ve TÜBİTAK başvuru süreçlerinde yapay zeka destekli rehberlik alabilmesi için geliştirilmektedir. Sistem, geçmiş proje raporlarını ve yarışma kılavuzlarını analiz ederek kullanıcıların sorularını kaynak göstererek yanıtlar.

---

## Motivasyon

TEKNOFEST ve TÜBİTAK başvuru süreçleri, özellikle ilk kez katılan öğrenciler için karmaşık olabiliyor. Proje amacını nasıl yazmalısın, metodoloji bölümünde ne bekleniliyor, jüri hangi kriterlere bakıyor gibi sorular genellikle net bir cevap bulamıyor. Danışman hocaya ulaşmak her zaman mümkün olmuyor, forumlardaki bilgiler ise çoğunlukla güncel değil.

Bu sistemin amacı, yüzlerce geçmiş proje raporunu ve resmi kılavuzları okuyup sindirmiş bir "dijital danışman" oluşturmak. Kullanıcı bir soru sorduğunda sistem, gerçek raporlardan ilgili bölümleri bulup modele besliyor ve kaynak göstererek yanıt üretiyor.

---

## Sistem Mimarisi

Sistem iki ana katmandan oluşuyor:

**1. Veri Katmanı (Ingestion Pipeline)**

PDF formatındaki proje raporları ve kılavuzlar sisteme yüklenir, metinlere ayrılır ve anlam vektörlerine dönüştürülerek bir vektör veritabanında saklanır. Bu işlem bir kez yapılır, raporlar güncellendiğinde tekrarlanır.

**2. Soru-Cevap Katmanı (RAG Pipeline)**

Kullanıcı bir soru sorduğunda sistem önce vektör veritabanında en alakalı rapor bölümlerini bulur, ardından bu bölümleri yerel LLM modeline bağlam olarak sunar. Model, yalnızca bu bağlama dayanarak yanıt üretir ve hangi kaynaktan yararlandığını belirtir.

```
Kullanıcı sorusu
      |
      v
Embedding modeli ile vektöre dönüştür
      |
      v
ChromaDB'de en yakın belge parçalarını bul
      |
      v
Qwen2.5 7B'ye bağlam + soru olarak ilet
      |
      v
Yanıt + kaynak referansı
```

---

## Teknoloji Yığını

| Katman | Teknoloji | Açıklama |
|---|---|---|
| LLM | Qwen2.5 7B (Q4_K_M) | Yerel çalışan, Türkçe performansı iyi model |
| Model Servisi | Ollama | Yerel LLM yönetimi |
| RAG Framework | LangChain | Retrieval pipeline |
| Vektör Veritabanı | ChromaDB | Yerel, kurulum gerektirmiyor |
| Embedding | sentence-transformers | Türkçe destekli çok dilli model |
| PDF İşleme | PyMuPDF | Metin ve tablo çıkarımı |
| Arayüz | Streamlit | Hızlı prototip UI |

Tüm sistem internetsiz, yerel olarak çalışacak. Raporlar dışarıya gönderilmiyor.

---

## Proje Yapısı

```
teknofest-rag/
├── data/
│   ├── reports/          # Ham PDF raporlar (git'e eklenmez)
│   └── vectorstore/      # ChromaDB index
├── src/
│   ├── ingestion/
│   │   ├── pdf_parser.py         # PDF'den metin çıkarma
│   │   ├── chunker.py            # Metni parçalara bölme
│   │   └── embedder.py           # Vektör oluşturma ve kaydetme
│   ├── retrieval/
│   │   ├── retriever.py          # ChromaDB sorgulama
│   │   └── reranker.py           # Sonuçları yeniden sıralama
│   ├── llm/
│   │   ├── ollama_client.py      # Ollama API istemcisi
│   │   └── prompt_templates.py   # Sistem ve kullanıcı promptları
│   └── api/
│       └── chain.py              # LangChain RAG zinciri
├── frontend/
│   └── app.py                    # Streamlit arayüzü
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   └── 02_rag_experiments.ipynb
├── tests/
│   ├── test_ingestion.py
│   └── test_retrieval.py
├── config.yaml                   # Model ve sistem ayarları
├── requirements.txt
└── README.md
```

---

## Kurulum

### Gereksinimler

- Python 3.11+
- [Ollama](https://ollama.ai) kurulu ve çalışıyor olmalı
- 8GB+ RAM (model için)
- 6GB VRAM (GPU inference için)

### Adımlar

```bash
# Repoyu klonla
git clone https://github.com/kullanici/teknofest-rag.git
cd teknofest-rag

# Sanal ortam oluştur
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Bağımlılıkları kur
pip install -r requirements.txt

# Modeli indir
ollama pull qwen2.5:7b

# Raporları sisteme yükle (PDF'leri data/reports/ altına koymuş olmalısın)
python src/ingestion/embedder.py

# Arayüzü başlat
streamlit run frontend/app.py
```

---

## Kullanım

Sistem başlatıldıktan sonra tarayıcıda `http://localhost:8501` adresine git. Örnek sorular:

- "TÜBİTAK 2204-A'da proje özeti nasıl yazılmalı?"
- "TEKNOFEST savunma kategorisinde hangi belgeler isteniyor?"
- "Geçen yıl kazanan yapay zeka projelerinde ortak neler var?"
- "Literatür taraması bölümünde kaç kaynak olmalı?"

Her yanıtın altında hangi raporlardan yararlandığı görünür.

---

## Veri Kaynakları

Sistem şu an aşağıdaki kaynaklarla besleniyor:

- TEKNOFEST başvuru kılavuzları (2021-2024)
- TÜBİTAK 2204-A/B proje rehberleri
- Finale kalan proje raporları (~500 rapor, 40-70 sayfa arası)

Raporlar gizlilik nedeniyle repoya eklenmemiştir. Sistemi kurmak isteyenler kendi kurumlarından raporları temin edebilir.

---

## Geliştirme Yol Haritası

- [x] Proje mimarisi tasarımı
- [x] Teknoloji seçimi ve kıyaslaması
- [ ] PDF ingestion pipeline
- [ ] ChromaDB vektör indeksi
- [ ] Temel RAG zinciri
- [ ] Streamlit arayüzü (v1)
- [ ] Türkçe embedding modeli karşılaştırması
- [ ] Kaynak gösterimi ve alıntı sistemi
- [ ] Performans değerlendirmesi (retrieval accuracy)
- [ ] Kullanıcı geri bildirim mekanizması

---

## Katkıda Bulunma

Proje aktif geliştirme aşamasında. Katkıda bulunmak isteyenler için iyi başlangıç noktaları:

- Türkçe embedding modellerinin karşılaştırılması
- Chunk boyutu ve overlap optimizasyonu
- Reranking stratejileri
- Arayüz iyileştirmeleri

PR göndermeden önce bir issue açıp ne yapmak istediğini belirtmen yeterli.

---

## Lisans

MIT License. Detaylar için [LICENSE](LICENSE) dosyasına bakabilirsin.

---

## İletişim

Proje hakkında soru veya önerileriniz için GitHub Issues kullanabilirsiniz.
