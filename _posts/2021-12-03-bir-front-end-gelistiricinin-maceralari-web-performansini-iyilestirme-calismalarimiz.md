---
layout: post
title: "Bir Front-End Geliştiricinin Maceraları — Web Performansını iyileştirme çalışmalarımız"
date: 2021-12-03 12:17:55 +0000
slug: bir-front-end-gelistiricinin-maceralari-web-performansini-iyilestirme-calismalarimiz
tags: [optimization, web-performance, web-development, best-practices, web-optimization]
lang: tr
---



![Cover Image]({{ '/assets/images/2a6ae3ce-6246-46e1-aba8-3dea1e29651a.png' | relative_url }})

[Heybooster](https://heybooster.ai/)’da Front-End Developer olarak işe başladığımda, [product ownerimiz](https://www.linkedin.com/in/neslihanbusraemikoglu/) aşağıdaki sorunları çözmemi istedi:

*   Kullanıcılardan sıkça dönüş aldığımız sayfa hızı sorunlarının çözümüne öncülük etmem,
*   Şu andaki yapıdaki bugların çözülmesi ile birlikte yeni tasarımların da %99 oranında aynı şekilde ve responsive şekilde geçirilmesi,
*   Sitedeki tüm userflowları cover ettiğimize emin olmak,

Ben de [Heybooster’i](https://www.heybooster.ai/) iyileştirmek, geliştirmek ve ilerletmek için elimden gelenin en iyisini yapmaya karar verip çalışmaya başladım.

Bu blog yazısında anlattığım çalışmalar genel olarak Front-End tarafındaki geliştirmeleri ve iyileştirmeleri kapsamaktadır.

#### **Gereksiz veya kullanılmayan paketlerin ve componentlerin kaldırılması**

*   Projemizde kurulu plan npm paketlerini incelediğimde bazı paketlerin kurulu olmasına rağmen proje içerisinde hiç kullanılmadığını gördüm ve bu npm paketlerini kaldırdım.
*   Proje içerisinde çok basit işlemler için bile npm paketinin kurulduğunu ve bu paketlerin beraberinde gereksiz yükler getirdiğini ve aslında bir npm paketi kullanmadan da sorunlarımızı çözebileceğimi farkettim.Bu npm paketlerinin hepsini kaldırarak kendi componentlerimizi ve javascript methodlarımızı yazdım.
*   Projenin geliştirilmesi ile birlikte bazı componentler artık kullanılmıyordu. Fakat yine de bu componentler import edilmeye devam ediliyordu. Kod incelemesi yaparak kullanmadığımız bu componentleri sildim.

#### **Büyük boyutlu paketlerin değiştirilmesi**

Line chart ve Treemap chart grafiklerini ve Grid tablosunu kullanmak için [syncfusion](https://www.syncfusion.com/)’un geliştirdiği paketleri kullanıyorduk.Fakat bu paketlerin boyutları çok yüksek olduğu için grafik oluşturulup kullanıcıya gösterilene kadar çok zaman geçmekteydi ve aynı zamanda kötü bir kullanıcı deneyimi oluşturmaktaydı.Bu sorunu çözmek için bu üç kütüphaneyi değiştirmeye karar verdim.

Yeni bir paket seçmeye karar verirken belirlediğimiz kriterler aşağıdaki gibiydi:

*   Açık kaynak olması
*   Desteklediği özellikler
*   Topluluk desteği
*   Paket boyutunun küçük olması
*   Ücretsiz olması

Yukarıdaki kriterlere göre çeşitli paketleri araştırdım ve deneyerek test ettim.Bizim projemiz için en uygun paketlerden, Line chart için [chart.js](https://www.npmjs.com/package/chart.js)’i, Grid tablosu için [grid.js](https://gridjs.io/)’i ve Treemap chart için [highcharts.js](https://www.npmjs.com/package/highcharts)’i kullanmaya karar verdim.

Bu 3 paketi değiştirdikten sonraki boyut değişimleri aşağıdaki grafik gibidir.

![]({{ '/assets/images/92abfa20-547f-4ef1-b9e6-40a9ad29786c.png' | relative_url }})

Markdown işlemleri için [vue-markdown](https://www.npmjs.com/package/vue-markdown) ve [vue-simple-markdown](https://www.npmjs.com/package/vue-simple-markdown) iki npm paketi kullanılmaktaydı.bütün markdown işlemleri için tek paket kullanmak için yukarıdaki iki paketi kaldırarak [marked.js](https://www.npmjs.com/package/markedjs)’yi kurdum.

![]({{ '/assets/images/a12acca8-3562-4a62-b721-d91665fe6af8.png' | relative_url }})

![]({{ '/assets/images/0597fd56-90dd-4a86-b20e-2484419fd969.png' | relative_url }})

#### Tekrarlayan kodların önüne geçmek için mixins ve helpers’lerin kullanılmaya başlanması

Projemizi geliştirirken aynı zamanda code review yaparak kendini tekrarlayan kodlar ile çok sık karşılaşmaktaydım. Daha temiz, yönetilebilir ve optimize kodlar için kendini tekrarlayan kodları mixins ve helperlere çevirdim.

#### Global olmayan paket, component, prototype ve mixins’lerin ana main.js dosyasından kaldırılması

Ana javascript dosyası main.js içerisinde global olmayan paket, component, prototype ve mixinslerin import edildiğini gördüm ve bu global paket, component, mixinleri sadece gerekli olduğu yerde içeri aktarılıp kullanılabilecek şekilde değiştirdim. Prototyleri de enums olarak localde oluşturdum.

#### Verileri ve durumları yönetmek için vuex’in kullanılmaya başlanması

Durum yönetimini sağlamak için [eventBus](https://v3.vuejs.org/guide/migration/events-api.html#_2-x-syntax) kullanmaktaydık. Fakat eventBus projemiz için hiç efektif ve kullanışlı değildi ve kodun okunabilirliğini, yönetimini ve geliştirilmesini zorlaştırıyordu.

Verileri yönetmek için de herhangi global bir yapımız bulunmamaktaydı.

Verileri yönetmek için bir yapının olmaması performance açısından büyük kayıplara yol açıyordu;

*   Her sayfa değişikliğinde tekrar tekrar aynı veriyi almak için api istekleri yapılıyordu ve her seferinde api sonucları gelene kadar kullanıcı beklemek zorunda kalıyordu.
*   Aynı veri birden fazla yerde kullanılmasına rağmen aynı veriyi elde etmek için farklı yerlerde tekrardan api isteğinde bulunuyordu.

Durum ve verilerin yönetimini sağlayarak web performansını iyileştirmek ve projenin yönetilebilirliğini artırmak için vue js’nin resmi kütüphanesi vuex’i kullanmaya karar verdik. Projenin durumlarını ve verilerini kategorilere ayırmamız gerekeceğini için vuex modüllerini oluşturmam gerekecekti.Vuex modulleri hakkında bilgim olmadığı için test amaçlı bir proje ile vuex modülleri oluşturup kullanarak yapısına hakim oldum.

Şuan [vuex modulleri](https://medium.com/heybooster/vuex-on-large-projects-how-to-structure-it-78c45c2b3f16)ni kullanarak projenin veri ve durumlarını kolay bir şekilde yönetebiliyorum.

#### Code Splitting and Lazy Loading

Bütün viewsler ve componentler tek bir javascript dosyasında toplanmaktaydı.Projedeki component ve views sayısı da büyüdüğü için ana javascript bundle paketinin boyutu çok büyüdü ve sayfa ve componentlerin yüklenme sürelerini kötü etkilemeye başladı.Bu kötü etki de web sayfasının yüklenmesini 30 saniye’ye kadar uzatıyordu.

![]({{ '/assets/images/a4f45959-b2b6-4136-ba70-f29bb226fb70.png' | relative_url }})

Bütün component ve views’leri tek bir javascript dosyasında toplamak yerine bunları dinamik olarak yükleyerek parçalarına ayırdım.Böylece bütün yükü dağıtarak ana javascript paketinin boyutunu küçülttük.

![]({{ '/assets/images/409d3c74-0f72-4833-9a7b-08e96251eecd.png' | relative_url }})

#### Sayfayı yenilemeden verileri değiştirme

Heybooster client bazlı çalışan bir projedir. Kullanıcının her client değiştirdiğinde o cliente ait verilerin yüklenmesi için sayfa yenileniyordu.Her sayfa yenilendiğinde bütün javascript ve resim dosyaları yeniden yüklenmesi performanse kaybına sebep olmaktaydı. ayrıca bazı apiler kullanıcı bazında çalıştığı için tekrardan yüklenmesi gereksizdi.Bu durum vue’nin yapısına da uygun olmayan yanlış geliştirilmiş bir özellikti.

Bu sorunu çözmek aşağıdaki adımları izledik.

*   Client değiştirildiğinde, bir cliente ait bütün bütün vuex verilerini temizledim.
*   Client’e göre çalışan bütün api isteklerini tekrarlatarak api isteklerini yeniden isteklerde bulundum.

#### Cache sorunlarının çözülmesi

Her deployment sonrası yaşadığımız en büyük problem cachelerin temizlenmesi ile ilgiliydi. Tarayıcı javascript dosyasını indirdikten sonra disk cache’sine kaydediyordu.Performance açısından baktığımızda web sayfasının daha hızlı yüklenmesi için iyi bir durum fakat yeni geliştirmelerin canlıya alınmasında sorunlar yaşıyorduk.

![]({{ '/assets/images/b64cc007-5103-4336-8163-bc0b1e8b1913.png' | relative_url }})

Her deployment sonrası cache’yi silme ilgili sorunları çözmek için aşağıdaki yöntemleri denedim:

*   Webpack ile javascript dosyalarının isimlerini dynamic hale getirerek hep deployment sonrası [cache](https://webpack.js.org/guides/caching/)’yi bozma: Bu çözümün uygulanması için her deployment sonrası kullanıcının sayfayı yenilemesi gerekiyordu. Çünkü eski bundle dosyaları silinerek içeriğe göre yeni javascript dosyaları oluşturuluyordu.Fakat kullanıcı sayfayı yenilenmeden başka bir sayfaya geçiş yapmak istediğinde “Loading chunk errors” hataları ile karşılaşıyorduk.Çünkü kullanıcı hala eski javascript dosyalarını kullanıyordu fakat o javascript dosyaları artık mevcut değil.
*   [Service worker](https://github.com/mustafadalga/service-worker) ile her deployment sonrası kullanıcıya bir uyarı mesajı göstererek yeni sürümün yüklenmesi için sayfayı yeniletmek:Bu çözüm ile de tam olarak istediğimiz gibi çalışmadı.Bu yüzden service worker’i kaldırdık.

Yukarıdaki çözümlerin hiçbiri tam olarak işimizi görmediği için ayarları eski haline getirdik.Front end tarafını [amplify](https://aws.amazon.com/tr/amplify/)’e taşıdıktan sonra amplify’in cache yönetimi ile ilgili bir çözümün olup olmadığını araştırdım ve amplify’in [performance modu](https://docs.aws.amazon.com/amplify/latest/userguide/ttl.html#Instant-cache-invalidation-with-instant-deploys) ile cache sorunumuzu çözebileceğimizi öğrendim. Performance modu’nu ayarladıktan sonra javascript dosyalarımız amplify cdn’si tarafından yönetilmeye başlandı ve her deployment sonrası cachemizi geçersiz kıldı.

Genel olarak web performansımızı iyileştirmek için yaptığım çalışmalardan bahsettim. Yukarıdaki çalışmaların dışında yapabileceğimiz birçok çalışma mevcut. Bunları uyguladıkça ve geliştirdikçe tecrübelerimi aktarmaya devam edeceğim.

Açık kaynak geliştirmelerimi de [github](https://github.com/mustafadalga) adresimde paylaşmaktayım.
