---
layout: post
title: "Vue.js Projesinde JavaScript'ten TypeScript'e Migration"
date: 2023-05-26 08:08:53 +0000
slug: vuejs-projesinde-javascriptten-typescripte-migration
lang: tr
tags: [javascript, vuejs, migration, reactjs, typescript]
---

Vue.js 2 projemizi Vue.js 3'e yükselttikten sonra kod tabanını JavaScript'ten TypeScript'e geçirmeye karar verdik. Büyük bir projede statik tip kontrolü olunca hataları henüz runtime'a çıkmadan yakalamak, iyi bir autocomplete almak ve kodu takip etmek çok daha rahat oluyor.

Ama ortada bir sürü eski `.js` dosyası vardı. Bunların hepsini tek tek oturup TypeScript'e çevirecek ne vaktimiz vardı ne de bunu yapmak mantıklıydı. Çünkü o kadar dosyayı aynı anda refactor etmeye çalışsan bütün işler durur.

Biz de hızlı bir çözüm olarak küçük bir bash script yazdık ve projedeki tüm `.js` dosyalarının uzantısını `.ts` olarak değiştirdik. Script sadece uzantıyı değiştirmekle kalmadı, her dosyanın en üst satırına `//@ts-nocheck` ekledi.

Bu sayede TypeScript compiler eski dosyalardaki tip hatalarını görmezden geldi ve çalışan mevcut kod hiç bozulmadan devam etti. Kullanılan bu script'i kendi [GitHub hesabımda](https://github.com/mustafadalga/file-extension-converter) paylaştım.

Tabi bu sadece migration'ın ilk adımıydı. Bundan sonraki süreçte amaç, bir dosyada değişiklik yapıldıkça en üstteki `//@ts-nocheck` satırını silip oradaki tipleri düzgünce tanımlamak. Bütün projeyi baştan yazmaya kalkışmadan, sistemi çalışır halde tutarak adım adım TypeScript'e geçmek pratikte çok daha sorunsuz ilerliyor.