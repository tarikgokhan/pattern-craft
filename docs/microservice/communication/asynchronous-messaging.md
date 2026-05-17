# Asynchronous Messaging

| Alan | Değer |
|---|---|
| Ana Kategori | Microservice Design Patterns |
| Alt Kategori | Communication Patterns |
| Pattern | Asynchronous Messaging |
| Dosya Yolu | `docs/microservice/communication/asynchronous-messaging.md` |
| Odak | Dağıtık sistem / mikroservis mimarisi |
| Önerilen Seviye | Servis sınırı, veri sahipliği, iletişim, dayanıklılık veya operasyon |

## 1. Kısa Tanım

Asynchronous Messaging, servislerin message broker üzerinden gevşek bağlı haberleşmesidir.

Örnekler, sektör bağımsız kalması için **Kurumsal Talep Yönetimi API'si** üzerinden verilmiştir. Bu örnek domain; talep oluşturma, onay akışı, audit log, bildirim simülasyonu, raporlama ve dış sistem entegrasyon simülasyonu gibi kurumsal uygulamalarda sık görülen ihtiyaçları temsil eder.

## 2. Çözdüğü Problem

Mikroservis mimarilerinde karmaşıklık çoğu zaman class seviyesinde değil; servis sınırı, network iletişimi, veri sahipliği, dağıtık transaction, hata yönetimi ve operasyonel izleme seviyesinde ortaya çıkar.

Bu desen aşağıdaki sorunları yönetmek için değerlendirilir:

- Servislerin birbirine gereğinden fazla bağımlı hale gelmesi
- Veri sahipliğinin belirsizleşmesi
- Dağıtık transaction ihtiyacının kontrolsüz büyümesi
- Runtime bağımlılıkların sistem genelinde hata yayması
- Log, metric ve trace verilerinin parçalı kalması
- Geçiş veya modernizasyon sürecinde eski ve yeni yapıların birbirini bozması

## 3. Kurumsal Talep Yönetimi Örneği

Talep onaylandığında bildirim ve raporlama güncellemeleri event üzerinden tetiklenir.

Bu örnek; RequestService, ApprovalService, NotificationService, ReportingService ve AuditService gibi sektör bağımsız servis adayları üzerinden düşünülebilir.

## 4. .NET ve Cloud-Native Yaklaşımı

Servisler gevşek bağlı olur ve kullanıcı akışı sadeleşir.

.NET tabanlı uygulamada aşağıdaki prensiplerle birlikte ele alınmalıdır:

- Her servis kendi bounded context ve veri sahipliği ile düşünülmelidir.
- API contract ve event contract geriye dönük uyumluluk gözetilerek versiyonlanmalıdır.
- Servisler arası çağrılarda timeout ve observability standart olmalıdır.
- Asenkron mesajlaşmada idempotency ve duplicate message kontrolü tasarlanmalıdır.
- Health check, structured logging, metric ve distributed tracing varsayılan kabul edilmelidir.
- Container ve CI/CD pipeline tasarımı servis bağımsızlığını desteklemelidir.

## 5. Basit Akış

```text
RequestApprovedEvent -> NotificationService + ReportingService
```

## 6. Örnek Mimari Taslak

```text
Client / Scheduler / Worker
        ↓
API Gateway veya Service Endpoint
        ↓
RequestService / ApprovalService / ReportingService
        ↓
Own Database / Message Broker / Observability Platform
```

Bu taslak desene göre sadeleştirilebilir veya genişletilebilir. Amaç, pattern’in hangi mimari seviyede karar ürettiğini görünür hale getirmektir.

## 7. Ne Zaman Kullanılır?

- Birden fazla servisin birlikte çalıştığı bir akış varsa
- Tek servis içi çözüm artık yeterli değilse
- Veri sahipliği veya servis sınırı netleştirilmek isteniyorsa
- Hata yayılımı, retry, timeout veya observability ihtiyacı oluştuysa
- Legacy yapıdan yeni mimariye kontrollü geçiş gerekiyorsa
- Client ihtiyaçları mikroservislerin ham API’lerinden farklıysa

## 8. Ne Zaman Kullanılmamalıdır?

- Sistem tek servisle sade şekilde çözülebiliyorsa
- Operasyonel ekip dağıtık sistem karmaşıklığını yönetmeye hazır değilse
- Desenin getirdiği altyapı maliyeti iş değerinden yüksekse
- Sadece teknolojik görünmek için mikroservis ayrımı yapılacaksa
- Takım API contract, versiyonlama ve observability disiplinine sahip değilse

## 9. Avantajlar

- Servis bağımsızlığını artırır.
- Değişiklik etkisini sınırlar.
- Ölçeklenebilirliği ve dayanıklılığı güçlendirir.
- Operasyonel görünürlüğü artırır.
- Modernizasyon sürecini daha kontrollü hale getirir.
- Domain sınırlarının korunmasına yardımcı olur.

## 10. Dikkat Edilecekler

- Mikroservis deseni, tek başına mimari kalite garantisi değildir.
- Her dağıtık karar network, güvenlik, deployment ve izleme maliyeti getirir.
- Eventual consistency kullanıcı deneyimi ve raporlama tarafında açıkça tasarlanmalıdır.
- Contract değişiklikleri release yönetimiyle birlikte ele alınmalıdır.
- Her servis kendi verisini korumalı; başka servis veritabanına doğrudan erişmemelidir.

## 11. Kontrol Listesi

- [ ] Bu desen gerçek bir dağıtık sistem problemini çözüyor mu?
- [ ] Servis sınırı ve veri sahipliği net mi?
- [ ] Timeout, retry, idempotency ve observability ihtiyaçları değerlendirildi mi?
- [ ] API veya event contract versiyonlama stratejisi var mı?
- [ ] Operasyonel karmaşıklık ekip tarafından yönetilebilir mi?
- [ ] Örnekler domain bağımsız ve güvenli mi?
