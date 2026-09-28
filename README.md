# 🏛️ Enterprise Applicant Tracking & Interview Management System (ATS)

![.NET Core](https://img.shields.io/badge/.NET%20Core-ASP.NET%20MVC-512BD4?style=for-the-badge&logo=dotnet)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Advanced%20Relational%20DB-4169E1?style=for-the-badge&logo=postgresql)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6%20%2B%20Fetch%20API-F7DF1E?style=for-the-badge&logo=javascript)
![Security](https://img.shields.io/badge/Security-BCrypt%20%7C%20Auth-success?style=for-the-badge&logo=springsecurity)

> **⚠️ GİZLİLİK VE NDA BİLDİRİMİ (CONFIDENTIALITY NOTICE)**
> Bu sistem, bir kamu kurumu bünyesinde kurumsal işe alım ve mülakat süreçlerini yönetmek amacıyla kapalı devre (On-Premise) çalışacak şekilde tasarlanıp geliştirilmiştir. Kurumun siber güvenlik politikaları ve imzalanan Gizlilik Sözleşmesi (NDA) gereğince projenin kaynak kodları, sunucu konfigürasyonları ve kuruma özel iş kuralları (Business Logic) bu repoda paylaşılamamaktadır. Bu doküman, projenin sistem mimarisini, veri tabanı tasarımını ve çözülen mühendislik problemlerini sektörel standartlarda sergilemek amacıyla teknik bir referans olarak hazırlanmıştır.

## 📌 Proje Özeti
Aday Takip ve Mülakat Yönetim Sistemi (ATS), binlerce adayın başvuru süreçlerini, özgeçmiş verilerini ve İnsan Kaynakları mülakat komisyonu değerlendirmelerini tek bir çatı altında toplayan web tabanlı bir otomasyondur. Sistem, "Aday Paneli" ve "Komisyon/İK Paneli" olmak üzere role dayalı (Role-Based) iki ana modülden oluşmaktadır.

## ⚙️ Sistem Mimarisi ve Teknoloji Yığını (Tech Stack)

### Backend (Sunucu Tarafı)
* **Framework:** C# / ASP.NET Core MVC
* **ORM:** Entity Framework Core (Code-First Yaklaşımı)
* **Veritabanı:** PostgreSQL
* **Güvenlik:** BCrypt.Net-Next (Şifre Kriptolama), Cookie-based Authentication

### Frontend (İstemci Tarafı)
* **Arayüz:** HTML5, CSS3, Bootstrap 5 (Responsive Design)
* **Asenkron İletişim:** JavaScript ES6, Fetch API (Sayfa yenilemesiz veri akışı)
* **Veri İşleme ve UI Kütüphaneleri:** 
  * `PDF.js`: İstemci tarafında CV belgelerinden metin ayrıştırma (Client-side parsing)
  * `Tom Select`: Çoklu veri seçimi ve dinamik dropdown yönetimi
  * `SweetAlert2`: Asenkron işlem geri bildirimleri

---

## 🚀 Çözülen Mühendislik Problemleri ve Optimizasyonlar

Basit CRUD (Ekle/Oku/Güncelle/Sil) işlemlerinin ötesine geçilerek, kurumsal bir projede karşılaşılan darboğazlar (bottlenecks) şu mimari yaklaşımlarla çözülmüştür:

**1. Büyük Veri Kümelerinde N+1 Sorgu Probleminin Çözümü**
* Komisyon panelinde adayların eğitim, deneyim ve sertifika gibi çok-a-çok (Many-to-Many) ilişkili verileri çekilirken oluşan N+1 sorgu problemi, Entity Framework `Include()` ve `ThenInclude()` yapılarıyla çözülerek tek bir SQL JOIN işlemine indirgenmiştir.
* Sadece okuma (Read-only) yapılan veri listelemelerinde bellek maliyetini düşürmek için `.AsNoTracking()` metodu kullanılmıştır.

**2. Ertelenmiş Çalıştırma (Deferred Execution) ile Filtreleme**
* İK yetkililerinin binlerce aday arasında yaptığı çoklu filtreleme işlemleri bellekte (RAM) değil, `IQueryable<T>` arayüzü sayesinde doğrudan PostgreSQL motoru üzerinde SQL WHERE şartlarına dönüştürülerek çalıştırılmıştır.

**3. Sunucu Maliyetlerini Düşüren İstemci Taraflı Veri Ayrıştırma**
* Adayların yüklediği PDF formatındaki özgeçmişlerin sunucuyu yorarak parse edilmesi yerine, `PDF.js` kullanılarak işlem kullanıcının tarayıcısında (Client-side) gerçekleştirilmiş ve elde edilen veriler başvuru formuna otomatik doldurulmuştur (Auto-fill).

**4. Asenkron (AJAX) İşlem Yönetimi ve Veri Tutarlılığı**
* İlan başvuruları ve mülakat puanlamaları sırasında kullanıcı deneyimini bölmemek adına `Fetch API` kullanılarak asenkron yapılar kurulmuştur.
* Eksik veya hatalı verilerin veritabanına yansımasını (Garbage Data) engellemek için Backend Controller seviyesinde `Guard Clauses` (Erken Çıkış Mekanizmaları) ve Frontend tarafında anlık validasyonlar entegre edilmiştir.

---

## 🗄️ Veritabanı Şeması (Kavramsal Model)
Proje, 3. Normal Form (3NF) kurallarına uygun olarak PostgreSQL üzerinde ilişkisel bir mimariyle tasarlanmıştır. Temel tablolar ve ilişkiler:
* `Users` (Aday ve Yetkili giriş bilgileri, BCrypt hashleri)
* `Candidates` (Demografik bilgiler, iletişim verileri)
* `Educations`, `Experiences`, `Certificates` (Adaylara 1:N One-to-Many ilişkili detay tabloları)
* `JobPostings` (Açık iş ilanları ve gereksinimleri)
* `Applications` (Adaylar ve İlanlar arasındaki N:N ilişkileri ve mülakat durumlarını tutan ara tablo)

---

## 👨‍💻 Geliştiriciler (Contributors)
Bu proje, aşağıdaki geliştiriciler tarafından omuz omuza analiz edilmiş, tasarlanmış ve kodlanmıştır:

* **Harun Melih Karakaş** - [LinkedIn](https://www.linkedin.com/in/harun-melih-karaka%C5%9F-ab1747332/) | [GitHub](https://github.com/hmkaraks)
* **Furkan Emirvelioğlu** - [LinkedIn](https://www.linkedin.com/in/furkan-emirvelio%C4%9Flu-b4a8a136a/) | [GitHub](https://github.com/lfurkan06)
