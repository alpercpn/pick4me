# API Tasarımı - OpenAPI Specification

**OpenAPI Spesifikasyon Dosyası:** [pick4me-openapi.yaml](pick4me-openapi.yaml)

Bu doküman, Pick4Me projesi için OpenAPI Specification (OAS) 3.0 standardına göre hazırlanmış REST API tasarımını içermektedir.

## OpenAPI Specification

```yaml
openapi: 3.0.3
info:
  title: Pick4Me API
  description: |
    Yapay zeka destekli film/dizi öneri platformu için RESTful API.

    ## Özellikler
    - Kullanıcı kayıt ve giriş işlemleri
    - Profil yönetimi
    - Film/dizi listeleme, arama ve detay görüntüleme
    - Kullanıcı kütüphanesi yönetimi
    - Yorum ve puan işlemleri
    - Küfür / kötü içerik analizi
    - Kullanıcıya özel öneri üretimi
  version: 1.0.0
  contact:
    name: Pick4Me Geliştirme Ekibi
    email: support@pick4me.com
    url: https://pick4me.com/support
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.pick4me.com
    description: Production server
  - url: http://localhost:5000
    description: Development server

tags:
  - name: auth
    description: Kimlik doğrulama işlemleri
  - name: users
    description: Kullanıcı profil işlemleri
  - name: contents
    description: Film ve dizi işlemleri
  - name: library
    description: Kullanıcı kütüphanesi işlemleri
  - name: ratings
    description: İçerik puanlama işlemleri
  - name: comments
    description: Yorum işlemleri
  - name: recommendations
    description: Öneri sistemi işlemleri

paths:
  /auth/register:
    post:
      tags:
        - auth
      summary: Yeni kullanıcı kaydı
      description: Sisteme yeni bir kullanıcı kaydeder
      operationId: registerUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RegisterRequest'
      responses:
        '201':
          description: Kullanıcı başarıyla oluşturuldu
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthResponse'
        '400':
          $ref: '#/components/responses/BadRequest'

  /auth/login:
    post:
      tags:
        - auth
      summary: Kullanıcı girişi
      description: Email ve şifre ile giriş yapar
      operationId: loginUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Giriş başarılı
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /auth/logout:
    post:
      tags:
        - auth
      summary: Kullanıcı çıkışı
      description: Giriş yapmış kullanıcının oturumunu sonlandırır
      operationId: logoutUser
      security:
        - bearerAuth: []
      responses:
        '200':
          description: Çıkış başarılı

  /users/{userId}:
    get:
      tags:
        - users
      summary: Profil bilgilerini getir
      description: Belirli bir kullanıcının profil bilgilerini getirir
      operationId: getUserById
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      responses:
        '200':
          description: Kullanıcı bilgileri başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'

    put:
      tags:
        - users
      summary: Profil bilgilerini güncelle
      description: Kullanıcının profil bilgilerini günceller
      operationId: updateUser
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateUserRequest'
      responses:
        '200':
          description: Profil bilgileri başarıyla güncellendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'

    delete:
      tags:
        - users
      summary: Hesap sil
      description: Kullanıcı hesabını sistemden siler
      operationId: deleteUser
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      responses:
        '204':
          description: Kullanıcı hesabı başarıyla silindi
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'

  /contents:
    get:
      tags:
        - contents
      summary: Film/Dizi listele
      description: Platformdaki film ve dizileri listeler
      operationId: listContents
      parameters:
        - name: type
          in: query
          description: İçerik türü
          schema:
            type: string
            enum: [movie, series]
        - name: genre
          in: query
          description: Tür filtresi
          schema:
            type: string
        - name: year
          in: query
          description: Yayın yılı filtresi
          schema:
            type: integer
      responses:
        '200':
          description: İçerik listesi başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ContentList'

  /contents/search:
    get:
      tags:
        - contents
      summary: Film/Dizi ara
      description: Film veya dizi adına göre arama yapar
      operationId: searchContents
      parameters:
        - name: query
          in: query
          required: true
          description: Aranacak kelime
          schema:
            type: string
      responses:
        '200':
          description: Arama sonuçları başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ContentList'

  /contents/{contentId}:
    get:
      tags:
        - contents
      summary: Film/Dizi detay görüntüle
      description: Seçilen içeriğin detaylarını getirir
      operationId: getContentById
      parameters:
        - $ref: '#/components/parameters/ContentIdParam'
      responses:
        '200':
          description: İçerik detay bilgileri başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ContentDetail'
        '404':
          $ref: '#/components/responses/NotFound'

  /users/{userId}/library:
    get:
      tags:
        - library
      summary: Kütüphanedeki öğeleri listele
      description: Kullanıcının kütüphanesindeki içerikleri listeler
      operationId: listLibraryItems
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      responses:
        '200':
          description: Kütüphane başarıyla listelendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LibraryItemList'

    post:
      tags:
        - library
      summary: Kütüphaneye film/dizi ekle
      description: Kullanıcının kütüphanesine yeni bir içerik ekler
      operationId: addLibraryItem
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateLibraryItemRequest'
      responses:
        '201':
          description: İçerik kütüphaneye başarıyla eklendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LibraryItem'

  /users/{userId}/library/{libraryItemId}:
    put:
      tags:
        - library
      summary: Kütüphanedeki öğeyi güncelle
      description: Kütüphane öğesinin durumunu veya puanını günceller
      operationId: updateLibraryItem
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
        - $ref: '#/components/parameters/LibraryItemIdParam'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateLibraryItemRequest'
      responses:
        '200':
          description: Kütüphane öğesi başarıyla güncellendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LibraryItem'

    delete:
      tags:
        - library
      summary: Kütüphanedeki öğeyi sil
      description: Kullanıcının kütüphanesinden bir içeriği siler
      operationId: deleteLibraryItem
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
        - $ref: '#/components/parameters/LibraryItemIdParam'
      responses:
        '204':
          description: Kütüphane öğesi başarıyla silindi

  /contents/{contentId}/ratings:
    post:
      tags:
        - ratings
      summary: Bir içeriğe puan ver
      description: Kullanıcı bir film veya diziye puan verir
      operationId: rateContent
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/ContentIdParam'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateRatingRequest'
      responses:
        '201':
          description: Puan başarıyla kaydedildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Rating'

  /contents/{contentId}/comments:
    post:
      tags:
        - comments
      summary: Bir içeriğe yorum ekle
      description: Kullanıcı seçilen içeriğe yorum ekler
      operationId: addComment
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/ContentIdParam'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateCommentRequest'
      responses:
        '201':
          description: Yorum başarıyla eklendi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Comment'

  /comments/{commentId}:
    delete:
      tags:
        - comments
      summary: Yorumu sil
      description: Kullanıcı kendi yorumunu siler
      operationId: deleteComment
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/CommentIdParam'
      responses:
        '204':
          description: Yorum başarıyla silindi

  /comments/analyze:
    post:
      tags:
        - comments
      summary: Yorumu analiz et
      description: Küfür veya kötü içerik tespiti için yorum analizi yapar
      operationId: analyzeComment
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AnalyzeCommentRequest'
      responses:
        '200':
          description: Yorum analiz sonucu başarıyla döndürüldü
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CommentAnalysisResponse'

  /users/{userId}/recommendations:
    get:
      tags:
        - recommendations
      summary: Kullanıcıya öneri üret
      description: Kullanıcıya özel film/dizi önerileri getirir
      operationId: getRecommendations
      security:
        - bearerAuth: []
      parameters:
        - $ref: '#/components/parameters/UserIdParam'
      responses:
        '200':
          description: Kullanıcıya özel öneriler başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ContentList'

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT token ile kimlik doğrulama

  parameters:
    UserIdParam:
      name: userId
      in: path
      required: true
      description: Kullanıcı ID bilgisi
      schema:
        type: string
        example: "user123"

    ContentIdParam:
      name: contentId
      in: path
      required: true
      description: Film veya dizi ID bilgisi
      schema:
        type: string
        example: "content123"

    LibraryItemIdParam:
      name: libraryItemId
      in: path
      required: true
      description: Kütüphane öğesi ID bilgisi
      schema:
        type: string
        example: "library123"

    CommentIdParam:
      name: commentId
      in: path
      required: true
      description: Yorum ID bilgisi
      schema:
        type: string
        example: "comment123"

  schemas:
    User:
      type: object
      required:
        - id
        - firstName
        - lastName
        - email
      properties:
        id:
          type: string
          example: "user123"
        firstName:
          type: string
          example: "Alper"
        lastName:
          type: string
          example: "Çapan"
        email:
          type: string
          format: email
          example: "alpercapan@example.com"
        phone:
          type: string
          example: "+905551234567"

    RegisterRequest:
      type: object
      required:
        - firstName
        - lastName
        - email
        - password
      properties:
        firstName:
          type: string
          example: "Alper"
        lastName:
          type: string
          example: "Çapan"
        email:
          type: string
          format: email
          example: "alpercapan@example.com"
        password:
          type: string
          format: password
          example: "Guvenli123!"

    LoginRequest:
      type: object
      required:
        - email
        - password
      properties:
        email:
          type: string
          format: email
          example: "alpercapan@example.com"
        password:
          type: string
          format: password
          example: "Guvenli123!"

    UpdateUserRequest:
      type: object
      properties:
        firstName:
          type: string
          example: "Alper"
        lastName:
          type: string
          example: "Çapan"
        email:
          type: string
          format: email
          example: "yenimail@example.com"
        phone:
          type: string
          example: "+905551234567"

    AuthResponse:
      type: object
      properties:
        token:
          type: string
          example: "jwt-token-example"
        user:
          $ref: '#/components/schemas/User'

    Content:
      type: object
      properties:
        id:
          type: string
          example: "content123"
        title:
          type: string
          example: "Interstellar"
        type:
          type: string
          enum: [movie, series]
          example: "movie"
        genre:
          type: string
          example: "Bilim Kurgu"
        releaseYear:
          type: integer
          example: 2014
        averageRating:
          type: number
          format: float
          example: 8.7

    ContentDetail:
      allOf:
        - $ref: '#/components/schemas/Content'
        - type: object
          properties:
            description:
              type: string
              example: "Uzay ve zaman temalı bilim kurgu filmi."
            director:
              type: string
              example: "Christopher Nolan"
            cast:
              type: array
              items:
                type: string
              example: ["Matthew McConaughey", "Anne Hathaway"]

    ContentList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/Content'

    LibraryItem:
      type: object
      properties:
        id:
          type: string
          example: "library123"
        userId:
          type: string
          example: "user123"
        contentId:
          type: string
          example: "content123"
        status:
          type: string
          enum: [watching, completed, dropped, planned]
          example: "watching"
        score:
          type: integer
          minimum: 1
          maximum: 10
          example: 9

    LibraryItemList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/LibraryItem'

    CreateLibraryItemRequest:
      type: object
      required:
        - contentId
        - status
      properties:
        contentId:
          type: string
          example: "content123"
        status:
          type: string
          enum: [watching, completed, dropped, planned]
          example: "watching"

    UpdateLibraryItemRequest:
      type: object
      properties:
        status:
          type: string
          enum: [watching, completed, dropped, planned]
          example: "completed"
        score:
          type: integer
          minimum: 1
          maximum: 10
          example: 9

    CreateRatingRequest:
      type: object
      required:
        - userId
        - score
      properties:
        userId:
          type: string
          example: "user123"
        score:
          type: integer
          minimum: 1
          maximum: 10
          example: 8

    Rating:
      type: object
      properties:
        id:
          type: string
          example: "rating123"
        userId:
          type: string
          example: "user123"
        contentId:
          type: string
          example: "content123"
        score:
          type: integer
          example: 8

    CreateCommentRequest:
      type: object
      required:
        - userId
        - text
      properties:
        userId:
          type: string
          example: "user123"
        text:
          type: string
          example: "Gerçekten çok güzel bir filmdi."

    Comment:
      type: object
      properties:
        id:
          type: string
          example: "comment123"
        userId:
          type: string
          example: "user123"
        contentId:
          type: string
          example: "content123"
        text:
          type: string
          example: "Gerçekten çok güzel bir filmdi."
        createdAt:
          type: string
          format: date-time
          example: "2026-03-06T12:00:00Z"

    AnalyzeCommentRequest:
      type: object
      required:
        - text
      properties:
        text:
          type: string
          example: "Bu film gerçekten çok kötüydü!"

    CommentAnalysisResponse:
      type: object
      properties:
        isToxic:
          type: boolean
          example: false
        containsProfanity:
          type: boolean
          example: false
        confidence:
          type: number
          format: float
          example: 0.92
        message:
          type: string
          example: "Yorum temiz görünüyor."

    Error:
      type: object
      properties:
        code:
          type: string
          example: "BAD_REQUEST"
        message:
          type: string
          example: "İstek parametreleri geçersiz"

  responses:
    BadRequest:
      description: Geçersiz istek
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    Unauthorized:
      description: Yetkisiz erişim
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    NotFound:
      description: Kaynak bulunamadı
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
```