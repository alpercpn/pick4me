1) **Giriş Yap**   
      
     - **API Metodu:** `POST /auth/login` 
     - **Açıklama:** Kullanıcının email/username ve şifre ile sisteme giriş yapmasını sağlar. Başarılı girişte oturum/token üretilir ve kullanıcı yetkilendirilir.
  
2) **Profil Bilgilerini Güncelle**

     - **API Metodu:** `PUT /users/me`  
     - **Açıklama:** Kullanıcının ad, soyad, kullanıcı adı, profil fotoğrafı vb. profil bilgilerini güncellemesini sağlar. Güvenlik için giriş yapmış olmak gerekir ve kullanıcı yalnızca kendi profilini güncelleyebilir.

3) **Hesabı Sil**

     - **API Metodu:** `DELETE /users/me`  
     - **Açıklama:** Kullanıcının hesabını sistemden kalıcı olarak siler. Kullanıcıya ait kütüphane, yorum, puan vb. verilerin silinmesi/anonimleştirilmesi kurala bağlanır. İşlem geri alınamaz; giriş gerekli.

4) **Film/Dizi Detay Görüntüle**

     - **API Metodu:** `GET /contents/{contentId}`  
     - **Açıklama:** Seçilen içerik için detay sayfasını döndürür (özet, türler, oyuncular, afiş, ortalama puan, yorum sayısı vb.).
5) **Kütüphanedeki Öğeleri Listele**

     - **API Metodu:** `GET /users/me/library`  
     - **Açıklama:** Kullanıcının kütüphanesindeki içerikleri listeler. İzleniyor/izlendi/planlanıyor gibi durumlara göre filtreleme yapılabilir. Giriş gerekli.

6) **Kütüphanedeki Öğeyi Güncelle**

     - **API Metodu:** `PUT /users/me/library/{contentId}`  
     - **Açıklama:** Kullanıcının kütüphane öğesi durumunu günceller (örn. izleniyor → izlendi), isteğe bağlı izlenme tarihi/son bölüm gibi alanlar eklenebilir. Giriş gerekli.

7) **Bir İçeriğe Puan Ver**

     - **API Metodu:** `POST /contents/{contentId}/ratings`  
     - **Açıklama:** Kullanıcının bir içeriğe puan vermesini sağlar (örn. 1–10). Kullanıcı aynı içeriğe tek puan verebilir; tekrar verirse güncelleme gibi ele alınabilir. Giriş gerekli.

8) **Yorumu Sil**

     - **API Metodu:** `DELETE /comments/{commentId}`  
     - **Açıklama:** Kullanıcının kendi yorumunu silmesini sağlar. Yetki kontrolü yapılır (yalnızca yorum sahibi veya admin). Giriş gerekli.

9) **Yorumu Analiz Et (Küfür / Kötü İçerik Tespiti)**

     - **API Metodu:** `POST /ai/comments/analyze`  
     - **Açıklama:** Kullanıcının gönderdiği yorumun yapay zeka destekli olarak analiz edilmesini sağlar. Yorumun küfür, hakaret, nefret söylemi, taciz, spam gibi kötü içerikler içerip içermediği tespit edilir ve sonuca göre yorumun yayınlanmasına izin verilir, uyarı verilir veya engellenir. Bu analiz, yorum kaydedilmeden önce (pre-check) ya da yorum kaydedildikten sonra (post-check/moderasyon kuyruğu) çalıştırılabilir.

