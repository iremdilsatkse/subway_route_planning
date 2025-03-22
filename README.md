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

### Örnek Kullanım ve Test Sonuçları
**BFS**
'''
def en_az_aktarma_bul(self, baslangic_id: str, hedef_id: str) -> Optional[List[Istasyon]]:
        # Başlangıç ve hedef istasyonların istasyonlar içerisinde mevcut olup olmadığı kontrolü
        if baslangic_id not in self.istasyonlar or hedef_id not in self.istasyonlar:
            return None
        
        # Başlangıç ve hedef istasyonlar
        baslangic = self.istasyonlar[baslangic_id]
        hedef = self.istasyonlar[hedef_id]

        # BFS için kuyruk ve ziyaret edilen istasyonlar
        kuyruk = deque([(baslangic, [baslangic])])
        ziyaret_edildi = set() 

        while kuyruk:
            # Kuyruğun başındaki istasyon ve yola ait öğe çıkarılır
            istasyon, yol = kuyruk.popleft() 

            # Eğer hedef istasyonuna ulaşılmışsa, yolu döndürür
            if istasyon == hedef: 
                return yol
            
            # Eğer bu istasyon daha önce ziyaret edilmediyse ziyaret edildi olarak işaretlenir
            if istasyon not in ziyaret_edildi: 
                ziyaret_edildi.add(istasyon)

            # Bu istasyonun komşuları üzerinde dönülür
            for komsu, _ in istasyon.komsular:
                if komsu not in ziyaret_edildi:
                    kuyruk.append((komsu, yol + [komsu])) # Bu istasyon yeni yol ile kuyruğa eklenir

        return None 
***A****
'''
def en_hizli_rota_bul(self, baslangic_id: str, hedef_id: str) -> Optional[Tuple[List[Istasyon], int]]:
        if baslangic_id not in self.istasyonlar or hedef_id not in self.istasyonlar:
            return None

        baslangic = self.istasyonlar[baslangic_id]
        hedef = self.istasyonlar[hedef_id]

        # A* için öncelikli kuyruk (toplam süre, istasyonun kimliği, istasyon, yol)
        pq = [(0, id(baslangic), baslangic, [baslangic])]
        ziyaret_edildi = set()

        while pq:
            # Öncelikli kuyruğun başındaki en az süreli öğe çıkarılır
            toplam_sure, _, istasyon, yol = heapq.heappop(pq)

            # Eğer hedef istasyonuna ulaşılmışsa yolu ve toplam süreyi döndürür
            if istasyon == hedef:
                return yol, toplam_sure

            # Eğer bu istasyon daha önce ziyaret edildiyse bu yolu atlar
            if istasyon in ziyaret_edildi:
                continue
        
            ziyaret_edildi.add(istasyon)
            for komsu, sure in istasyon.komsular:
                if komsu not in ziyaret_edildi:
                    yeni_sure = toplam_sure + sure # Her ziyaretten sonra toplam süre güncellenir
                    heapq.heappush(pq, (yeni_sure, id(komsu), komsu, yol + [komsu]))

        return None

- **Test Sonuçları**
'''
rota = metro.en_az_aktarma_bul("M1", "K4")
print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("M1", "K4")
print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

- **En az aktarmalı rota**: AŞTİ -> Kızılay -> Ulus -> Demetevler -> OSB
- **En hızlı rota (20 dakika)**: AŞTİ -> Kızılay -> Demetevler -> OSB


### Projeyi Geliştirme Fikirleri

- **Dinamik Güncelleme**: Gerçek zamanlı olarak metro hattındaki durakları ve bağlantıları güncelleyerek kullanıcıya daha doğru bilgiler sunulabilir.
- **Farklı Ulaşım Araçları Entegrasyonu**: Metro hattının yanı sıra otobüs, tramvay gibi diğer ulaşım araçları da sisteme entegre edilebilir.
