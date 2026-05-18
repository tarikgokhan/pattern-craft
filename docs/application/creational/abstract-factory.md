# Abstract Factory Tasarım Deseni
![Pattern Craft - Design Patterns Overview](/assets/images/abstract-factory.png)
> **Kategori:** Application Design Patterns / Creational Patterns  
> **Platform:** .NET / C#  
> **Seviye:** Orta  
> **Amaç:** Birbiriyle ilişkili nesne ailelerini, somut sınıflara doğrudan bağımlı olmadan üretmek.

---

## 1. Kısa Tanım

**Abstract Factory**, birbiriyle ilişkili veya birlikte çalışması gereken nesne ailelerini üretmek için kullanılan bir tasarım desenidir.

Bu desenin temel amacı, uygulamanın ihtiyaç duyduğu nesneleri doğrudan `new` anahtar kelimesiyle oluşturmak yerine, bu nesnelerin üretimini soyut bir fabrika arayüzü üzerinden yönetmektir.

Basit ifadeyle:

```text
Client Kod
   ↓
Abstract Factory
   ↓
Birbiriyle uyumlu nesne ailesi
```

---

## 2. Ne Zaman Kullanılır?

Abstract Factory aşağıdaki durumlarda tercih edilir:

- Birbiriyle ilişkili nesne grupları birlikte üretilmeliyse
- Uygulama, somut sınıflara doğrudan bağımlı olmamalıysa
- Farklı ortam, tema, kanal, sağlayıcı veya platformlara göre nesne ailesi değişiyorsa
- Aynı iş akışı farklı nesne setleriyle çalıştırılmak isteniyorsa
- Factory Method tek başına yetersiz kalıyor ve birden fazla ilgili nesnenin birlikte üretilmesi gerekiyorsa

---

## 3. Temel Problem

Bir uygulamada farklı çıktı türleri için uyumlu bileşenler üretmek istediğimizi düşünelim.

Örneğin bir raporlama altyapısında iki farklı çıktı ailesi olsun:

```text
PDF Output Family
   ├── PdfHeaderRenderer
   ├── PdfBodyRenderer
   └── PdfFooterRenderer

HTML Output Family
   ├── HtmlHeaderRenderer
   ├── HtmlBodyRenderer
   └── HtmlFooterRenderer
```

Client kodun şunu bilmesini istemeyiz:

```csharp
var header = new PdfHeaderRenderer();
var body = new PdfBodyRenderer();
var footer = new PdfFooterRenderer();
```

Çünkü bu durumda client kod doğrudan somut sınıflara bağımlı olur.

Daha doğru yaklaşım:

```csharp
var header = factory.CreateHeaderRenderer();
var body = factory.CreateBodyRenderer();
var footer = factory.CreateFooterRenderer();
```

Bu sayede client kod sadece soyut fabrika ile çalışır.

---

## 4. Yapısal Görünüm

```text
IReportRendererFactory
   ├── CreateHeaderRenderer()
   ├── CreateBodyRenderer()
   └── CreateFooterRenderer()

PdfReportRendererFactory
   ├── PdfHeaderRenderer
   ├── PdfBodyRenderer
   └── PdfFooterRenderer

HtmlReportRendererFactory
   ├── HtmlHeaderRenderer
   ├── HtmlBodyRenderer
   └── HtmlFooterRenderer
```

---

## 5. .NET Örneği

Bu örnekte amaç, farklı rapor çıktı formatları için uyumlu renderer nesneleri üretmektir.

Kullanılan senaryo tamamen teknik ve domain bağımsızdır.

---

## 6. Product Interface Tanımları

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Rapor başlık bölümünü oluşturan renderer sözleşmesini temsil eder.
/// </summary>
public interface IHeaderRenderer
{
    /// <summary>
    /// Başlık içeriğini üretir.
    /// </summary>
    /// <param name="title">Rapor başlığı.</param>
    /// <returns>Üretilen başlık içeriği.</returns>
    string Render(string title);
}

