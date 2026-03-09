# API Tasarımı - OpenAPI Specification Örneği

**OpenAPI Spesifikasyon Dosyası:** [Tiklaal_2.yaml](Tiklaal_2.yaml)

Bu doküman, OpenAPI Specification (OAS) 3.0 standardına göre hazırlanmış örnek bir API tasarımını içermektedir.

## OpenAPI Specification

openapi: 3.0.3
info:
  title: TıklaAl E-Ticaret API
  description: Bu API, TıklaAl adlı e‑ticaret web sitesinin temel işlevleri için tasarlanmış bir RESTful setvistir. Sistem, kullanıcıların platforma kayıt olması, giriş yapması, ürünleri görüntülemesi ve alışveriş sepeti üzerinden ürün satın alma sürecini yönetmesi gibi işlemleri sağlar. API, JWT tabanlı kimlik doğrulama (Bearer Token) ile korunmaktadır.

  version: 1.0.0
  contact:
    name: Okan Ağcabay, Emre Dursun
    email: destek@tiklaal.com

servers:
  - url: http://localhost:3000
    description: Development server

tags:
  - name: auth
    description: Kayıt olma, giriş yapma ve çıkış yapma işlemleri
  - name: products
    description: Ürün listeleme, ürün detayı görme işlemleri, ürün arama ve yorum yapma işlemleri
  - name: orders
    description: Sipariş oluşturma ve sipariş geçmişini görme işlemleri
  - name: cart
    description: Sepete ürün ekleme, sepetten ürün silme ve sepetteki adedini güncelleme işlemleri
  - name: comments
    description: Yorm silme işlemi
  - name: favorites
    description: Favorilere ekleme ve favorilerden silme işlemleri
  - name: users
    description: Kayıt silme ve kullancı profil bilgilerini güncelleme işlemleri

