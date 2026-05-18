# Abstract Factory Tasarım Deseni

**Kategori:** Application Design Patterns / Creational Patterns | **Platform:** .NET / C# | **Seviye:** Orta

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

Hayal edin ki e-ticaret platformunuz hem mobil hem de web kanallarını destekliyor. Her kanal için benzersiz bir ürün sunumu, ödeme kartı gösterimi ve bildirim bileşeni gerekiyor. 

Mobil için:
- Compact UIButton, Swipeable Payment Card, Push Notification
  
Web için:
- Full-width UIButton, Interactive Payment Card, Email Notification

Naif bir yaklaşım, her bileşeni doğrudan örneğini oluşturmak olabilir:

```csharp
// ❌ Kötü: Kanal bağımlılığı client kodunda
var button = new MobileUIButton();
var card = new MobilePaymentCard();
var notification = new MobilePushNotification();
```

Bu şekilde client kod, tüm somut sınıfları bilmek zorunda kalır. Yeni bir kanal eklenmek istenirse hangi bileşenlerin gerektiği, bunların nasıl birleştirileceği belirsiz hale gelir.

✅ Daha iyi bir yaklaşım:

```csharp
IComponentFactory factory = new MobileComponentFactory();
var button = factory.CreateButton();
var card = factory.CreatePaymentCard();
var notification = factory.CreateNotification();
```

Artık client kod hiçbir somut sınıf bilmez, sadece soyut factory ile çalışır. Kanal değişikliği yapılmak istenirse, factory değiştirilir — uygulamanın geri kalanı dokunulmaz.

---

## 4. Yapısal Görünüm

```
IComponentFactory (Abstract Factory)
   ├── CreateButton(): IButton
   ├── CreatePaymentCard(): IPaymentCard
   └── CreateNotification(): INotification

MobileComponentFactory         WebComponentFactory
   ├── MobileButton              ├── WebButton
   ├── MobilePaymentCard         ├── WebPaymentCard
   └── MobilePushNotification    └── WebEmailNotification
```

---

## 5. .NET Örneği

Bu örnekte amaç, farklı kanallar (mobile, web) için uyumlu UI bileşenleri üretmektir. Her kanal kendi tasarım prensipleri, animasyon stili ve kullanıcı etkileşim modeline sahiptir. Ama tüm kanallar aynı özellikleri karşılamak zorundadır.

---

## 6. Product Interface Tanımları

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Uygulama arayüzünde kullanılan buton bileşeninin sözleşmesini tanımlar.
/// </summary>
public interface IButton
{
    /// <summary>
    /// Butonu ekranda görüntülemek için gereken HTML veya render kodunu üretir.
    /// </summary>
    /// <param name="label">Butonun üzerinde gösterilecek metin.</param>
    /// <returns>Render edilmiş buton çıktısı.</returns>
    string Render(string label);
}

/// <summary>
/// Ödeme kartı göstergesi bileşeninin sözleşmesini tanımlar.
/// </summary>
public interface IPaymentCard
{
    /// <summary>
    /// Ödeme kartını güvenli bir şekilde göstermek için gereken formatı üretir.
    /// </summary>
    /// <param name="cardNumber">Gizli kart numarası (son 4 hanesi gösterilir).</param>
    /// <returns>Gösterim formatı.</returns>
    string Display(string cardNumber);
}

/// <summary>
/// Bildirim bileşeninin sözleşmesini tanımlar.
/// </summary>
public interface INotification
{
    /// <summary>
    /// Kullanıcıya bildirim göndermek için uygun kanalı ve formatı hazırlar.
    /// </summary>
    /// <param name="message">Bildirim mesajı.</param>
    /// <returns>Gönderim komutu veya gösterim formatı.</returns>
    string Send(string message);
}
```

---

## 7. Abstract Factory Interface

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Belirtilen kanal için uyumlu UI bileşeni ailesini üreten fabrika sözleşmesini tanımlar.
/// </summary>
public interface IComponentFactory
{
    /// <summary>
    /// Kanal uyumlu buton bileşenini oluşturur.
    /// </summary>
    /// <returns>Kanal özel buton implementasyonu.</returns>
    IButton CreateButton();

    /// <summary>
    /// Kanal uyumlu ödeme kartı göstergesi oluşturur.
    /// </summary>
    /// <returns>Kanal özel ödeme kartı implementasyonu.</returns>
    IPaymentCard CreatePaymentCard();

    /// <summary>
    /// Kanal uyumlu bildirim sistemi oluşturur.
    /// </summary>
    /// <returns>Kanal özel bildirim implementasyonu.</returns>
    INotification CreateNotification();
}
```

