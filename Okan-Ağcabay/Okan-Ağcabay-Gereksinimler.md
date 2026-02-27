1. **Sipariş Oluşturma**
   -**API Metodu:**'POST /orders'
   -**Açıklama:** Kullanıcının sepetindeki ürünleri sipariş etmesini sağlar. Ödeme sürecini başlatır.
2. **Sipariş Geçmişini görme**
   -**API Metodu:**'GET /orders/{userId}'
   -**Açıklama:** Kullanıcının geçmiş siparişlerini listeler. Eğer kullanıcı isterse ürünleri tekrar sepetine ekler ve spariş sürecine devam edebilir.
3. **Yorum Yapma**
   -**API Metodu:**'POST /products/{productId}/comments'
   -**Açıklama:** Kullanıcının satın aldığı ürüne yorum yapmasını sağlar. Diğer kullanıcılara ürünün kalitesi hakkında fikir verir.
4. **Yorum Silme**
   -**API Metodu:**'DELETE /comments/{commentId}'
   -**Açıklama:** Kullanıcının yaptığı yorumu silmesini sağlar.
5. **Ürün Arama**
   -**API Metodu:**' GET /products/search?q=anahtarKelime'
   -**Açıklama:** Anahtar kelimeye göre ürün araması yapar. İstenilen ürünlere erişimi kolaylaştırır.
6. **Favorilere Ekleme**
   -**API Metodu:**'POST /favorites'
    -**Açıklama:** Seçilen ürünü kullanıcının favori listesine ekler.
7. **Favorilerden Silme**
    -**API Metodu:**'DELETE /favorites/{productId}'
    -**Açıklama:** Ürünü favori listesinden çıkarır.
8. **Kullanıcı Profil Bilgilerini Güncelleme**
    -**API Metodu:**'PUT /users/{userId}'
    -**Açıklama:** Kullanıcının profil bilgilerini (ad, soyad, telefon vb.) güncellemesini sağlar.
