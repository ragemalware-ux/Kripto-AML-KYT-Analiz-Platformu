# Kripto-AML-KYT-Analiz-Platformu
Mevcut depoyu incele ve projeyi aşağıdaki hedef mimariye uyarlayarak çalışan, test edilebilir bir **Kripto Varlık, AML/KYT ve Risk Analiz Platformu** hâline getir. Yalnızca öneri veya örnek kod verme: kodu depoda uygula, gerekli testleri çalıştır ve sonucu doğrula.

## 1. Önce depoyu incele

Başlamadan önce:

- Mevcut dosyaları, teknoloji yığınını, API uçlarını, veritabanını, arayüzü ve testleri incele.
- Depoda zaten bulunan işlevleri ve eksikleri belirle.
- Mevcut çalışan yapıyı koruyarak ilerle; gerekmedikçe projeyi baştan yazma.
- Entegrasyonları kodlamadan önce her sağlayıcının **resmî API dokümantasyonunu**, ürün adını, erişim koşullarını, kimlik doğrulamasını, hız sınırlarını ve gerekli planı doğrula.
- Bir ürün veya API mevcut değilse, adını sessizce başka bir ürünle değiştirme. Entegrasyonu “doğrulanamadı/yapılandırılmadı” olarak belgele.
- API anahtarları yokken entegrasyon başarılıymış gibi davranma ve sahte canlı veri gösterme.

## 2. Ürün amacı ve kullanım sınırları

Platform; kullanıcı tarafından sağlanan cüzdan adresleri, işlem kimlikleri ve izinli/açık kaynaklardan gelen veriler üzerinde AML/KYT, dolandırıcılık göstergeleri ve blokzincir risk incelemesi yapsın. Bulgular araştırma ve uyum incelemesine yardımcı olsun; otomatik hukuki karar vermesin.

Her sonuçta kaynağı, sağlayıcıyı, sorgu zamanını, açıklamayı, güven düzeyini ve belirsizlikleri göster. Cüzdan bakiyesi veya tek bir risk etiketi, kişinin kimliğinin ya da suçluluğunun kanıtı olarak sunulmasın. Son karar insan incelemesine bırakılsın.

Bu sistem kişi bulup fon nakde çevirmek, broker bulmak veya alıcı-satıcı eşleştirmek için kullanılmayacak. Özel mesajları izinsiz toplama, kapalı gruplara girme, platform engellerini aşma, CAPTCHA çözme, proxy ile kimlik gizleme veya otomatik kişilere ulaşma işlevi ekleme.

## 3. Hedef klasör yapısı

Mevcut yapıyı mümkün olduğunca koru; gerekiyorsa aşağıdaki yapıya yaklaştır:

```text
global-intelligence/
├── backend/
│   ├── main.py
│   ├── api/
│   │   ├── wallets.py
│   │   ├── transactions.py
│   │   ├── reports.py
│   │   └── health.py
│   ├── services/
│   │   ├── providers/
│   │   │   ├── base.py
│   │   │   ├── chainalysis.py
│   │   │   ├── trm.py
│   │   │   ├── elliptic.py
│   │   │   ├── merkle_science.py
│   │   │   ├── blockchain_data.py
│   │   │   ├── scam_detection.py
│   │   │   └── kyc_aml.py
│   │   ├── risk_engine.py
│   │   ├── otc_analysis.py
│   │   └── report_service.py
│   ├── database/
│   ├── schemas/
│   ├── core/
│   └── tests/
├── frontend/
│   ├── dashboard/
│   ├── wallet-search/
│   ├── transaction-history/
│   ├── risk-review/
│   └── reports/
├── docker-compose.yml
├── .env.example
└── README.md
```

Bu ağaç bir hedeftir; mevcut çerçeveye uymayan dosyaları körlemesine taşımak yerine mevcut mimariye uygun karşılıklarını kullan.

## 4. API entegrasyonları

Sağlayıcıları tek tek, modüler adaptörler olarak uygula. Her adaptör ortak bir arayüz kullansın; sağlayıcı değişimi iş mantığını bozmasın. İhtiyaç olmayan entegrasyonları zorunlu kılma. Anahtar veya hesap erişimi bulunmayanlar devre dışı ve açıkça yapılandırılmamış görünsün.

### Blokzincir AML/KYT ve risk verileri

Aşağıdakileri aday entegrasyonlar olarak değerlendir ve resmî dokümanlarını kontrol et:

- **Chainalysis KYT** ve **AMLBot API**
- **TRM Labs**
- **Elliptic**
- **Merkle Science**

Her sağlayıcı için desteklenen ağları, adres/işlem sorgularını, risk kategorilerini, yanıt alanlarını ve fiyat/erişim gereksinimlerini doğrula. Sağlayıcılar aynı kavramlara farklı adlar verebilir; iç sistemde ortak bir risk şeması tanımla ve orijinal yanıtı kontrollü biçimde sakla.