---

## 8. Mobil Platform Ürün Ailesi

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Mobil platform için optimize edilmiş buton bileşenidir.
/// Compact tasarım, dokunsal geri bildirim uyumlu.
/// </summary>
public sealed class MobileButton : IButton
{
    /// <inheritdoc />
    public string Render(string label)
    {
        return $"<button class=\"mobile-btn compact\" style=\"padding: 8px 16px; font-size: 12px;\">{label}</button>";
    }
}

/// <summary>
/// Mobil platformda güvenli ödeme kartı görüntüleme (push notification tabanlı).
/// </summary>
public sealed class MobilePaymentCard : IPaymentCard
{
    /// <inheritdoc />
    public string Display(string cardNumber)
    {
        var masked = cardNumber.Length > 4 ? "•••• •••• •••• " + cardNumber.Substring(cardNumber.Length - 4) : cardNumber;
        return $"<div class=\"mobile-card-display\">Kart: {masked}</div>";
    }
}

/// <summary>
/// Mobil platform için push notification tabanlı bildirim sistemi.
/// </summary>
public sealed class MobilePushNotification : INotification
{
    /// <inheritdoc />
    public string Send(string message)
    {
        return $"push_notification: title=Bildirim, body={message}, vibrate=true";
    }
}
```

---

## 9. Web Platform Ürün Ailesi

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Web platformu için tam genişlik buton bileşenidir.
/// Sürükleme, açılır menü özellikleri destekler.
/// </summary>
public sealed class WebButton : IButton
{
    /// <inheritdoc />
    public string Render(string label)
    {
        return $"<button class=\"web-btn primary\" style=\"width: 100%; padding: 12px 24px; font-size: 14px;\">{label}</button>";
    }
}

/// <summary>
/// Web platformunda etkileşimli ödeme kartı görüntüleme (hover efektleri, tooltip).
/// </summary>
public sealed class WebPaymentCard : IPaymentCard
{
    /// <inheritdoc />
    public string Display(string cardNumber)
    {
        var masked = cardNumber.Length > 4 ? "•••• •••• •••• " + cardNumber.Substring(cardNumber.Length - 4) : cardNumber;
        return $"<div class=\"web-card-display\" onmouseover=\"this.style.transform='scale(1.05)'\" title=\"Ödeme kartınız\">{masked}</div>";
    }
}

/// <summary>
/// Web platformu için e-posta ve toast bildirim sistemi.
/// </summary>
public sealed class WebEmailNotification : INotification
{
    /// <inheritdoc />
    public string Send(string message)
    {
        return $"email: subject=Haber, body={message}; toast: position=bottom-right, duration=5000";
    }
}
```

---

## 10. Concrete Factory Implementasyonları

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Mobil platform için uyumlu UI bileşen ailesini üreten fabrikadır.
/// Mobile-first tasarımı destekler.
/// </summary>
public sealed class MobileComponentFactory : IComponentFactory
{
    /// <inheritdoc />
    public IButton CreateButton()
    {
        return new MobileButton();
    }

    /// <inheritdoc />
    public IPaymentCard CreatePaymentCard()
    {
        return new MobilePaymentCard();
    }

    /// <inheritdoc />
    public INotification CreateNotification()
    {
        return new MobilePushNotification();
    }
}

/// <summary>
/// Web platformu için uyumlu UI bileşen ailesini üreten fabrikadır.
/// Desktop-first tasarımı destekler.
/// </summary>
public sealed class WebComponentFactory : IComponentFactory
{
    /// <inheritdoc />
    public IButton CreateButton()
    {
        return new WebButton();
    }

    /// <inheritdoc />
    public IPaymentCard CreatePaymentCard()
    {
        return new WebPaymentCard();
    }

