# Builder

## 1. Kısa Tanım

Builder, çok parçalı ve kurallı bir nesneyi tek seferde karmaşık constructor çağrılarıyla kurmak yerine, adım adım ve okunabilir bir akışla üretmeyi sağlar.

Kısacası: Nesneyi "bir kerede doldur" yerine "anlamlı adımlarla hazırla, en sonda doğrula ve üret" yaklaşımıdır.

## 2. Çözdüğü Problem

Builder en çok şu noktada fark yaratır: Ortada tek bir nesne vardır ama bu nesnenin oluşması için zorunlu alanlar, opsiyonel tercihler ve birden fazla iş kuralı aynı anda devrededir.

Bu durumda constructor tabanlı yaklaşım hızla yorucu hale gelir:

- Parametre listesi uzar ve okunabilirlik düşer.
- Parametre sırası hataları sessizce üretime sızabilir.
- Hangi alanın zorunlu, hangisinin opsiyonel olduğu belirsizleşir.
- Doğrulama mantığı farklı katmanlara dağılır.

Builder ise bu yükü tek yerde toplar:

- Kurulum adımlarını niyet odaklı metotlarla görünür yapar.
- `Build()` anında doğrulama yaparak geçersiz nesne üretimini engeller.
- Aynı ürünün farklı varyasyonlarını tekrar etmeden oluşturmayı kolaylaştırır.

## 3. İş Modeli Örneği (Etkinlik Programı Yayınlama)

Bir etkinlik platformunda "program yayınlama" komutu oluşturduğunuzu düşünün. Yayına çıkmak için etkinlik adı, tarih aralığı ve mekan zorunlu; konuşmacılar, canlı yayın linki, sponsor paketleri ve bildirim ayarları ise opsiyonel.

Bu verileri tek constructor çağrısına sıkıştırmak yerine Builder ile adım adım topladığınızda akış doğal hale gelir: önce temel bilgileri kurar, sonra opsiyonel parçaları eklersiniz. `Build()` aşamasında da "yayınlanabilir program" kuralları merkezi olarak doğrulanır.

Bu senaryoda Builder sadece estetik bir tercih değildir; doğrudan doğruya geçersiz program üretimini azaltan ve kullanım akışını netleştiren bir yapı taşıdır.

## 4. .NET İçinde Kullanım Yaklaşımı

.NET tarafında Builder çoğunlukla `Command`, `Query`, `Options` veya test verisi üretiminde kullanılır.

Uygularken pratik bir denge kurmak önemlidir:

- Public API yüzeyini XML documentation comments ile açık tutun.
- Builder içinde toplanan doğrulama kurallarını `Build()` aşamasında çalıştırın.
- Ürünü mümkünse immutable olarak döndürün.
- 2-3 alanlı basit modellerde Builder yazma refleksine kapılmayın; gerçekten karmaşa varsa kullanın.

## 5. Basit Akış

```text
Client -> EventProgramBuilder -> Build() doğrulaması -> PublishEventProgramCommand
```

## 6. Örnek Kod / Taslak

```csharp
/// <summary>
/// Etkinlik programı yayınlama komutunu adım adım oluşturan Builder sınıfı.
/// </summary>
public sealed class EventProgramBuilder
{
    private string _eventName = string.Empty;
    private DateTimeOffset? _startsAt;
    private DateTimeOffset? _endsAt;
    private string _venue = string.Empty;
    private readonly List<string> _speakers = new();
    private Uri? _liveStreamUrl;

    /// <summary>
    /// Etkinlik adını ayarlar.
    /// </summary>
    public EventProgramBuilder WithEventName(string eventName)
    {
        _eventName = eventName;
        return this;
    }

    /// <summary>
    /// Etkinliğin başlangıç ve bitiş zamanını ayarlar.
    /// </summary>
    public EventProgramBuilder WithSchedule(DateTimeOffset startsAt, DateTimeOffset endsAt)
    {
        _startsAt = startsAt;
        _endsAt = endsAt;
        return this;
    }

    /// <summary>
    /// Etkinliğin düzenleneceği mekanı ayarlar.
    /// </summary>
    public EventProgramBuilder AtVenue(string venue)
    {
        _venue = venue;
        return this;
    }

    /// <summary>
    /// Programa bir konuşmacı ekler.
    /// </summary>
    public EventProgramBuilder AddSpeaker(string speaker)
    {
        _speakers.Add(speaker);
        return this;
    }

    /// <summary>
    /// Etkinlik için canlı yayın adresi tanımlar.
    /// </summary>
    public EventProgramBuilder WithLiveStream(Uri liveStreamUrl)
    {
        _liveStreamUrl = liveStreamUrl;
        return this;
    }

    /// <summary>
    /// Tüm zorunlu alanları doğrular ve komutu üretir.
    /// </summary>
    public PublishEventProgramCommand Build()
    {
        if (string.IsNullOrWhiteSpace(_eventName))
        {
            throw new InvalidOperationException("Event name is required.");
        }

        if (_startsAt is null || _endsAt is null || _startsAt >= _endsAt)
        {
            throw new InvalidOperationException("A valid schedule is required.");
        }

        if (string.IsNullOrWhiteSpace(_venue))
        {
            throw new InvalidOperationException("Venue is required.");
        }

        return new PublishEventProgramCommand(
            _eventName,
            _startsAt.Value,
            _endsAt.Value,
            _venue,
            _speakers.AsReadOnly(),
            _liveStreamUrl);
    }
}

/// <summary>
/// Yayınlanmaya hazır etkinlik programı komutunu temsil eder.
/// </summary>
public sealed record PublishEventProgramCommand(
    string EventName,
    DateTimeOffset StartsAt,
    DateTimeOffset EndsAt,
    string Venue,
    IReadOnlyCollection<string> Speakers,
    Uri? LiveStreamUrl);
```

## 7. Ne Zaman Kullanılır?

- Nesne üretimi 6-7 adımı geçiyor ve okunabilirlik hızla düşüyorsa
- Zorunlu/opsiyonel alan ayrımı belirgin şekilde yönetilmek isteniyorsa
- `Build()` aşamasında merkezi doğrulama ile geçersiz nesneler engellenmek isteniyorsa
- Aynı ürünün farklı varyantları (ör. canlı yayınlı / yayınsız program) tekrar etmeden üretilecekse

## 8. Ne Zaman Kullanılmamalıdır?

- Model çok basitse ve tek constructor veya object initializer yeterliyse
- Ek bir builder sınıfı, çözdüğünden daha fazla karmaşıklık getiriyorsa
- Varyasyon ihtiyacı yoksa ve doğrulama yükü minimalse

## 9. Avantajlar

- Oluşturma akışını okunur ve niyet odaklı hale getirir.
- Geçersiz nesne üretimini `Build()` noktasında azaltır.
- Karmaşık kurulum mantığını tek bir yerde toplar.
- Yeni varyasyonlar eklendiğinde mevcut çağrıları daha az etkiler.
