2xx - Başarılı Yanıtlar:
- **200 OK:** İstek başarıyla tamamlandı ve sonuçlar gönderildi.
- **201 Created:** İstek nesne oluşturdu ve başarıyla kaydedildi.
- **204 No Content:** İstek başarıyla tamamlandı, ancak yanıt olarak içerik gönderilmedi (örn. başarıyla silme işlemi).

3xx - Yönlendirmeler:
- **301 Moved Permanently:** İstek kaynağı kalıcı olarak başka bir konuma taşındı. İstemci, yeni konuma yönlendirilmelidir.
- **302 Found (ya da 303 See Other):** İstek kaynağı geçici olarak başka bir konuma taşındı. İstemci, yeni konuma yönlendirilmelidir.
- **304 Not Modified:** Sayfa istendi, ancak değişiklik yok. Önbellek kullanımı için önemli.

4xx - İstemci Hataları:
- **400 Bad Request:** İstek yanlış yapılandırıldı veya sunucu tarafından anlaşılamadı.
- **401 Unauthorized:** Kimlik doğrulama başarısız oldu veya kimlik doğrulama gereklidir.
- **403 Forbidden:** İstemci, kaynağa erişim yetkisine sahip değil.
- **404 Not Found:** İstek kaynağı sunucuda bulunamadı.
- **409 Conflict:** İstek, sunucudaki kaynakla çakışıyor (örn. aynı anda birden fazla istekle aynı kaynağı değiştirme).

5xx - Sunucu Hataları:
- **500 Internal Server Error:** Sunucu içinde bir hata oluştu ve istek işlenemedi.
- **502 Bad Gateway:** Sunucu, üst bir sunucuyla (örn. bir ağ geçidi) geçerli bir yanıt alamadı.
- **503 Service Unavailable:** Sunucu, isteği şu anda işleyemiyor (örn. bakım sırasında).