    /// <inheritdoc />
    public INotification CreateNotification()
    {
        return new WebEmailNotification();
    }
}
```

---

## 11. Client Service Kullanımı

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Kullanıcı arayüzü bileşenlerini yönetir ve çoklu kanal desteği sağlar.
/// Factory pattern sayesinde kanal detaylarından izole edilmiştir.
/// </summary>
public sealed class UserInterfaceManager
{
    private readonly IComponentFactory _componentFactory;

    /// <summary>
    /// UserInterfaceManager sınıfının yeni bir örneğini oluşturur.
    /// </summary>
    /// <param name="componentFactory">Kanal uyumlu bileşenleri üreten fabrika.</param>
    public UserInterfaceManager(IComponentFactory componentFactory)
    {
        _componentFactory = componentFactory;
    }

    /// <summary>
    /// Satın alma işlemi için gerekli tüm UI bileşenlerini render eder.
    /// </summary>
    /// <param name="buttonText">Butonun üzerindeki metin.</param>
    /// <param name="cardNumber">Ödeme kartı numarası.</param>
    /// <param name="feedbackMessage">Kullanıcı geri bildirimi.</param>
    /// <returns>Tam render edilmiş checkout ekranı.</returns>
    public string RenderCheckout(string buttonText, string cardNumber, string feedbackMessage)
    {
        var button = _componentFactory.CreateButton();
        var paymentCard = _componentFactory.CreatePaymentCard();
        var notification = _componentFactory.CreateNotification();

        return string.Join(
            Environment.NewLine,
            "<div class=\"checkout-container\">",
            paymentCard.Display(cardNumber),
            button.Render(buttonText),
            $"<p class=\"feedback\">{feedbackMessage}</p>",
            $"<!-- Notification: {notification.Send(feedbackMessage)} -->",
            "</div>");
    }
}
```

---

## 12. Program.cs Kullanım Örneği

```csharp
using PatternCraft.ApplicationPatterns.AbstractFactory;

// Kullanıcı aracı mobil cihazdan giriş yapıyor
IComponentFactory mobileFactory = new MobileComponentFactory();
var mobileUI = new UserInterfaceManager(mobileFactory);

var mobileCheckout = mobileUI.RenderCheckout(
    buttonText: "Öde",
    cardNumber: "4532123456789012",
    feedbackMessage: "Ödeme işlemi başlamış.");

Console.WriteLine("=== MOBILE CHECKOUT ===");
Console.WriteLine(mobileCheckout);

// Aynı işlemi web tarayıcıdan yapan kullanıcı
IComponentFactory webFactory = new WebComponentFactory();
var webUI = new UserInterfaceManager(webFactory);

var webCheckout = webUI.RenderCheckout(
    buttonText: "Satın Almayı Onayla",
    cardNumber: "4532123456789012",
    feedbackMessage: "Lütfen ödeme detaylarınızı onaylayın.");

Console.WriteLine("\n=== WEB CHECKOUT ===");
Console.WriteLine(webCheckout);
```

Çalıştırıldığında farklı formatlar görülür — ama client kod (UserInterfaceManager) aynıdır!

---

## 13. Dependency Injection ile Kullanım

Gerçek ASP.NET uygulamalarında kanal seçimi HTTP istek başlığından ve kullanıcı aracısından otomatik yapılır:

```csharp
builder.Services.AddScoped<UserInterfaceManager>();
builder.Services.AddScoped<IComponentFactory>(provider => 
{
    var httpContext = provider.GetRequiredService<IHttpContextAccessor>().HttpContext;
    
    var userAgent = httpContext?.Request.Headers["User-Agent"].ToString() ?? "";
    
    return userAgent.Contains("Mobile") || userAgent.Contains("Android") || userAgent.Contains("iPhone")
        ? new MobileComponentFactory()
        : new WebComponentFactory();
});
```

Daha yapılandırılabilir bir yaklaşım:

