---
layout: post
title: "TailwindCSS ile SCSS’nin Ötesine Geçme: Clean Code ve CSS Optimize Etme"
date: 2023-05-26 09:59:30 +0000
slug: tailwindcss-ile-scssnin-otesine-gecme-clean-code-ve-css-optimize-etme
tags: [frontend, frontend-development, front-end-cik5w32oi016zos53hitiymhh, tailwind, tailwind-css]
lang: tr
---



![Cover Image]({{ '/assets/images/8784d607-9846-48e9-84e9-16579c5b7f89.png' | relative_url }})

Bu blog yazısında , bir süre önce karşılaştığım bir problemden ve bu problemi nasıl çözdüğümden bahsetmek istiyorum.

Proje ekibimizle birlikte Vue.js ile bir dizi komponent üzerinde çalışıyorduk. Projemizde SCSS (veya SASS) pre-processor kullanarak UI tasarımını yönetiyorduk. SCSS ile utility classları oluşturduk, ancak genellikle 20px, 30px, 80px gibi belirli genişliklere ihtiyaç duyduğumuzda, bu gibi genel olmayan utility classları oluşturmadık. Bu genellikle bazı belirli durumlarda gerekiyordu ve her seferinde bunları oluşturmak kod tekrarına neden oluyordu. SCSS ile oluşturduğum utility classlarına [bu gist](https://gist.github.com/mustafadalga/3a105e7ef96da1dc85d4294902265eae)’ten göz atabilirsiniz.

Ancak zamanla fark ettim ki, kendimi tekrar eden CSS kodları yazıyorum. Örneğin, belki 100 farklı yerde `padding: 32px` gibi bir kod kullanıyordum. Bu, dosyaların boyutunu gereksiz yere artırmakla kalmıyordu, aynı zamanda temiz kod prensiplerine de aykırıydı.

Bu durumun bir çözümünü bulmalıydım. Çözüm olarak, TailwindCSS kullanmayı tercih ettim. Bu CSS framework’ünü kullanmaya başladığımda, sadece gerekli olan classları build etme imkanım oldu. Örneğin, 100 farklı yerde `padding: 32px` kullanmak yerine, sadece bir kez `p-[2rem]` classını oluşturabiliyordum. Bu da kendini tekrarlayan kodların önüne geçti.

Peki, neden TailwindCSS kullanmaya karar verdim ve bu SCSS ile ne farklılık gösteriyor?

TailwindCSS, “utility-first” bir CSS frameworküdür. Bu, bize özelleştirilebilir, yeniden kullanılabilir ve okunabilir CSS classları oluşturma yeteneği sağlar. Her bir class, belirli bir stil özelliği (örneğin, padding, margin, genişlik vb.) tanımlar. Bu, kod tekrarını büyük ölçüde azaltır ve bize daha fazla kontrol imkanı sunar. Özellikle büyük ölçekli projelerde, TailwindCSS ile kodun daha ölçeklenebilir ve yönetilebilir olmasını sağlayabiliriz.

SCSS de benzer bir işlev görür ancak daha çok geleneksel CSS pre-processor olarak hareket eder. SCSS ile oluşturulan stil dosyaları genellikle daha büyük ve daha karmaşıktır. Kendi içinde değişkenler, mixin’ler, fonksiyonlar ve daha fazlasını barındırır. SCSS’nin sunduğu bu özellikler yararlıdır ancak aynı zamanda kodun karmaşıklaşmasına ve genişlemesine yol açabilir.

Bu nedenle, kendimi tekrar eden CSS kodlarından kaçınmak ve daha temiz, daha ölçeklenebilir bir kod tabanı oluşturmak için TailwindCSS’i tercih ettim. TailwindCSS’in utility-first yaklaşımı, bir kez oluşturulan ve birçok yerde kullanılan classların, kod tekrarını önler ve bu sayede daha hızlı ve daha verimli bir şekilde kod yazmamızı sağlar.

Sonuç olarak, TailwindCSS ve SCSS arasında kesin bir kazanan yoktur. Her ikisi de belirli durumlar ve ihtiyaçlar için uygundur. Ancak benim durumumda, TailwindCSS’nin sağladığı ölçeklenebilirlik ve esneklik, karmaşıklığı azalttı ve kodun daha temiz ve düzenli olmasını sağladı. Ayrıca, projemizde utility classlarının yeniden kullanılabilirliği sayesinde, kod tabanımızın boyutunu önemli ölçüde azaltmayı başardık.

Bu blog yazısıyla, bir başka front-end mühendisinin benzer bir probleme çözüm ararken, TailwindCSS’i düşünmesi için bir sebep daha oluşturmayı umuyorum. Kendi tecrübelerimden yola çıkarak, TailwindCSS’in utility-first yaklaşımının, projelerin daha hızlı ve daha verimli bir şekilde ilerlemesine yardımcı olabileceğine inanıyorum.

Umarım bu blog yazısı, sizin ve takımınızın karşılaştığı sorunları ve çözümleri yeterince açıklamıştır. Herhangi bir düzenleme veya ek bilgi gerekip gerekmediği konusunda bana bildirin.