/// <summary>
/// Rapor gövde bölümünü oluşturan renderer sözleşmesini temsil eder.
/// </summary>
public interface IBodyRenderer
{
    /// <summary>
    /// Gövde içeriğini üretir.
    /// </summary>
    /// <param name="content">Rapor ana içeriği.</param>
    /// <returns>Üretilen gövde içeriği.</returns>
    string Render(string content);
}

/// <summary>
/// Rapor alt bilgi bölümünü oluşturan renderer sözleşmesini temsil eder.
/// </summary>
public interface IFooterRenderer
{
    /// <summary>
    /// Alt bilgi içeriğini üretir.
    /// </summary>
    /// <param name="text">Alt bilgi metni.</param>
    /// <returns>Üretilen alt bilgi içeriği.</returns>
    string Render(string text);
}
```

---

## 7. Abstract Factory Interface

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Bir rapor formatına ait uyumlu renderer nesnelerini üreten abstract factory sözleşmesini temsil eder.
/// </summary>
public interface IReportRendererFactory
{
    /// <summary>
    /// Rapor başlık renderer nesnesini oluşturur.
    /// </summary>
    /// <returns>Başlık renderer nesnesi.</returns>
    IHeaderRenderer CreateHeaderRenderer();

    /// <summary>
    /// Rapor gövde renderer nesnesini oluşturur.
    /// </summary>
    /// <returns>Gövde renderer nesnesi.</returns>
    IBodyRenderer CreateBodyRenderer();

    /// <summary>
    /// Rapor alt bilgi renderer nesnesini oluşturur.
    /// </summary>
    /// <returns>Alt bilgi renderer nesnesi.</returns>
    IFooterRenderer CreateFooterRenderer();
}
```

---

## 8. PDF Ürün Ailesi

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// PDF formatı için başlık renderer implementasyonudur.
/// </summary>
public sealed class PdfHeaderRenderer : IHeaderRenderer
{
    /// <inheritdoc />
    public string Render(string title)
    {
        return $"[PDF HEADER] {title}";
    }
}

/// <summary>
/// PDF formatı için gövde renderer implementasyonudur.
/// </summary>
public sealed class PdfBodyRenderer : IBodyRenderer
{
    /// <inheritdoc />
    public string Render(string content)
    {
        return $"[PDF BODY] {content}";
    }
}

/// <summary>
/// PDF formatı için alt bilgi renderer implementasyonudur.
/// </summary>
public sealed class PdfFooterRenderer : IFooterRenderer
{
    /// <inheritdoc />
    public string Render(string text)
    {
        return $"[PDF FOOTER] {text}";
    }
}
```

---

## 9. HTML Ürün Ailesi

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// HTML formatı için başlık renderer implementasyonudur.
/// </summary>
public sealed class HtmlHeaderRenderer : IHeaderRenderer
{
    /// <inheritdoc />
    public string Render(string title)
    {
        return $"<header><h1>{title}</h1></header>";
    }
}

/// <summary>
/// HTML formatı için gövde renderer implementasyonudur.
/// </summary>
public sealed class HtmlBodyRenderer : IBodyRenderer
{
    /// <inheritdoc />
    public string Render(string content)
    {
        return $"<main><p>{content}</p></main>";
    }
}

/// <summary>
/// HTML formatı için alt bilgi renderer implementasyonudur.
/// </summary>
public sealed class HtmlFooterRenderer : IFooterRenderer
{
    /// <inheritdoc />
    public string Render(string text)
    {
        return $"<footer>{text}</footer>";
    }
}
```

---

## 10. Concrete Factory Implementasyonları

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// PDF formatı için uyumlu renderer ailesini oluşturan factory implementasyonudur.
/// </summary>
public sealed class PdfReportRendererFactory : IReportRendererFactory
{
    /// <inheritdoc />
    public IHeaderRenderer CreateHeaderRenderer()
    {
        return new PdfHeaderRenderer();
    }