### Zincir verisi ve gezgin kaynakları

İhtiyaca ve mevcut erişime göre şu kaynakları destekle:

- **GetBlock**
- **Bitquery**
- **Alchemy**
- **QuickNode**
- Solana ve Ethereum herkese açık gezgin/API kaynakları

**Risk eklentileri** veya özel veri paketleri varsa bunların sağlayıcı tarafından gerçekten sunulduğunu dokümantasyondan doğrula. Zincir verisi adaptörleri salt okunur çalışsın. Adres bakiyesi, işlem geçmişi ve ağ bilgisi döndürsün; fonların hukuki kaynağı ya da adres sahibinin kimliği hakkında kesin iddiada bulunmasın.

### Dolandırıcılık ve DeFi risk sinyalleri

Aşağıdaki entegrasyonları kullanılabilirlikleri ve resmî API koşulları bakımından kontrol et:

- **ScamSniffer API**
- **BlockSec Phalcon API**
- **Token Sniffer API**
- **DeFiLlama API**
- Topluluk hack listeleri

Topluluk listelerini tek başına kesin hüküm kabul etme. Her kayıtta liste adı, kaynak bağlantısı, son güncellenme zamanı ve eşleşme türü bulunsun. Lisansı ve kullanım izni belirsiz veriyi ürüne dâhil etme.

### KYC/AML ve yaptırım taraması

Aşağıdaki adayları doğrula ve her birinin işlevini doğru sınıflandır:

- **Sumsub**
- **Persona**
- **Veriff**
- **Civic API**
- **ComplyAdvantage**
- **OpenSanctions API**

Kimlik doğrulama ve kişi tarama özelliklerini yalnızca gerekli kullanıcı izni, yasal dayanak ve sağlayıcı erişimi varsa etkinleştir. Kimlik belgelerini gereksiz yere saklama. Geliştirme ortamına gerçek kişisel veri koyma.

### Cüzdan bağlantısı ve kimlik doğrulama

Aşağıdaki ürünleri, gerçekten gerekli oldukları kullanım senaryoları için değerlendir:

- **Fireblocks**
- **Turnkey API**
- **Privy**
- **Dynamic**

Bu projede varsayılan davranış cüzdanı bağlamak veya işlem imzalatmak değildir. Kullanılacaklarsa yalnızca açık kullanıcı eylemiyle ve resmî SDK/API üzerinden ekle. Özel anahtar, seed phrase veya imza sırrı sunucuya gönderilmesin ve loglanmasın.

### Yapay zekâ ve piyasa verisi

- **OpenAI API**: İsteğe bağlı metin sınıflandırma ve açıklama desteği. Gönderilecek veriyi en aza indir; sırları ve gereksiz kişisel verileri temizle. Model çıktısını kanıt veya kesin hüküm olarak gösterme.
- **CoinMarketCap API**: Gerekirse varlık sembolü/fiyat bilgisi için kullan. Sağlayıcı, zaman damgası ve veri gecikmesini göster.

## 5. Ortak entegrasyon kuralları

Her sağlayıcı adaptörü:

- Zaman aşımı, sınırlı yeniden deneme ve sağlayıcı hata yönetimi kullansın.
- Hız sınırlarına uysun; tekrar eden sorguları uygun şekilde önbelleğe alsın.
- API anahtarlarını yalnızca ortam değişkenlerinden veya güvenli yapılandırmadan alsın.
- Hata yanıtlarında hassas başlıkları ve anahtarları gizlesin.
- Sağlayıcı erişilemiyorsa sonucu “bilinmiyor/sağlayıcı hatası” olarak işaretlesin; bunu “temiz” veya “risk yok” diye yorumlamasın.
- Ortak iç şemaya dönüştürülmüş sonucu ve kaynak metaverisini saklasın.
- Sağlayıcıya özel alanları kaybetmeden denetlenebilirlik sağlasın.
- API anahtarı olmadan test edilebilmesi için sağlayıcı arayüzüne bağlı, açıkça demo olarak etiketlenmiş fixture/mock testleri kullansın. Mock veriyi canlı sonuç gibi arayüzde gösterme.

`.env.example` içine yalnızca değişken adlarını ve açıklamalarını koy; gerçek anahtar ekleme. Örnek değişkenler:

```text
DATABASE_URL=
OPENAI_API_KEY=
CHAINALYSIS_API_KEY=
AMLBOT_API_KEY=
TRM_API_KEY=
ELLIPTIC_API_KEY=
MERKLE_SCIENCE_API_KEY=
GETBLOCK_API_KEY=
BITQUERY_API_KEY=
ALCHEMY_API_KEY=
QUICKNODE_API_KEY=
SCAMSNIFFER_API_KEY=
BLOCKSEC_API_KEY=
TOKENSNIFFER_API_KEY=
SUMSUB_APP_TOKEN=
PERSONA_API_KEY=
VERIFF_API_KEY=
CIVIC_API_KEY=
COMPLYADVANTAGE_API_KEY=
OPENSANCTIONS_API_KEY=
COINMARKETCAP_API_KEY=
```

