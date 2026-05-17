# Proxy

| Alan | Değer |
|---|---|
| Ana Kategori | Application Design Patterns |
| Alt Kategori | Structural Patterns |
| Pattern | Proxy |
| Dosya Yolu | `docs/application/structural/proxy.md` |
| Odak | Tek uygulama / tek mikroservis içi kod mimarisi |
| Önerilen Katman | Infrastructure, Application veya API sınırı |

## 1. Kısa Tanım

Proxy, gerçek nesneye erişimi kontrol eden vekil nesne sağlar.

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

Rapor sorgusu veya dış sistem çağrısı önüne cache, yetki veya rate kontrolü yapan vekil nesne konur.

Bu örnek, gerçek bir sektör bağımlılığı üretmeden desenin nasıl kullanılabileceğini gösterir. Talep oluşturma, onay, revizyon, audit ve raporlama akışları bu desen için yeterince zengin bir çalışma alanı sağlar.

## 4. .NET İçinde Kullanım Yaklaşımı

`CachedRequestQueryProxy`, `AuthorizedDocumentServiceProxy` gibi uygulanabilir.

Uygulama yapılırken aşağıdaki kurallar korunmalıdır:

- Interface ve class isimleri açık ve niyet belirten şekilde seçilmelidir.
- `Manager`, `Helper`, `Util` gibi belirsiz isimlerden kaçınılmalıdır.
- Public class ve public üyelerde XML Documentation Comment standardı uygulanmalıdır.
- Async operasyonlarda `CancellationToken` kullanılmalıdır.
- Domain entity doğrudan API contract olarak dışarı açılmamalıdır.
- Test edilebilirlik için somut bağımlılıklar yerine abstraction kullanılmalıdır.

## 5. Basit Akış

```text
Client -> Proxy -> Real Service
```

## 6. Örnek Kod / Taslak

```csharp
public sealed class CachedRequestQueryProxy : IRequestQueryService
{
    private readonly IRequestQueryService _inner;
    private readonly IMemoryCache _cache;

    public CachedRequestQueryProxy(IRequestQueryService inner, IMemoryCache cache)
    {
        _inner = inner;
        _cache = cache;
    }

    public Task<RequestDetailResponse> GetDetailAsync(Guid id, CancellationToken cancellationToken)
    {
        return _cache.GetOrCreateAsync($"request-detail:{id}", _ => _inner.GetDetailAsync(id, cancellationToken))!;
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

## 10. Dikkat Edilecekler

- Desen, gerçek bir problemi çözmelidir.
- Fazla abstraction kodun anlaşılmasını zorlaştırabilir.
- Dosya ve namespace isimleri ana README yapısıyla uyumlu olmalıdır.
- Public API yüzeyi XML comment ile dokümante edilmelidir.
- Örnek domain dışında gerçek şirket, gerçek müşteri veya hassas iş modeli adı kullanılmamalıdır.

## 11. Kontrol Listesi

- [ ] Desen gerçek bir tekrar, değişkenlik veya bağımlılık problemini çözüyor mu?
- [ ] Class ve interface isimleri niyeti açık anlatıyor mu?
- [ ] Katman sorumlulukları korunuyor mu?
- [ ] Unit test yazmak kolay mı?
- [ ] Public üyeler XML Documentation Comment içeriyor mu?
- [ ] Örnekler domain bağımsız mı?
