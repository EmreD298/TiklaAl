1. **Kayıt Olma**
   - **API Metodu:** `POST /auth/register`
   - **Açıklama:** Kullanıcının email adresi ve şifre belirlemesi ile sisteme yeni hesap oluşturmasını sağlar.

2. **Kayıt Silme**
   - **API Metodu:** `DELETE /users/{userId}`
   - **Açıklama:** Kullanıcının hesabını sistemden kalıcı olarak silmesini sağlar.

3. **Giriş Yapma**
   - **API Metodu:** `POST /auth/login`
   - **Açıklama:** Kullanıcının kayıt olma işleminde belirlemiş olduğu email ve şifre bilgileri ile sisteme giriş yapmasını sağlar.

4. **Çıkış Yapma**
   - **API Metodu:** `POST /auth/logout`
   - **Açıklama:** Kullanıcının aktif oturumunu sonlandırır.

5. **Ürün Listeleme**
   - **API Metodu:** `GET /products`
   - **Açıklama:** Tüm ürünleri listeler. Kullanıcı isterse filtreleme kısmından istediği kategorideki ürünleri görebilir.

6. **Ürün Detayı Görme**
   - **API Metodu:** `GET /products/{productId}`
   - **Açıklama:** Kullanıcının seçtiği ürünün detay bilgilerini (fiyat, stok, açıklama vb.) gösterir.

7. **Sepette Ürün Ekleme**
   - **API Metodu:** `POST /cart`
   - **Açıklama:** Kullanıcının seçtiği ürünü alışveriş sepetine ekler.

8. **Sepetten Ürün Silme**
   - **API Metodu:** `DELETE /cart/{productId}`
   - **Açıklama:** Kullancı, sepetinde bulunan ürün/ürünlerden istediğini kaldırır.
  
9. **Sepetteki Adedini Güncelleme**
   - **API Metodu:** `UPDATE /cart/{productId}`
   - **Açıklama:** Kullancı, sepetinde bulunan ürünün miktarını arttırıp azaltabilir.