Dokümantasyonla doğrulanmayan değişkenleri kullanıma hazır gibi tanıtma.

## 6. Backend uçları

Mevcut API tasarımına uyarla; en azından aşağıdaki işlevleri sağla:

- `GET /health`: uygulama ve yapılandırılmış sağlayıcıların durumunu bildir.
- `GET /api/wallets/{chain}/{address}`: adres biçimini doğrula ve salt okunur genel bilgileri getir.
- `GET /api/wallets/{chain}/{address}/risk`: etkin AML/KYT ve risk sağlayıcılarından sonuçları al.
- `GET /api/transactions/{chain}/{tx_hash}`: işlem ayrıntısını getir.
- `POST /api/reports`: kullanıcı tarafından başlatılan inceleme için rapor oluştur.
- `GET /api/reports/{report_id}`: raporu ve kaynaklarını getir.
- `GET /api/providers`: kullanılabilir, yapılandırılmış ve kapalı entegrasyonların durumunu göster.

Girdi doğrulaması, tutarlı hata şeması, erişim kontrolü ve sorgu kayıtları ekle. Kimlik bilgilerini hiçbir API yanıtına koyma.

## 7. Risk motoru

Farklı sağlayıcıların sonuçlarını ortak bir şemada birleştir:

- Ağ ve incelenen adres/işlem
- Sağlayıcı ve kaynak bağlantısı
- Sağlayıcının ham kategori kodu
- Normalleştirilmiş kategori
- Sağlayıcı skoru varsa orijinal skor ve ölçeği
- Açıklama ve kanıt
- Sorgu zamanı ve veri güncelliği
- Hata veya eksik veri durumu
- İnsan incelemesi durumu

Sağlayıcı skorlarını ölçekleri aynıymış gibi doğrudan ortalama alma. Çelişen sonuçları gizleme; ayrı ayrı göster. Sistem kendi birleşik risk özetini verirse, bunun nasıl hesaplandığını açıklanabilir ve yapılandırılabilir yap. Sonuçlar “inceleme gerektiriyor” gibi temkinli durumlar kullansın.

## 8. Arayüz

Mevcut frontend teknolojisine uygun olarak şu ekranları oluştur veya tamamla:

- Gösterge paneli: son incelemeler ve sağlayıcı sağlık durumu
- Cüzdan arama: ağ ve açık adres girişi, biçim doğrulaması
- Cüzdan ayrıntısı: genel zincir verisi ve sağlayıcı sonuçları
- İşlem geçmişi ve işlem ayrıntısı
- Risk inceleme: kaynak bazında sonuçlar, çelişkiler ve insan inceleme notları
- Raporlar: rapor listesi, ayrıntı ve dışa aktarma

Yükleniyor, boş sonuç, sağlayıcıya ulaşılamıyor, yapılandırılmamış, geçersiz adres ve kısmi sonuç durumlarını ayrı ayrı göster. “Risk yok” ifadesini, sorgu başarısızken veya sağlayıcı yapılandırılmamışken gösterme. Tasarım mobil ekranda da kullanılabilir olsun.

## 9. Veritabanı ve mahremiyet

Mevcut veritabanı yaklaşımını takip ederek sağlayıcı sorgularını, raporları ve inceleme notlarını sakla. Gereksiz kişisel veri toplama. Saklanan veriler için mümkünse saklama süresi ve silme davranışı tanımla. Loglarda API anahtarları, özel anahtarlar, seed phrase, oturum belirteçleri veya gereksiz mesaj içeriği bulunmasın.

## 10. Test, çalıştırma ve teslim

Anlamlı testler ekle ve çalıştır:

- Doğru ve hatalı adres biçimi
- Her sağlayıcı için başarılı yanıt, hata ve zaman aşımı
- Birden fazla sağlayıcı sonucu ve çelişkili risk kategorileri
- API anahtarı yokken yapılandırılmamış durum
- API anahtarlarının log/yanıt içinde sızmaması
- Rapor oluşturma ve kaynakların raporda korunması
- Arayüzün boş, kısmi ve hata durumları

Mevcut lint, biçimlendirme, tip denetimi ve test komutlarını çalıştır. Docker Compose ve README’yi gerçek proje davranışına göre tamamla. README’de kurulum, ortam değişkenleri, hangi sağlayıcıların gerçekten entegre edildiği, erişim gereksinimleri, test ve sınırlamalar yer alsın.

İş bitince kısa bir sonuç ver: yapılan başlıca değişiklikler, gerçekten çalışan entegrasyonlar, anahtar/hesap gerektiren entegrasyonlar ve çalıştırılan kontroller. Başarısız veya doğrulanamayan hiçbir şeyi tamamlanmış gibi sunma.