    /// <inheritdoc />
    public IBodyRenderer CreateBodyRenderer()
    {
        return new PdfBodyRenderer();
    }

    /// <inheritdoc />
    public IFooterRenderer CreateFooterRenderer()
    {
        return new PdfFooterRenderer();
    }
}

/// <summary>
/// HTML formatı için uyumlu renderer ailesini oluşturan factory implementasyonudur.
/// </summary>
public sealed class HtmlReportRendererFactory : IReportRendererFactory
{
    /// <inheritdoc />
    public IHeaderRenderer CreateHeaderRenderer()
    {
        return new HtmlHeaderRenderer();
    }

    /// <inheritdoc />
    public IBodyRenderer CreateBodyRenderer()
    {
        return new HtmlBodyRenderer();
    }

    /// <inheritdoc />
    public IFooterRenderer CreateFooterRenderer()
    {
        return new HtmlFooterRenderer();
    }
}
```

---

## 11. Client Service Kullanımı

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Rapor oluşturma akışını yöneten uygulama servisidir.
/// </summary>
public sealed class ReportComposer
{
    private readonly IReportRendererFactory _rendererFactory;

    /// <summary>
    /// ReportComposer sınıfının yeni bir örneğini oluşturur.
    /// </summary>
    /// <param name="rendererFactory">Rapor renderer nesnelerini üreten factory.</param>
    public ReportComposer(IReportRendererFactory rendererFactory)
    {
        _rendererFactory = rendererFactory;
    }

    /// <summary>
    /// Başlık, gövde ve alt bilgi bölümlerinden oluşan rapor çıktısını üretir.
    /// </summary>
    /// <param name="title">Rapor başlığı.</param>
    /// <param name="content">Rapor içeriği.</param>
    /// <param name="footer">Rapor alt bilgisi.</param>
    /// <returns>Üretilen rapor çıktısı.</returns>
    public string Compose(string title, string content, string footer)
    {
        var headerRenderer = _rendererFactory.CreateHeaderRenderer();
        var bodyRenderer = _rendererFactory.CreateBodyRenderer();
        var footerRenderer = _rendererFactory.CreateFooterRenderer();

        return string.Join(
            Environment.NewLine,
            headerRenderer.Render(title),
            bodyRenderer.Render(content),
            footerRenderer.Render(footer));
    }
}
```

---

## 12. Program.cs Kullanım Örneği

```csharp
using PatternCraft.ApplicationPatterns.AbstractFactory;

IReportRendererFactory factory = new HtmlReportRendererFactory();

var composer = new ReportComposer(factory);

var output = composer.Compose(
    title: "System Health Report",
    content: "All services are running within expected thresholds.",
    footer: "Generated by PatternCraft sample.");

Console.WriteLine(output);
```

Örnek çıktı:

```html
<header><h1>System Health Report</h1></header>
<main><p>All services are running within expected thresholds.</p></main>
<footer>Generated by PatternCraft sample.</footer>
```

PDF ailesi kullanılmak istenirse sadece factory değiştirilir:

```csharp
IReportRendererFactory factory = new PdfReportRendererFactory();
```

Client kodun geri kalanı değişmez.

---

## 13. Dependency Injection ile Kullanım

Gerçek .NET uygulamalarında factory seçimi configuration üzerinden yapılabilir.

```csharp
builder.Services.AddScoped<IReportRendererFactory, HtmlReportRendererFactory>();
builder.Services.AddScoped<ReportComposer>();
```

