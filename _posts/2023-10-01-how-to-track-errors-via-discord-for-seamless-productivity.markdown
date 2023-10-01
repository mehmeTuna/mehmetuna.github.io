---
layout: post
title: "Sorunsuz Üretkenlik İçin Discord Üzerinden Hata Takibi Nasıl Yapılır?"
date: 2023-10-01 10:12:29 +0300
categories: [Error track]
---

Sınırlı sayıda hizmet sunan bir kod tabanında hata olasılığı daha düşüktür. Ancak bu kod tabanı büyüdükçe maliyetlerin yanı sıra potansiyel hataların da artıp artmadığını
takip etmek gereklidir. Response time'lar, sunucu anlık trafik artışlarında verdiği tepkiler veya eski kullanıcıların birikmiş verilerini iyi yönetebiliyor musunuz? Veya yeni bir özellik eklediğinizde
bu istediğiniz gibi çalışıyor mu? Kullanıcılar bir hatayla karşılaştıklarında çoğu zaman bunun farkında olmazlar. Buradaki sorun, hatayı fark edemeyen bir kullanıcının sizinle iletişime
geçme olasılığının çok düşük olması. Ya da kullandığınız 3. taraf API'ler production ortamında tutarlı bir davranış gösteriyorlar mı? Yaptığınız bir API çağrısı istediğinizin dışında bir hata dönerse?

Buradaki konu, production ortamında olası tüm hataların takibi ve bundan anında haberdar olmaktır.

Örneğin, uygulamanızın günlük trafik oranını her gün mesai bitiminde Discord üzerinden bildirim olarak alıyor olsaydınız? Veya son 24 saat içinde kod tabanında gerçekleşen hataları ve detaylarını
Discord üzerinden bildirim olarak alıyor olsaydınız. Ya da planlı bir toplantı öncesi? Böylece hiç uğraşmadan birçok istatistik ve veriye sahip olacaksınız. Bunu Discord üzerinde basit bir örnek ile deneyelim.

Senaryomuz: Ekibinizin veya diğer ekiplerin işine yarayacak bir istatistik olduğunu düşündünüz veya belirli bir problem olduğunda ilgili kanala bu durumu ileten bir mekanizmaya sahip olmak istediniz.
Bu hizmeti sunan uygulamalar var ama bunların kurulumu zaman alır ve çoğu zaman kod tabanınıza ekstra bağımlılıklar eklerler. Bu tür karmaşık işlerle uğraşmadan Discord üzerinden kolay bir şekilde ilgili
kanala bildirim göndererek kısa zamanda etkili bir çalışma ile çözüm üretebiliriz.

Discord üzerinden aşağıdaki adımları takip ederek bir webhook URL'si edinebilirsiniz.

1. Discord üzerinden ilgili kanalın ayarlar sekmesine gidiyoruz.
   ![discord integrations](/assets/images/discord-integrations-page.png)

2. Daha sonra "Webhook'lar" yazan kısma tıklıyoruz.
   ![discord new webhook](/assets/images/discord-new-webhook.png)

3. Daha sonra "Yeni Webhook" butonuna tıklayarak yeni bir bot oluşturup URL'sini kopyalıyoruz.
   ![discord webhook url](/assets/images/discord-webhook-url.png)

Benim örneğimde uygulama her gün belirli bir saatte günlük ağ trafiğini bir grafik haline getirip ilgili kanala gönderecek.

![network traffic](/assets/images/network-traffic-155209.png)

Aşağıda bildirim gönderilmiş hali mevcut.
![discord error trace](/assets/images/discord-error-trace-103644.png)

Bunun nasıl gerçekleştiğine bir bakalım.

Kopyaladığımız webhook URL'ine bir HTTP post isteği yapacağız. Bu istekler ilgili kanalda bildirim olarak görünecek.

Peki body içeriği nasıl olacak? Benim gönderdiğim gibi bir mesaj içeriğine sahip olmak istiyorsanız

Aşağıdaki gibi bir body oluşturmanız yeterli.

```json
{
  "username": "Traffic",
  "avatar_url": "https://cdn-icons-png.flaticon.com/512/3165/3165065.png",
  "embeds": [
    {
      "title": "Ağ trafiği (kb/s)",
      "url": "https://google.com/",
      "description": "Bu nasıl görünüyor? @app-ops - Mehmet tarafından gönderildi (7kb)",
      "color": 15258703,
      "image": {
        "url": "https://i.ibb.co/b7NK4Mt/Screenshot-2023-09-30-155209.png"
      }
    }
  ]
}
```

Buradaki

`username` alanı bota verdiğimiz ismi içermeli.

`avatar_url` alanı bota bir profil fotoğrafı eklemek için kullanılır.

`embeds` alanı mesaj içeriğini düzenlemek için kullandığımız alandır.

Artık siz de mevcut uygulamanıza hızlıca takip etmek istediğiniz verilerle ilgili bildirim gönderebilir ve bunlardan anında haberdar olabilirsiniz.
