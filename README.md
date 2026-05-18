# PatternCraft
![Pattern Craft - Design Patterns Overview](assets/images/PatternCraft.png)

Application ve mikroservis tasarım desenleri için pratik referans dokümantasyon deposu.
## Kategori Yapısı
| Ana Kategori | Alt Kategoriler | Amaç |
|---|---|---|
| Application Design Patterns | Creational, Structural, Behavioral, .NET Application | Uygulama içindeki sınıf, nesne, katman ve kod organizasyonunu düzenler. |
| Microservice Design Patterns | Service Decomposition, Data Management, Communication, API Access, Distributed Transaction, Resilience, Infrastructure, Observability, Modernization | Birden fazla servisin birlikte nasıl tasarlanacağını, haberleşeceğini, ölçekleneceğini ve dayanıklı çalışacağını düzenler. |

## Tüm Tasarım Desenleri
| # | Ana Kategori | Alt Kategori | Tasarım Deseni | Kısa Açıklama | Link |
|---:|---|---|---|---|---|
| 1 | Application Design Patterns | Creational Patterns | **Abstract Factory** | Birbiriyle ilişkili nesne ailelerini üretir. | [Abstract Factory](docs/application/creational/abstract-factory.md) |
| 2 | Application Design Patterns | Creational Patterns | **Builder** | Karmaşık nesneleri adım adım oluşturur. | [Builder](docs/application/creational/builder.md) |
| 3 | Application Design Patterns | Creational Patterns | **Factory Method** | Nesne üretme kararını alt sınıflara veya factory yapısına bırakır. | [Factory Method](docs/application/creational/factory-method.md) |
| 4 | Application Design Patterns | Creational Patterns | **Prototype** | Var olan bir nesneyi kopyalayarak yeni nesne üretir. | [Prototype](docs/application/creational/prototype.md) |
| 5 | Application Design Patterns | Creational Patterns | **Singleton** | Bir sınıftan uygulama boyunca tek instance kullanılmasını sağlar. | [Singleton](docs/application/creational/singleton.md) |
| 6 | Application Design Patterns | Structural Patterns | **Adapter** | Uyumsuz arayüzleri sistemin beklediği yapıya uyarlar. | [Adapter](docs/application/structural/adapter.md) |
| 7 | Application Design Patterns | Structural Patterns | **Bridge** | Soyutlama ile implementasyonu birbirinden ayırır. | [Bridge](docs/application/structural/bridge.md) |
| 8 | Application Design Patterns | Structural Patterns | **Composite** | Parça-bütün ilişkisini ağaç yapısında modeller. | [Composite](docs/application/structural/composite.md) |
| 9 | Application Design Patterns | Structural Patterns | **Decorator** | Mevcut nesneye dinamik olarak ek sorumluluklar ekler. | [Decorator](docs/application/structural/decorator.md) |
| 10 | Application Design Patterns | Structural Patterns | **Facade** | Karmaşık alt sistemleri sade bir arayüzle dışarı açar. | [Facade](docs/application/structural/facade.md) |
| 11 | Application Design Patterns | Structural Patterns | **Flyweight** | Çok sayıda benzer nesne için ortak veriyi paylaşarak bellek tasarrufu sağlar. | [Flyweight](docs/application/structural/flyweight.md) |
| 12 | Application Design Patterns | Structural Patterns | **Proxy** | Gerçek nesneye erişimi kontrol eden vekil nesne sağlar. | [Proxy](docs/application/structural/proxy.md) |
| 13 | Application Design Patterns | Behavioral Patterns | **Chain of Responsibility** | Bir isteği handler zinciri boyunca işler. | [Chain of Responsibility](docs/application/behavioral/chain-of-responsibility.md) |
| 14 | Application Design Patterns | Behavioral Patterns | **Command** | Bir işlemi nesne olarak temsil eder. | [Command](docs/application/behavioral/command.md) |
| 15 | Application Design Patterns | Behavioral Patterns | **Interpreter** | Basit dil, ifade veya kural yapısını yorumlar. | [Interpreter](docs/application/behavioral/interpreter.md) |
| 16 | Application Design Patterns | Behavioral Patterns | **Iterator** | Koleksiyonlar üzerinde standart gezinme mekanizması sağlar. | [Iterator](docs/application/behavioral/iterator.md) |
| 17 | Application Design Patterns | Behavioral Patterns | **Mediator** | Nesneler arası doğrudan bağımlılığı merkezi aracıyla azaltır. | [Mediator](docs/application/behavioral/mediator.md) |
| 18 | Application Design Patterns | Behavioral Patterns | **Memento** | Nesnenin önceki durumunu saklayıp geri yüklemeyi sağlar. | [Memento](docs/application/behavioral/memento.md) |
| 19 | Application Design Patterns | Behavioral Patterns | **Observer** | Bir olay veya değişiklik olduğunda aboneleri bilgilendirir. | [Observer](docs/application/behavioral/observer.md) |
| 20 | Application Design Patterns | Behavioral Patterns | **State** | Nesnenin davranışını mevcut durumuna göre değiştirir. | [State](docs/application/behavioral/state.md) |
| 21 | Application Design Patterns | Behavioral Patterns | **Strategy** | Değişebilir algoritmaları ayrı sınıflar halinde yönetir. | [Strategy](docs/application/behavioral/strategy.md) |
| 22 | Application Design Patterns | Behavioral Patterns | **Template Method** | Genel algoritma akışını sabit tutup bazı adımları özelleştirir. | [Template Method](docs/application/behavioral/template-method.md) |
| 23 | Application Design Patterns | Behavioral Patterns | **Visitor** | Nesne yapısını değiştirmeden yeni operasyonlar eklemeyi sağlar. | [Visitor](docs/application/behavioral/visitor.md) |
| 24 | Application Design Patterns | .NET Application Patterns | **Dependency Injection** | Bağımlılıkların dışarıdan verilmesini sağlayarak gevşek bağlı yapı kurar. | [Dependency Injection](docs/application/dotnet-application/dependency-injection.md) |
| 25 | Application Design Patterns | .NET Application Patterns | **Options Pattern** | Konfigürasyonları strongly typed sınıflarla yönetir. | [Options Pattern](docs/application/dotnet-application/options-pattern.md) |
| 26 | Application Design Patterns | .NET Application Patterns | **Middleware Pattern** | HTTP request pipeline üzerinde ortak davranışları yönetir. | [Middleware Pattern](docs/application/dotnet-application/middleware-pattern.md) |
| 27 | Application Design Patterns | .NET Application Patterns | **Repository Pattern** | Veri erişim detaylarını application/domain katmanından soyutlar. | [Repository Pattern](docs/application/dotnet-application/repository-pattern.md) |
| 28 | Application Design Patterns | .NET Application Patterns | **Unit of Work** | Birden fazla veri değişikliğini tek transaction kapsamında yönetir. | [Unit of Work](docs/application/dotnet-application/unit-of-work.md) |
| 29 | Application Design Patterns | .NET Application Patterns | **CQRS** | Command ve Query sorumluluklarını ayırır. | [CQRS](docs/application/dotnet-application/cqrs.md) |
| 30 | Application Design Patterns | .NET Application Patterns | **Domain Event** | Domain içinde oluşan önemli olayları event olarak modeller. | [Domain Event](docs/application/dotnet-application/domain-event.md) |
| 31 | Application Design Patterns | Behavioral Patterns | **Specification Pattern** | İş kurallarını tekrar kullanılabilir koşullar olarak tanımlar. | [Specification Pattern](docs/application/dotnet-application/specification-pattern.md) |
| 32 | Application Design Patterns | .NET Application Patterns | **Application Service** | Use-case seviyesindeki uygulama akışlarını yönetir. | [Application Service](docs/application/dotnet-application/application-service.md) |
| 33 | Application Design Patterns | .NET Application Patterns | **Validation Pipeline** | Komut ve istekleri merkezi doğrulama zincirinden geçirir. | [Validation Pipeline](docs/application/dotnet-application/validation-pipeline.md) |
| 34 | Application Design Patterns | .NET Application Patterns | **Result Pattern** | Başarı/hata çıktısını exception dışı standart bir modelle temsil eder. | [Result Pattern](docs/application/dotnet-application/result-pattern.md) |
| 35 | Application Design Patterns | .NET Application Patterns | **Domain Service** | Tek bir entity içine koyulamayan domain davranışlarını taşır. | [Domain Service](docs/application/dotnet-application/domain-service.md) |
| 36 | Application Design Patterns | .NET Application Patterns | **Value Object** | Kimliği olmayan, değeriyle anlam kazanan domain nesnelerini modeller. | [Value Object](docs/application/dotnet-application/value-object.md) |
| 37 | Application Design Patterns | .NET Application Patterns | **Aggregate** | DDD içinde tutarlılık sınırını belirler. | [Aggregate](docs/application/dotnet-application/aggregate.md) |
| 38 | Microservice Design Patterns | Service Decomposition Patterns | **Decompose by Business Capability** | Servisleri iş kabiliyetlerine göre böler. | [Decompose by Business Capability](docs/microservice/service-decomposition/decompose-by-business-capability.md) |
| 39 | Microservice Design Patterns | Service Decomposition Patterns | **Decompose by Subdomain** | Servisleri DDD subdomain sınırlarına göre böler. | [Decompose by Subdomain](docs/microservice/service-decomposition/decompose-by-subdomain.md) |
| 40 | Microservice Design Patterns | Service Decomposition Patterns | **Bounded Context** | Domain modelinin geçerli olduğu sınırı netleştirir. | [Bounded Context](docs/microservice/service-decomposition/bounded-context.md) |
| 41 | Microservice Design Patterns | Data Management Patterns | **Database per Service** | Her servisin kendi veritabanına sahip olmasını sağlar. | [Database per Service](docs/microservice/data-management/database-per-service.md) |
| 42 | Microservice Design Patterns | Data Management Patterns | **Shared Database** | Geçiş dönemlerinde birden fazla servisin aynı veritabanını kullanmasıdır. | [Shared Database](docs/microservice/data-management/shared-database.md) |
| 43 | Microservice Design Patterns | Data Management Patterns | **API Composition** | Dağıtık veriyi birden fazla servisten toplayarak cevap oluşturur. | [API Composition](docs/microservice/data-management/api-composition.md) |
| 44 | Microservice Design Patterns | Data Management Patterns | **CQRS** | Mikroservislerde okuma ve yazma modellerini ayırır. | [CQRS](docs/microservice/data-management/cqrs.md) |
| 45 | Microservice Design Patterns | Data Management Patterns | **Event Sourcing** | Son durum yerine olay geçmişini saklar. | [Event Sourcing](docs/microservice/data-management/event-sourcing.md) |
| 46 | Microservice Design Patterns | Data Management Patterns | **Materialized View** | Okuma performansı için hazır okuma modeli oluşturur. | [Materialized View](docs/microservice/data-management/materialized-view.md) |
| 47 | Microservice Design Patterns | Communication Patterns | **Synchronous Communication** | Servislerin HTTP/gRPC gibi anlık çağrılarla haberleşmesidir. | [Synchronous Communication](docs/microservice/communication/synchronous-communication.md) |
| 48 | Microservice Design Patterns | Communication Patterns | **Asynchronous Messaging** | Servislerin message broker üzerinden gevşek bağlı haberleşmesidir. | [Asynchronous Messaging](docs/microservice/communication/asynchronous-messaging.md) |
| 49 | Microservice Design Patterns | Communication Patterns | **Publisher / Subscriber** | Bir olayın birden fazla abone servis tarafından dinlenmesini sağlar. | [Publisher / Subscriber](docs/microservice/communication/publisher-subscriber.md) |
| 50 | Microservice Design Patterns | Communication Patterns | **Request / Reply Messaging** | Mesajlaşma üzerinden istek-cevap modeli kurar. | [Request / Reply Messaging](docs/microservice/communication/request-reply-messaging.md) |
| 51 | Microservice Design Patterns | Communication Patterns | **Messaging Bridge** | Farklı mesajlaşma sistemleri arasında köprü kurar. | [Messaging Bridge](docs/microservice/communication/messaging-bridge.md) |
| 52 | Microservice Design Patterns | API Access Patterns | **API Gateway** | Client uygulamalar için tek giriş noktası sağlar. | [API Gateway](docs/microservice/api-access/api-gateway.md) |
| 53 | Microservice Design Patterns | API Access Patterns | **Backend for Frontend** | Her frontend tipi için özel backend katmanı oluşturur. | [Backend for Frontend](docs/microservice/api-access/backend-for-frontend.md) |
| 54 | Microservice Design Patterns | API Access Patterns | **Gateway Routing** | Gelen istekleri ilgili mikroservise yönlendirir. | [Gateway Routing](docs/microservice/api-access/gateway-routing.md) |
| 55 | Microservice Design Patterns | API Access Patterns | **Gateway Aggregation** | Birden fazla servisten veri toplayıp tek cevap döner. | [Gateway Aggregation](docs/microservice/api-access/gateway-aggregation.md) |
| 56 | Microservice Design Patterns | API Access Patterns | **Gateway Offloading** | SSL, auth, rate limit gibi ortak işleri gateway’e taşır. | [Gateway Offloading](docs/microservice/api-access/gateway-offloading.md) |
| 57 | Microservice Design Patterns | Distributed Transaction Patterns | **Saga** | Dağıtık işlemleri yerel transaction ve telafi adımlarıyla yönetir. | [Saga](docs/microservice/distributed-transaction/saga.md) |
| 58 | Microservice Design Patterns | Distributed Transaction Patterns | **Saga Orchestration** | Merkezi orchestrator ile dağıtık akışı yönetir. | [Saga Orchestration](docs/microservice/distributed-transaction/saga-orchestration.md) |
| 59 | Microservice Design Patterns | Distributed Transaction Patterns | **Saga Choreography** | Servislerin event’lerle birbirini tetiklediği saga yaklaşımıdır. | [Saga Choreography](docs/microservice/distributed-transaction/saga-choreography.md) |
| 60 | Microservice Design Patterns | Distributed Transaction Patterns | **Transactional Outbox** | DB kaydı ile event yayınlamayı güvenli hale getirir. | [Transactional Outbox](docs/microservice/distributed-transaction/transactional-outbox.md) |
| 61 | Microservice Design Patterns | Distributed Transaction Patterns | **Inbox / Idempotent Consumer** | Aynı mesajın tekrar işlenmesini engeller. | [Inbox / Idempotent Consumer](docs/microservice/distributed-transaction/inbox-idempotent-consumer.md) |
| 62 | Microservice Design Patterns | Distributed Transaction Patterns | **Compensating Transaction** | Hata durumunda önceki başarılı adımları telafi eder. | [Compensating Transaction](docs/microservice/distributed-transaction/compensating-transaction.md) |
| 63 | Microservice Design Patterns | Resilience Patterns | **Retry** | Geçici hatalarda işlemi kontrollü şekilde tekrar dener. | [Retry](docs/microservice/resilience/retry.md) |
| 64 | Microservice Design Patterns | Resilience Patterns | **Circuit Breaker** | Hatalı servise sürekli çağrı yapılmasını engeller. | [Circuit Breaker](docs/microservice/resilience/circuit-breaker.md) |
| 65 | Microservice Design Patterns | Resilience Patterns | **Timeout** | Servis çağrıları için maksimum bekleme süresi belirler. | [Timeout](docs/microservice/resilience/timeout.md) |
| 66 | Microservice Design Patterns | Resilience Patterns | **Bulkhead** | Kaynakları izole ederek hatanın tüm sisteme yayılmasını önler. | [Bulkhead](docs/microservice/resilience/bulkhead.md) |
| 67 | Microservice Design Patterns | Resilience Patterns | **Rate Limiting** | Belirli zaman aralığında yapılabilecek istek sayısını sınırlar. | [Rate Limiting](docs/microservice/resilience/rate-limiting.md) |
| 68 | Microservice Design Patterns | Resilience Patterns | **Throttling** | Yoğunluk durumunda sistemi korumak için istekleri yavaşlatır veya reddeder. | [Throttling](docs/microservice/resilience/throttling.md) |
| 69 | Microservice Design Patterns | Resilience Patterns | **Fallback** | Ana akış çalışmadığında alternatif cevap veya davranış sağlar. | [Fallback](docs/microservice/resilience/fallback.md) |
| 70 | Microservice Design Patterns | Infrastructure Patterns | **Service Discovery** | Servislerin dinamik olarak birbirini bulmasını sağlar. | [Service Discovery](docs/microservice/infrastructure/service-discovery.md) |
| 71 | Microservice Design Patterns | Infrastructure Patterns | **Client-side Discovery** | Client’ın hedef servis adresini registry üzerinden bulmasıdır. | [Client-side Discovery](docs/microservice/infrastructure/client-side-discovery.md) |
| 72 | Microservice Design Patterns | Infrastructure Patterns | **Server-side Discovery** | Servis yönlendirmesinin platform veya load balancer tarafından yapılmasıdır. | [Server-side Discovery](docs/microservice/infrastructure/server-side-discovery.md) |
| 73 | Microservice Design Patterns | Infrastructure Patterns | **Externalized Configuration** | Konfigürasyonun uygulama kodundan ayrılmasıdır. | [Externalized Configuration](docs/microservice/infrastructure/externalized-configuration.md) |
| 74 | Microservice Design Patterns | Infrastructure Patterns | **Sidecar** | Ana uygulamanın yanında yardımcı container/process çalıştırır. | [Sidecar](docs/microservice/infrastructure/sidecar.md) |
| 75 | Microservice Design Patterns | Infrastructure Patterns | **Ambassador** | Dış servis iletişimini temsilci proxy üzerinden yönetir. | [Ambassador](docs/microservice/infrastructure/ambassador.md) |
| 76 | Microservice Design Patterns | Infrastructure Patterns | **Service Mesh** | Servisler arası trafiği platform seviyesinde yönetir. | [Service Mesh](docs/microservice/infrastructure/service-mesh.md) |
| 77 | Microservice Design Patterns | Observability Patterns | **Log Aggregation** | Log’ları merkezi bir yerde toplar. | [Log Aggregation](docs/microservice/observability/log-aggregation.md) |
| 78 | Microservice Design Patterns | Observability Patterns | **Application Metrics** | Uygulama ve sistem metriklerini toplar. | [Application Metrics](docs/microservice/observability/application-metrics.md) |
| 79 | Microservice Design Patterns | Observability Patterns | **Distributed Tracing** | Dağıtık request akışını servisler arasında izler. | [Distributed Tracing](docs/microservice/observability/distributed-tracing.md) |
| 80 | Microservice Design Patterns | Observability Patterns | **Audit Logging** | Kritik iş olaylarını denetlenebilir şekilde kaydeder. | [Audit Logging](docs/microservice/observability/audit-logging.md) |
| 81 | Microservice Design Patterns | Observability Patterns | **Health Check** | Servisin çalışabilirlik ve hazırlık durumunu bildirir. | [Health Check](docs/microservice/observability/health-check.md) |
| 82 | Microservice Design Patterns | Observability Patterns | **Exception Tracking** | Hataları merkezi olarak takip etmeyi sağlar. | [Exception Tracking](docs/microservice/observability/exception-tracking.md) |
| 83 | Microservice Design Patterns | Modernization Patterns | **Strangler Fig** | Legacy sistemi parça parça yeni yapıya taşır. | [Strangler Fig](docs/microservice/modernization/strangler-fig.md) |
| 84 | Microservice Design Patterns | Modernization Patterns | **Anti-Corruption Layer** | Farklı model kullanan sistemler arasında koruyucu çeviri katmanı sağlar. | [Anti-Corruption Layer](docs/microservice/modernization/anti-corruption-layer.md) |
| 85 | Microservice Design Patterns | Modernization Patterns | **Canonical Data Model** | Sistemler arası ortak veri modeli oluşturur. | [Canonical Data Model](docs/microservice/modernization/canonical-data-model.md) |
| 86 | Microservice Design Patterns | Modernization Patterns | **Open Host Service** | Domain kabiliyetlerini dış sistemlere kontrollü API ile açar. | [Open Host Service](docs/microservice/modernization/open-host-service.md) |
| 87 | Microservice Design Patterns | Modernization Patterns | **Published Language** | Sistemler arası ortak sözleşme dili oluşturur. | [Published Language](docs/microservice/modernization/published-language.md) |