```csharp
namespace PatternCraft.ApplicationPatterns.AbstractFactory;

/// <summary>
/// Platform adına göre uygun factory seçerek bileşen üretimini sağlar.
/// </summary>
public sealed class ComponentFactoryProvider
{
    /// <summary>
    /// Belirtilen platform adına göre uygun factory örneğini döndürür.
    /// </summary>
    /// <param name="platform">Platform adı (mobile, web, tablet, vb.).</param>
    /// <returns>Seçilen component factory nesnesi.</returns>
    /// <exception cref="NotSupportedException">Desteklenmeyen platform için fırlatılır.</exception>
    public IComponentFactory GetFactory(string platform)
    {
        return platform.ToLowerInvariant() switch
        {
            "mobile" => new MobileComponentFactory(),
            "web" => new WebComponentFactory(),
            _ => throw new NotSupportedException($"Platform '{platform}' desteklenmiyor.")
        };
    }
}
```

---

## 14. Avantajlar ve Riskler

### Avantajlar
- **Somut Sınıf Bağımlılığı Azalır:** Client kod hiçbir concrete sınıfı bilmez, sadece arayüzlerle çalışır.
- **Uyumlu Ürün Aileleri:** Birbiriyle tutarlı olan nesneler birlikte üretilir, çatışma riski azalır.
- **Kolay Genişletme:** Yeni bir kanal/format eklemek için sadece yeni factory ve product sınıfları eklenir; mevcut kod değişmez.
- **Test Kolaylığı:** Mock factory ve product'ları easily oluşturulabilir.
- **Open/Closed Principle:** Kodu kapalı tutarak genişletebilirsiniz.

### Riskler ve Dikkat Edilecek Noktalar
- **Karmaşıklık:** Basit senaryo için gereksiz yere interface ve sınıf sayısı artar.
- **Yeni Product Eklenmesi:** Tüm factory implementasyonları güncellenmek zorunda kalır.
- **Over-Engineering:** Sadece abstract factory yapısı için sorunu zorlamak. Eğer gerçekten birden fazla ilişkili nesne yoksa, Factory Method bile yeterli olabilir.

---

## 15. Abstract Factory ile Factory Method Arasındaki Fark

| Konu | Factory Method | Abstract Factory |
|---|---|---|
| Odak | Tek bir nesne üretimi | Birbiriyle ilişkili nesne ailesi üretimi |
| Karmaşıklık | Daha basit | Daha kapsamlı |
| Kullanım | Tek ürün değişiyorsa | Birden fazla uyumlu ürün birlikte değişiyorsa |
| Örnek | `CreateButton()` | `CreateButton()`, `CreatePaymentCard()`, `CreateNotification()` |

---

## 16. .NET Projelerinde Kullanım Alanları

Abstract Factory aşağıdaki teknik alanlarda kullanılabilir:

- **Multi-Channel UI Bileşenleri:** Mobil, web, tablet gibi farklı kanallar için uyumlu bileşen aileleri
- **Tema Desteği:** Light/Dark tema, farklı UI tema aileleri
- **Database Provider Aileleri:** SQL Server, PostgreSQL, SQLite için uyumlu query/command setleri
- **Notification Kanalları:** Email, SMS, Push, In-App notification aileleri
- **Dosya İşleme Stratejileri:** PDF, Excel, CSV export için uyumlu writer aileleri
- **Ortam Spesifik Konfigürasyon:** Development, Staging, Production ortamları için farklı bileşen setleri

---

## 17. Test Edilebilirlik Notları

Abstract Factory aşağıdaki soruya “evet” cevabı verildiğinde güçlü bir adaydır:

> Aynı bağlam içinde birlikte çalışması gereken birden fazla nesneyi, farklı aileler halinde üretmem gerekiyor mu?

Eğer sadece tek nesne üretilecekse çoğu zaman **Factory Method** yeterlidir.

Eğer birlikte değişen ve uyumlu kalması gereken birden fazla nesne varsa **Abstract Factory** daha doğru bir çözümdür.

---

## 19. Özet

Abstract Factory, birbiriyle ilişkili nesne ailelerinin üretimini soyutlayarak uygulamanın somut sınıflara bağımlılığını azaltır.

Bu desen özellikle .NET projelerinde genişletilebilir, test edilebilir ve sürdürülebilir yapı kurmak için faydalıdır. Ancak yalnızca ihtiyaç olduğunda kullanılmalı, basit nesne üretimleri için gereksiz karmaşıklık yaratmamalıdır.