Daha dinamik seçim gerekiyorsa aşağıdaki gibi bir provider kullanılabilir:

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Rapor formatına göre uygun renderer factory nesnesini seçer.
/// </summary>
public sealed class ReportRendererFactoryProvider
{
    /// <summary>
    /// Belirtilen formata göre uygun factory örneğini döndürür.
    /// </summary>
    /// <param name="format">Rapor çıktı formatı.</param>
    /// <returns>Seçilen renderer factory nesnesi.</returns>
    /// <exception cref="NotSupportedException">Desteklenmeyen formatlar için fırlatılır.</exception>
    public IReportRendererFactory GetFactory(string format)
    {
        return format.ToLowerInvariant() switch
        {
            "pdf" => new PdfReportRendererFactory(),
            "html" => new HtmlReportRendererFactory(),
            _ => throw new NotSupportedException($"The report format '{format}' is not supported.")
        };
    }
}
```

---

## 14. Abstract Factory ile Factory Method Arasındaki Fark

| Konu | Factory Method | Abstract Factory |
|---|---|---|
| Odak | Tek bir nesne üretimi | Birbiriyle ilişkili nesne ailesi üretimi |
| Karmaşıklık | Daha basit | Daha kapsamlı |
| Kullanım | Tek ürün değişiyorsa | Birden fazla uyumlu ürün birlikte değişiyorsa |
| Örnek | `CreateRenderer()` | `CreateHeaderRenderer()`, `CreateBodyRenderer()`, `CreateFooterRenderer()` |

---

## 15. Avantajlar

- Client kod somut sınıflara bağımlı olmaz.
- Birbiriyle uyumlu nesne aileleri birlikte üretilir.
- Yeni ürün ailesi eklemek kolaylaşır.
- Test edilebilirlik artar.
- Open/Closed Principle desteklenir.
- Nesne oluşturma karmaşıklığı merkezi hale gelir.

---

## 16. Dezavantajlar

- Basit senaryolarda gereksiz soyutlama oluşturabilir.
- Yeni bir product interface eklenirse tüm factory implementasyonlarının güncellenmesi gerekir.
- Sınıf sayısı artar.
- Yanlış kullanıldığında kodu gereğinden fazla karmaşık hale getirebilir.

---

## 17. .NET Projelerinde Kullanım Alanları

Abstract Factory aşağıdaki teknik alanlarda kullanılabilir:

- Farklı çıktı formatları için renderer ailesi üretimi
- Farklı UI tema bileşenleri üretimi
- Farklı storage provider aileleri üretimi
- Farklı notification channel aileleri üretimi
- Farklı dosya işleme stratejilerine ait bileşen setleri üretimi
- Farklı ortamlar için uyumlu servis aileleri üretimi
- Test ve production ortamlarında farklı ama uyumlu nesne setleri kullanımı

---

## 18. Kullanım Kararı

Abstract Factory aşağıdaki soruya “evet” cevabı verildiğinde güçlü bir adaydır:

> Aynı bağlam içinde birlikte çalışması gereken birden fazla nesneyi, farklı aileler halinde üretmem gerekiyor mu?

Eğer sadece tek nesne üretilecekse çoğu zaman **Factory Method** yeterlidir.

Eğer birlikte değişen ve uyumlu kalması gereken birden fazla nesne varsa **Abstract Factory** daha doğru bir çözümdür.

---

## 19. Mini Kontrol Listesi

| Soru | Cevap Evetse |
|---|---|
| Birden fazla ilişkili nesne birlikte mi üretiliyor? | Abstract Factory düşünülebilir. |
| Client kod somut sınıfları bilmemeli mi? | Abstract Factory uygundur. |
| Aynı iş akışı farklı nesne aileleriyle mi çalışıyor? | Abstract Factory uygundur. |
| Yeni aileler eklenecek ama client kod değişmemeli mi? | Abstract Factory uygundur. |
| Sadece tek bir nesne mi üretiliyor? | Factory Method daha sade olabilir. |

---

## 20. Özet

Abstract Factory, birbiriyle ilişkili nesne ailelerinin üretimini soyutlayarak uygulamanın somut sınıflara bağımlılığını azaltır.

Bu desen özellikle .NET projelerinde genişletilebilir, test edilebilir ve sürdürülebilir yapı kurmak için faydalıdır. Ancak yalnızca ihtiyaç olduğunda kullanılmalı, basit nesne üretimleri için gereksiz karmaşıklık yaratmamalıdır.