paths:  
  /auth/register:
    post:
      summary: Kayıt olma
      description: Kullanıcının email ve şifre ile sisteme yeni hesap oluşturmasını sağlar.
      operationId: registerUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username:
                  type: string
                password:
                  type: string
                email:
                  type: string
      responses:
        "200":
          description: Kullanıcı başarıyla oluşturuldu
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

    /auth/login:
    post:
      summary: Giriş yapma
      description: Email ve şifre ile kullanıcı giriş yapar ve JWT token döner.
      operationId: loginUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username:
                  type: string
                password:
                  type: string
      responses:
        "200":
          description: Giriş yapıldı
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string

    /auth/logout:
    post:
      summary: Çıkış yapma
      description: Kullanıcının aktif oturumunu sonlandırır.
      operationId: logoutUser
      responses:
        "200":
          description: Çıkış yapıldı
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string

    /products:
    get:
      summary: Ürün listeleme
      description: Sistemdeki tüm ürünleri listeler
      operationId: listProducts
      responses:
        "200":
          description: Ürün listeleme başarılı
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'

    /products/{productId}:
    get:
      summary: Ürün detayı görme
      description: Seçilen ürünün detaylarını gösterir.
      operationId: getProductById
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: integer
      responses:
        "200":
          description: Ürün detayı gösterme başarılı
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'

    /products/search:
    get:
      operationId: searchProducts
      summary: Ürün arama
      description: Anahtar kelimeye göre ürün araması yapar. İstenilen ürünlere erişimi kolaylaştırır.
      parameters:
        - name: q
          in: query
          required: true
          schema:
            type: string
          description: Keyword to search for products
      responses:
        "200":
          description: Ürün arama başarılı
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'

    /products/{productId}/comments:
    post:
      operationId: leaveComment
      summary: Yorum yapma
      description: Kullanıcının satın aldığı ürüne yorum yapmasını sağlar. Diğer kullanıcılara ürünün kalitesi hakkında fikir verir.
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                userId:
                  type: integer
                comment:
                  type: string
              required:
                - userId
                - comment
      responses:
        "201":
          description: Yorum başarıyla eklendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'

    /orders:
    post:
      operationId: placeOrder
      summary: Sipariş oluşturma
      description: Kullanıcının sepetindeki ürünleri sipariş etmesini sağlar. Ödeme sürecini başlatır.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                productId:
                  type: integer
                quantity:
                  type: integer
              required:
                - productId
                - quantity
      responses:
        "201":
          description: Sipariş oluşturma başarılı
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
                
    /orders/{userId}: 
    get:
      operationId: getOrderHistory
      summary: Sipariş geçmişini görme
      description: Kullanıcının geçmiş siparişlerini listeler. Eğer kullanıcı isterse ürünleri tekrar sepetine ekler ve spariş sürecine devam edebilir.
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
      responses:
        "200":
          description: Sipariş geçmişi görme başarılı
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Order'

    /cart:
    post: 
      summary: Sepete ürün ekleme
      description: Kullanıcının seçtiği ürünü alışveriş sepetine ekler.
      operationId: addProductToCart
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                productId:
                  type: integer
                quantity:
                  type: integer
                  default: 1
      responses:
        "200":
          description: Ürün ekleme başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string

    /cart/{productId}:
    delete:
      summary: Sepetten ürün silme
      description: Sepetteki ürünü kaldırır.
      operationId: removeProductFromCart
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: integer
      responses:
        "204":
          description: Ürün silme başarılı

    put:
      summary: Sepetteki adedini güncelleme
      operationId: updateProductQuantityInCart
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                quantity:
                  type: integer
      responses:
        "200":
          description: Ürün miktarı güncelleme başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string

    /comments/{commentId}:
    delete:
      operationId: removeComment
      summary: Yorum silme
      description: Kullanıcının yaptığı yorumu silmesini sağlar.
      parameters:
        - name: commentId
          in: path
          required: true
          schema:
            type: integer
      responses:
        "204":
          description: Yorum silme başarılı

    /favorites:
    post:
      operationId: addToFavorites
      summary: Favorilere ekleme
      description: Seçilen ürünü kullanıcının favori listesine ekler.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                userId:
                  type: integer
                productId:
                  type: integer
              required:
                - userId
                - productId
      responses:
        "201":
          description: Ürün başarıyla favorilere eklendi

    /favorites/{productId}:
    delete:
      operationId: removeFromFavorites
      summary: Favorilerden silme
      description: Ürünü favori listesinden çıkarır.
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                userId:
                  type: integer
              required:
                - userId
      responses:
        "204":
          description: Ürün favorilerden silindi

     /users/{userId}:
    delete:
      summary: Kayıt silme
      description: Sepette bulunan ürünün miktarını değiştirir.
      operationId: deleteUserAccount
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
      responses:
        "204":
          description: Kayıt silme başarılı

    put:
      operationId: updateUserProfile
      summary: Kullanıcı profil bilgilerini güncelleme
      description: Kullanıcının profil bilgilerini (ad, soyad, telefon vb.) güncellemesini sağlar.
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username:
                  type: string
                email:
                  type: string
                fullName:
                  type: string
                phone:
                  type: string
      responses:
        "200":
          description: Profil başarıyla güncellendi

    components:
    schemas:
    User:
      type: object
      description: Bir kullanıcı siteye kayıt oldu.
      properties:
        userId:
          type: integer
        username:
          type: string
        email:
          type: string

    Product:
      type: object
      description: Sitede bir ürün listelendi.
      properties:
        id:
          type: integer
        name:
          type: string
        description:
          type: string
        price:
          type: number
          format: float

    Order:
      type: object
      description: Bir müşteri tarafındanbsipariş verildi.
      properties:
        orderId:
          type: integer
        productId:
          type: integer
        quantity:
          type: integer
        status:
          type: string
          description: Status of the order (e.g., pending, completed)
          
    Comment:
      type: object
      description: Bir ürüne yorum yapıldı.
      properties:
        commentId:
          type: integer
        userId:
          type: integer
        productId:
          type: integer
        comment:
          type: string
        createdAt:
          type: string
          format: date-time

 
 
 
  

  

  

  

  

  
    
