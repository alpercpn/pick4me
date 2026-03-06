# Ferhat Demir REST API Metotları

**API Test Videosu:** [Link buraya eklenecek](#)

## 1. Giriş Yap
- **Endpoint:** `POST /auth/login`
- **Request Body:**
```json
{
  "email": "ferhatdemir@example.com",
  "password": "Guvenli123!"
}
```
- **Response:** `200 OK` - Kullanıcı başarıyla giriş yaptı ve erişim tokenı oluşturuldu

---

## 2. Profil Bilgilerini Güncelle
- **Endpoint:** `PUT /users/{userId}`
- **Path Parameters:**
  - `userId` (string, required) - Güncellenecek kullanıcının ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "firstName": "Ferhat",
  "lastName": "Demir",
  "email": "ferhatdemir@example.com",
  "phone": "+905551112233"
}
```
- **Response:** `200 OK` - Profil bilgileri başarıyla güncellendi

---

## 3. Hesap Sil
- **Endpoint:** `DELETE /users/{userId}`
- **Path Parameters:**
  - `userId` (string, required) - Silinecek kullanıcının ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Response:** `204 No Content` - Kullanıcı hesabı başarıyla silindi

---

## 4. Film/Dizi Detay Görüntüle
- **Endpoint:** `GET /contents/{contentId}`
- **Path Parameters:**
  - `contentId` (string, required) - Detayı görüntülenecek film veya dizi ID bilgisi
- **Response:** `200 OK` - Film/dizi detay bilgileri başarıyla getirildi

---

## 5. Kütüphanedeki Öğeleri Listele
- **Endpoint:** `GET /users/{userId}/library`
- **Path Parameters:**
  - `userId` (string, required) - Kütüphanesi görüntülenecek kullanıcının ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Response:** `200 OK` - Kullanıcının kütüphanesindeki öğeler başarıyla listelendi

---

## 6. Kütüphanedeki Öğeyi Güncelle
- **Endpoint:** `PUT /users/{userId}/library/{libraryItemId}`
- **Path Parameters:**
  - `userId` (string, required) - İşlemi yapan kullanıcının ID bilgisi
  - `libraryItemId` (string, required) - Güncellenecek kütüphane öğesinin ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "status": "completed",
  "score": 9
}
```
- **Response:** `200 OK` - Kütüphane öğesi başarıyla güncellendi

---

## 7. Bir İçeriğe Puan Ver
- **Endpoint:** `POST /contents/{contentId}/ratings`
- **Path Parameters:**
  - `contentId` (string, required) - Puan verilecek içeriğin ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "userId": "user123",
  "score": 8
}
```
- **Response:** `201 Created` - İçeriğe verilen puan başarıyla kaydedildi

---

## 8. Yorumu Sil
- **Endpoint:** `DELETE /comments/{commentId}`
- **Path Parameters:**
  - `commentId` (string, required) - Silinecek yorumun ID bilgisi
- **Authentication:** Bearer Token gerekli
- **Response:** `204 No Content` - Yorum başarıyla silindi

---

## 9. Yorumu Analiz Et (Küfür / Kötü İçerik Tespiti)
- **Endpoint:** `POST /comments/analyze`
- **Authentication:** Bearer Token gerekli
- **Request Body:**
```json
{
  "text": "Bu film gerçekten çok kötüydü!"
}
```
- **Response:** `200 OK` - Yorum analiz edildi ve sonuç döndürüldü
