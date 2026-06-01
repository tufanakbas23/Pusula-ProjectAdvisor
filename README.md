# Pusula – TEKNOFEST ve TÜBİTAK Proje Rehberliği İçin Fine-Tuned Yapay Zeka Sistemi

Türkiye'deki öğrenciler için TEKNOFEST ve TÜBİTAK proje süreçlerinde yapay zeka destekli rehberlik sunmayı amaçlayan bir proje.

Bu proje, Türkiye'deki lisans ve lisansüstü öğrencilerin TEKNOFEST ve TÜBİTAK başvuru süreçlerinde daha doğru, daha tutarlı ve daha uygulanabilir rehberlik alabilmesi için geliştirilmektedir. Sistem, geçmiş proje raporları, başvuru metinleri ve proje yazım örneklerinden oluşturulan alan-özel bir veri kümesi üzerinde ince ayar yapılmış bir büyük dil modeli kullanır. Proje tamamlanınca dosyalar paylaşılacaktır süreç boyunca README güncellenecektir.

---

## Model

Fine-tune edilen model HuggingFace üzerinde yayınlanmıştır. Modeli indirmek veya doğrudan kullanmak için aşağıdaki bağlantıyı ziyaret edebilirsiniz:

**[tufanakbas23/Pusula-danisman-ai — HuggingFace](https://huggingface.co/tufanakbas23/Pusula-danisman-ai)**

---

## Motivasyon

TEKNOFEST ve TÜBİTAK başvuru süreçleri, özellikle ilk kez başvuru yapacak öğrenciler için karmaşık ve belirsiz olabilir. Problem tanımı nasıl yazılmalı, amaç ve yöntem nasıl ayrılmalı, özgün değer nasıl kurulmalı, jüri hangi noktalara dikkat eder gibi sorular çoğu zaman net ve uygulanabilir cevaplar bulamaz.

Bu projenin amacı, proje geliştirme ve proje yazımı süreçlerinde kullanıcılara dijital danışman benzeri destek sunan, alan-özel olarak eğitilmiş bir yapay zeka sistemi oluşturmaktır. Böylece kullanıcılar yalnızca genel amaçlı cevaplar değil, doğrudan TEKNOFEST ve TÜBİTAK bağlamına daha uygun rehberlik alabilir.

---

## Yaklaşım

Projenin ilk aşamasında RAG tabanlı bir yapı düşünülmüş olsa da, daha tutarlı ve görev odaklı çıktılar elde edebilmek için doğrudan fine-tuning yaklaşımı tercih edilmiştir.

Bu kapsamda:
- TEKNOFEST ve TÜBİTAK proje yazımıyla ilişkili örneklerden oluşan 4000+ veri noktası hazırlanmıştır.
- Önceden eğitilmiş bir büyük dil modeli bu alan-özel veri üzerinde fine-tune edilmiştir.
- Modelin çıktıları, proje fikri netleştirme, rapor bölümü yazma, başlık/özet üretme, uygulanabilirlik değerlendirmesi ve sunum/jüri hazırlığı gibi görevler için yapılandırılmıştır.
- Sistemin daha düzenli ve görev odaklı çalışabilmesi için skill tabanlı bir mimari kurulmuştur.

---

## Sistem Mimarisi

Sistem üç ana katmandan oluşur:

### 1. Veri ve Eğitim Katmanı
TEKNOFEST ve TÜBİTAK bağlamındaki proje metinleri, rapor örnekleri ve rehberlik içerikleri toplanır, temizlenir ve eğitim verisine dönüştürülür. Ardından bu veri kümesi ile önceden eğitilmiş LLM üzerinde fine-tuning yapılır.

### 2. Davranış Katmanı
Modelin hangi tonda, hangi kapsamda ve hangi görev mantığıyla cevap vereceğini belirlemek için sistem prompt, routing guide ve skill dosyaları kullanılır. Böylece model yalnızca metin üretmez; kullanıcının ihtiyacına göre doğru görev yapısına yönlendirilir.

### 3. Uygulama Katmanı
Kullanıcının sisteme erişebilmesi için web sitesi geliştirilmektedir. Bu katman, kullanıcıdan gelen proje sorularını alır, uygun yönlendirme mantığıyla modele iletir ve proje rehberliği çıktısını kullanıcıya sunar.

---

## Sağladığı Yetenekler

Sistem aşağıdaki görevlerde yardımcı olmayı hedefler:

- Belirsiz proje fikirlerini netleştirme
- Proje kapsamını daraltma
- Problem tanımı oluşturma
- Amaç, yöntem, özgün değer ve yaygın etki yazımı
- TÜBİTAK başvuru mantığına uygun rehberlik
- TEKNOFEST KTR/PTR ve teknik rapor desteği
- Uygulanabilirlik ve risk analizi
- Başlık, özet ve abstract üretimi
- Sunum, pitch ve jüri hazırlığı

---

## Kullanılan Yaklaşım

Projede genel amaçlı soru-cevap sistemi yerine, alan-özel ince ayar yapılmış bir model tercih edilmiştir. Bunun temel nedeni, kullanıcıların proje geliştirme ve rapor yazımı gibi görevlerde daha tutarlı, daha bağlama duyarlı ve daha yönlendirici cevaplar almasını sağlamaktır.

Bu yaklaşım sayesinde sistem:
- sadece bilgi veren bir yapı olmaktan çıkar,
- görev odaklı proje danışmanlığına yaklaşır,
- TEKNOFEST ve TÜBİTAK bağlamına daha uygun cevaplar üretir.

---

## Veri Kümesi

Model eğitimi için kullanılan veri kümesi, TEKNOFEST ve TÜBİTAK proje süreçleriyle ilişkili içeriklerden oluşturulmuştur.

Veri kümesi:
- 4000+ veri noktasından oluşur
- proje fikri geliştirme örnekleri içerir
- rapor bölümü yazımı örnekleri içerir
- başvuru rehberliği ve yapılandırılmış cevap örnekleri içerir
- sunum ve jüri hazırlığına yönelik örnekleri içerir

---

## Proje Yapısı

```text
project_root/
├── system_prompt.md
├── routing_guide.md
├── integration_note.md
├── sample_test_prompts.md
├── skills/
│   ├── README.md
│   ├── shared/
│   │   ├── output_style.md
│   │   ├── question_policy.md
│   │   └── competition_context.md
│   ├── project_idea_refinement.md
│   ├── report_section_writer.md
│   ├── tubitak_application_guidance.md
│   ├── teknofest_ktr_ptr_guidance.md
│   ├── feasibility_and_risk_check.md
│   ├── title_abstract_generator.md
│   └── presentation_and_jury_preparation.md
└── README.md
```

---

## Geliştirme Durumu

Şu ana kadar:
- veri kümesi hazırlanmıştır
- model fine-tuning süreci tamamlanmıştır
- skill tabanlı görev yapısı oluşturulmuştur
- system prompt ve routing yapısı hazırlanmıştır
- test senaryoları tanımlanmıştır
- web sitesi geliştirme süreci tamamlanmıştır.
- Proje bitmiştir.

---

## Hedef Kitle

Bu sistem özellikle şu kullanıcılar için tasarlanmaktadır:
- TEKNOFEST projesi hazırlayan öğrenciler
- TÜBİTAK başvurusu yapacak lise ve üniversite öğrencileri
- Proje fikrini netleştirmekte zorlanan ekipler
- Proje raporu yazarken rehber desteğe ihtiyaç duyan kullanıcılar

---

## Yol Haritası

- [x] Proje mimarisi oluşturulması
- [x] Veri kümesinin hazırlanması
- [x] Fine-tuning sürecinin tamamlanması
- [x] Skill tabanlı görev yapısının hazırlanması
- [x] System prompt ve routing mantığının oluşturulması
- [x] Model testlerinin tamamlanması
- [x] Web sitesi entegrasyonunun tamamlanması
- [x] Kullanıcı testleri ve geri bildirim toplanması
- [x] Cevap kalitesine göre prompt/skill revizyonları

---

## Amaç

Bu proje, öğrencilerin proje üretme ve proje yazma süreçlerinde daha erişilebilir, daha tutarlı ve daha pratik bir dijital rehber sunmayı amaçlar. Nihai hedef, kullanıcıların TEKNOFEST ve TÜBİTAK süreçlerinde yalnızca genel öneriler değil, doğrudan ihtiyaçlarına uygun yönlendirme alabilmesidir.

---

## Lisans

Bu proje MIT License ile lisanslanmıştır.

---

## İletişim

Proje hakkında soru veya öneriler için repo üzerinden iletişim kurulabilir.
