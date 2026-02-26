1) **Kayıt Ol**   
      
     - **API Metodu:** `POST /auth/register` 
     - **Açıklama:** Kullanıcıların sisteme yeni hesap oluşturarak kayıt olmasını sağlar. Email/username ve şifre gibi bilgiler alınır, doğrulama kontrolleri yapılır ve kullanıcı hesabı oluşturulur.
  
2) **Çıkış Yap**

     - **API Metodu:** `POST /auth/logout`  
     - **Açıklama:** Kullanıcının aktif oturumunu sonlandırır. Token/oturum bilgisini geçersiz kılar. Bu işlem için kullanıcının giriş yapmış olması gerekir.

3) **Profil Bilgilerini Getir**

     - **API Metodu:** `GET /users/me`  
     - **Açıklama:** Kullanıcının profil bilgilerini görüntülemesini sağlar (kullanıcı adı, email, hesap durumu vb.). Güvenlik için giriş yapmış olmak gerekir.

4) **Film/Dizi Listele**

     - **API Metodu:** `GET /contents`  
     - **Açıklama:** Sistemdeki film/dizi içeriklerini listeler. Tür, yıl, puan aralığı, içerik tipi (film/dizi) gibi filtreler desteklenebilir.

5) **Film/Dizi Arama**

     - **API Metodu:** `GET /contents/search?q=...`  
     - **Açıklama:** Kullanıcının başlık/anahtar kelimeye göre içerik aramasını sağlar. İsteğe bağlı olarak tür/yıl gibi ek filtreler sunulabilir.

6) **Kütüphaneye Film/Dizi Ekle**

     - **API Metodu:** `POST /users/me/library`  
     - **Açıklama:** Kullanıcının bir içeriği kütüphanesine eklemesini sağlar (contentId + başlangıç durumu gibi). Aynı içerik iki kez eklenemez. Giriş gerekli.

7) **Kütüphanedeki Öğeyi Sil**

     - **API Metodu:** `DELETE /users/me/library/{contentId}`  
     - **Açıklama:** Kullanıcının kütüphanesinden seçili içeriği kaldırmasını sağlar. Giriş gerekli.

8) **Bir İçeriğe Yorum Ekle**

     - **API Metodu:** `POST /contents/{contentId}/comments`  
     - **Açıklama:** Kullanıcının içerik altına yorum yazmasını sağlar. Yorum metni, tarih ve kullanıcı bilgisiyle kaydedilir. Küfür/spam kontrolü gibi kurallar eklenebilir. Giriş gerekli.

9) **Kullanıcıya Öneri Üret**

     - **API Metodu:** `GET /users/me/recommendations`  
     - **Açıklama:** Kullanıcının kütüphanesi, izleme geçmişi, verdiği puanlar ve beğendiği türlere göre kişiselleştirilmiş film/dizi önerileri üretir.
