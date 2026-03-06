# Alper Çapan REST API Metotları

**API Test Videosu:** [Link buraya eklenecek](#)

## 1. Kayıt Ol
- **Endpoint:** `POST /auth/register`
- **Request Body:**
```json
{
  "email": "alpercapan@example.com",
  "password": "Guvenli123!",
  "firstName": "Alper",
  "lastName": "Çapan"
}
```
- **Response:** `201 Created` - Kullanıcı başarıyla oluşturuldu

---

## 2. Çıkış Yap
- **Endpoint:** `POST /auth/logout`
- **Authentication:** Bearer Token gerekli
- **Response:** `200 OK` - Kullanıcı başarıyla çıkış yaptı

---

## 3. Profil Bilgilerini Getir
- **Endpoint:** `GET /users/{userId}`
- **Path Parameters:**
  - `userId` (string, required) - Kullanıcı ID'si
- **Authentication:** Bearer Token gerekli
- **Response:** `200 OK` - Kullanıcı bilgileri başarıyla getirildi

---

## 4. Film/Dizi Listele
- **Endpoint:** `GET /contents`
- **Response:** `200 OK` - Film ve dizi listesi başarıyla getirildi

---

## 5. Film/Dizi Ara
- **Endpoint:** `GET /contents/search`
- **Query Parameters:**
  - `query` (string, required) - Aranacak film veya dizi adı
- **Response:** `200 OK` - Arama sonuçları başarıyla getirildi

---

## 6. Kütüphaneye Film/Dizi Ekle
- **Endpoint:** `POST /users/{userId}/library`
- **Path Parameters:**
  - `userId` (string, required) - Kullanıcı ID'si
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "contentId": "content123",
  "status": "watching"
}
```
- **Response:** `201 Created` - Film veya dizi kütüphaneye başarıyla eklendi

---

## 7. Kütüphanedeki Öğeyi Sil
- **Endpoint:** `DELETE /users/{userId}/library/{libraryItemId}`
- **Path Parameters:**
  - `userId` (string, required) - Kullanıcı ID'si
  - `libraryItemId` (string, required) - Silinecek kütüphane öğesinin ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Response:** `204 No Content` - Kütüphane öğesi başarıyla silindi

---

## 8. Bir İçeriğe Yorum Ekle
- **Endpoint:** `POST /contents/{contentId}/comments`
- **Path Parameters:**
  - `contentId` (string, required) - Yorum yapılacak içeriğin ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "userId": "user123",
  "text": "Gerçekten çok güzel bir filmdi."
}
```
- **Response:** `201 Created` - Yorum başarıyla eklendi

---

## 9. Kullanıcıya Öneri Üret
- **Endpoint:** `GET /users/{userId}/recommendations`
- **Path Parameters:**
  - `userId` (string, required) - Öneri üretilecek kullanıcının ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Response:** `200 OK` - Kullanıcıya özel öneriler başarıyla üretildi
