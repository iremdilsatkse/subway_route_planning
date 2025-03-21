# Metro Rota Planlama Sistemi

Bu proje, bir metro ağındaki istasyonlar arasındaki en hızlı ve en az aktarmalı rotaları bulmayı amaçlayan bir sistemdir. Kullanıcılar, başlangıç ve hedef istasyonlarını seçerek, metro hatları arasında en uygun rotayı belirleyebilirler.

## Kullanılan Teknolojiler ve Kütüphaneler

- **Python 3**: Proje Python 3 ile geliştirilmiştir.
- **heapq**: A* algoritmasında kullanılan öncelikli kuyruğun yönetilmesi için kullanılır.
- **collections**: `deque` ve `defaultdict` veri yapıları için kullanılmıştır.
- **typing**: Kodun daha anlaşılır ve tür kontrolü yapılabilmesi için kullanılmıştır.

## Algoritmaların Çalışma Mantığı

### BFS (Breadth-First Search)

- BFS, genişlik öncelikli arama algoritmasıdır ve her seviyedeki düğümlerin sırayla incelenmesini sağlar.
- Bu algoritma, **en az aktarmalı rota**yı bulmak için kullanılır. Başlangıç istasyonundan hedef istasyonuna ulaşana kadar istasyonlar, bir kuyruk (queue) kullanılarak sırasıyla ziyaret edilir.
- BFS, aktarma sayısını minimize etmek için, her istasyonun bağlı olduğu istasyonlara geçerken, hedefe en kısa yoldan ulaşmayı sağlar.

### A* (A-star)

- A* algoritması, **en hızlı rota**yı bulmak için kullanılır. Bu algoritma, her bir istasyonu bir öncelik sırasına göre değerlendirir. 
- A* algoritmasında, her geçilen istasyonun toplam süresi (geçiş süresi) ve tahmin edilen hedefe olan mesafesi (heuristic) dikkate alınır.
- A* algoritması, BFS'e göre daha hızlı sonuçlar verebilir çünkü her adımda, en kısa yola ulaşmaya çalışan istasyonlar daha öncelikli değerlendirilir.

### Neden Bu Algoritmalar Kullanıldı?

- **BFS**: Metro ağında aktarmaların sayısının minimize edilmesi hedeflendiğinden, BFS algoritması doğrudan çözüm sunmaktadır.
- **A***: Hızlı bir rota bulmak gerektiğinde, geçiş sürelerini hesaba katan A* algoritması, daha optimal sonuçlar verir ve genel kullanıcı deneyimini iyileştirir.

### Projeyi Geliştirme Fikirleri

- **Dinamik Güncelleme**: Gerçek zamanlı olarak metro hattındaki durakları ve bağlantıları güncelleyerek kullanıcıya daha doğru bilgiler sunulabilir.
- **Farklı Ulaşım Araçları Entegrasyonu**: Metro hattının yanı sıra otobüs, tramvay gibi diğer ulaşım araçları da sisteme entegre edilebilir.
