---
layout: post
title: "Heybooster Front End Projesi: Clean Code Prensipleri ile Optimizasyon"
date: 2023-05-26 10:48:16 +0000
slug: heybooster-front-end-projesi-clean-code-prensipleri-ile-optimizasyon
tags: [software-development, software-engineering, frontend-development, clean-code, clean-architecture]
lang: tr
---



![Cover Image]({{ '/assets/images/c77b26902dcd14e374deabce7efc10cf.jpeg' | relative_url }})

Yazılım geliştirme sürecinde, kodun sürdürülebilir, okunabilir ve yönetilebilir olması son derece önemlidir. Bu prensipleri benimseyerek ve uygulayarak, [Heybooster](https://app.heybooster.ai/) Front End projemizde önemli iyileştirmeler gerçekleştirdim. Bu blog yazısında, kod optimizasyonu ve clean code prensiplerinin nasıl uygulandığını ve bu iyileştirmelerin projeyi nasıl etkilediğini anlatacağım.

### Sabit Enum Değişkenlerin Yeniden Düzenlenmesi

Vue.js projemizde çok sayıda component, layout ve componentlerde kullanılan sabit değişkenler bulunmaktaydı. Bu değişkenlerin tekrarlanması ve yönetimini kolaylaştırmak adına, tüm sabit enum değişkenlerini bir ‘enums’ klasörü altında topladım. Bu şekilde, tekrarlanan tüm enum değişkenlerini silip, ihtiyaç duyulduğunda ‘enums’ klasöründen import ederek kullanmaya başladım. Bu değişiklik, kodun daha optimize olmasını ve tekrarlanan kısımların ortadan kalkmasını sağladı.

### Constants Klasörü Oluşturulması

Benzer bir yaklaşımı constants için de uyguladım. Tekrarlanan tüm constantsları bir ‘constants’ klasörü altında topladım. Constantslar artık sadece bu klasörden import edilerek kullanılmakta. Bu değişiklikle, constantslar tek bir seferde tanımlanmış oldu ve kodun daha sade ve yönetilebilir hale gelmesine yardımcı oldu.

### API İsteklerinde Vuex Actions Kullanarak Tekrarlanan Fonksiyonların Optimize Edilmesi

Proje içerisinde birden fazla component ve API isteği için tekrarlanan fonksiyonlar bulunmaktaydı. Bu tekrarlanan fonksiyonları Vuex actionları ile yeniden oluşturdum ve tekrarlanan methodları sildim. Bu sayede, aynı işlevi gerçekleştiren kodları bir kez yazıp, ihtiyaç duyulan her yerde bu actionları kullanmaya başladım.

### Composable Functions ile Tekrarlanan Kodların Düzeltilmesi

Vue.js özelliklerini kullanarak oluşturulan method, functions, computed ve benzeri kodlar da sıklıkla tekrarlanmaktaydı. Bu tekrarlanan kodları düzeltmek ve daha sade bir yapıya kavuşturmak adına, bunlar için composable functions oluşturdum. Böylece tekrarlanan tüm method, functions, computed vs. kodları silerek, yerlerine composableları kullanmaya başladım.

### Utility Helper ve Utility Fonksiyonlarının Toplanması

Proje içerisinde, çeşitli işlemler için birden fazla yerde yeniden oluşturulan ve kullanılan utility fonksiyonları bulunmaktaydı. Bu kodların yönetimini kolaylaştırmak ve tekrarlanan kısımları ortadan kaldırmak adına, bu kodları ‘utility’ adında bir klasör altında topladım. Böylece, bu utility fonksiyonları tek bir yerden yönetmeye ve kullanmaya başladım, kod tekrarlarını önledim ve kodu optimize ettim.

### Axios Wrapper Plugin’in Oluşturulması

API istekleri için her seferinde axios’ı import etmek ve konfigürasyonları yapmak zaman alıcı bir işlem olabiliyor. Bu süreci hızlandırmak ve daha verimli hale getirmek adına, bir [axios wrapper plugin](https://gist.github.com/mustafadalga/b0bf1f48608cf2e8013bc0bf4bc7d394) oluşturdum. Bu plugini global olarak tanımlayıp projenin her yerinde kullanmaya başladım. Bu sayede, her seferinde axios’ı import etme ve konfigürasyon işlemleri ile zaman kaybetme sorununu ortadan kaldırdım.

### Sonuç

Bu değişiklikler ve iyileştirmeler, [Heybooster](https://app.heybooster.ai/) Front End projemizin kodunun daha sade, yönetilebilir ve optimize olmasını sağladı. Clean code prensiplerinin uygulanması, projemizin bakımını kolaylaştırdı. Bir yazılım geliştirici olarak, kodun sürdürülebilirliği ve kalitesi benim için her zaman öncelikli olmuştur ve bu prensipler, projelerimizde daha etkin bir şekilde çalışmamıza olanak sağlamaktadır. Bu tür iyileştirmeler ve optimizasyonlar, kodun kalitesini artırmakla kalmaz, aynı zamanda ekip çalışmasını da kolaylaştırır ve projenin genel başarısına katkıda bulunur.
