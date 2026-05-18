# Builder

| Alan | Değer |
|---|---|
| Ana Kategori | Application Design Patterns |
| Alt Kategori | Creational Patterns |
| Pattern | Builder |
| Dosya Yolu | `docs/application/creational/builder.md` |
| Odak | Tek uygulama / tek mikroservis içi kod mimarisi |
| Önerilen Katman | Application veya Infrastructure |

## 1. Kısa Tanım

Builder, karmaşık nesneleri adım adım oluşturur.

Örnekler, sektör bağımsız kalması için **Kurumsal Talep Yönetimi API'si** üzerinden verilmiştir. Bu örnek domain; talep oluşturma, onay akışı, audit log, bildirim simülasyonu, raporlama ve dış sistem entegrasyon simülasyonu gibi kurumsal uygulamalarda sık görülen ihtiyaçları temsil eder.

## 2. Çözdüğü Problem

Bu desen, kod içinde sorumlulukların dağılması, tekrar eden karar bloklarının çoğalması, test edilebilirliğin azalması veya teknik detayların iş akışına karışması gibi problemleri azaltmak için kullanılır.

Özellikle .NET tabanlı kurumsal API projelerinde amaç şudur:

- Controller veya endpoint seviyesini sade tutmak
- Application katmanında use-case akışını okunabilir hale getirmek
- Domain davranışlarını teknik detaylardan korumak
- Değişen davranışları izole etmek
- Kod tekrarını kontrollü biçimde azaltmak
- Unit test yazılabilecek küçük bileşenler üretmek

## 3. Kurumsal Talep Yönetimi Örneği

Yeni talep oluşturulurken başlık, açıklama, öncelik, ek bilgiler ve onay tercihleri adım adım hazırlanır.

Bu örnek, gerçek bir sektör bağımlılığı üretmeden desenin nasıl kullanılabileceğini gösterir. Talep oluşturma, onay, revizyon, audit ve raporlama akışları bu desen için yeterince zengin bir çalışma alanı sağlar.

## 4. .NET İçinde Kullanım Yaklaşımı

`RequestDraftBuilder`, `ReportQueryBuilder` veya test datası hazırlayan builder sınıflarında kullanılabilir.

Uygulama yapılırken aşağıdaki kurallar korunmalıdır:

- Interface ve class isimleri açık ve niyet belirten şekilde seçilmelidir.
- `Manager`, `Helper`, `Util` gibi belirsiz isimlerden kaçınılmalıdır.
- Public class ve public üyelerde XML Documentation Comment standardı uygulanmalıdır.
- Async operasyonlarda `CancellationToken` kullanılmalıdır.
- Domain entity doğrudan API contract olarak dışarı açılmamalıdır.
- Test edilebilirlik için somut bağımlılıklar yerine abstraction kullanılmalıdır.

## 5. Basit Akış

```text
Client -> Builder -> Valid Request Draft -> Command Handler
```

## 6. Örnek Kod / Taslak

```csharp
public sealed class RequestDraftBuilder
{
    private string _title = string.Empty;
    private string _description = string.Empty;
    private RequestPriority _priority = RequestPriority.Normal;

    public RequestDraftBuilder WithTitle(string title)
    {
        _title = title;
        return this;
    }

    public RequestDraftBuilder WithDescription(string description)
    {
        _description = description;
        return this;
    }

    public RequestDraftBuilder WithPriority(RequestPriority priority)
    {
        _priority = priority;
        return this;
    }

    public CreateRequestCommand Build()
    {
        return new CreateRequestCommand(_title, _description, _priority);
    }
}
```

## 7. Ne Zaman Kullanılır?

- Aynı davranış birden fazla yerde tekrar etmeye başladıysa
- Değişen kararları merkezi veya açık bir modele almak gerekiyorsa
- Unit test yazmak için davranışın izole edilmesi gerekiyorsa
- Controller, handler veya servis sınıfı fazla sorumluluk almaya başladıysa
- Yeni davranış eklerken mevcut kodu bozma riski yükseldiyse

## 8. Ne Zaman Kullanılmamalıdır?

- Problem henüz basitse ve desen gereksiz soyutlama üretecekse
- Tek kullanımlık, değişmeyecek ve kritik olmayan bir kod parçası için ağır bir yapı kurulacaksa
- Ekip deseni anlamadan sadece “pattern kullanmış olmak” için uygulanacaksa
- Daha sade bir method veya küçük class ayrımı yeterliyse

## 9. Avantajlar

- Kodun okunabilirliğini artırır.
- Sorumlulukları daha net ayırır.
- Test edilebilirliği güçlendirir.
- Değişiklik etkisini sınırlar.
- Clean Architecture yaklaşımını destekler.
- Domain ve application sınırlarını korumaya yardımcı olur.
