---
layout: post
title: "Front End Takımındaki Standart Uyumsuzlukları ve Çözümüm"
date: 2024-11-19 22:00:00 +0000
slug: front-end-takimindaki-standart-uyumsuzluklari-ve-cozumum
tags: [front-end-development, problem-solving, team-management, coding-standards, codingstandards]
lang: tr
---


Front End Developer olarak çalışırken projede ve takım içinde ortak bir yazma standardını nasıl belirlediğimi ve bu süreçte yaşadığım sorunları bu blog yazısında paylaşmak istiyorum.

**Kod Geliştirme ve Standartlar**

Kodlarımızı geliştirdikten sonra her zaman pull request ile code review’e gönderiyoruz. Ancak bu süreçte belli başlı standartlara uymakta zorlanıldığını fark ettim. Projede standart bir yazım dili oturtmaya çalışıyordum, ancak aşağıdaki gibi birçok sorunla karşılaştık:

• **Değişken İsimlendirme:** CamelCase kullanıyorduk, fakat bazı geliştiriciler bu standarda uymuyordu.

• **Enum İsimlendirme:** Legacy kodlarda enum isimleri camelCase ile yazılmıştı, yeni kodlarda ise PascalCase kullanılıyordu. Sonunda enum isimlendirme ve property’lerini PascalCase yapmaya karar verdik.

• **Fonksiyon Tanımlama:** Daha önce arrow function kullanıyorduk, fakat hizalama sorunları nedeniyle declaration function kullanmaya başladık. Ancak geliştiriciler bu yeni standarda geçmekte zorlanıyordu. Örneğin, onMounted hook’unda kullanılan fonksiyonlar arrow olarak tanımlanıyordu ve hizalama kurallarını bozuyordu.

• **Vue.js Standartları:** Vue.js 3 ile birlikte composition API ve TypeScript kullanmaya başladık. Ancak bazı geliştiriciler hala options API veya defineComponent ile çalışıyordu.

• **Vue componentleri Blok Sıralaması:** Kodlar “script”, “template”, “style” sırasıyla geliştirilmeye başlanmıştı. Buna rağmen bazıları hala template bloğunu en üste koyuyordu.

• **Props İsimlendirme:** Propslar genelde camelCase ile yazılıyordu, fakat bazı yerlerde kebab-case kullanılıyordu.

**Standartların Belirlenmesi ve Uygulanması**

Bu sorunları çözmek için, belirlediğim standartları ve kuralları içeren dokümantasyonlar hazırladım ve takımla paylaştım. Ancak, dokümantasyonların okunmadığını ve standartlara yeterince önem verilmediğini fark ettim. Bu, code review süreçlerinde sürekli aynı geri bildirimleri vermeme neden oluyordu.

**Çözüm: Eslint, Prettier ve Lint-Staged**

Dokümantasyonların yeterince etkili olmadığını fark edince şu adımları izledim:

1\. **Eslint Kuralları:** Dokümantasyondaki standartlara uygun olarak Eslint kurallarını yapılandırdım.

2\. **Kod Formatlama:** Kod formatlama sorunları için .editorconfig ve .prettierrc dosyalarını yapılandırdım.

3\. **Lint-Staged Kullanımı:** Eski kodların tamamını düzeltmek mümkün olmadığından, lint-staged aracını kullanarak sadece commitlenecek geliştirmelere odaklandım. Bu, geliştirmelerin daha tutarlı olmasını sağladı.

**Build ve Demo Sorunları**

Code review ve demo sırasında sıkça karşılaşılan bir başka sorun, kodların build olmamasıydı. Typescript hataları veya unit testlerin çalışmaması nedeniyle deployment sırasında hatalar fark ediliyordu. Bu durumda kod tekrar inprogress’e gönderiliyordu ve iki kere bakmak durumunda kalıyorduk. Bunun önüne geçmek için:

• Lint-staged aracını, commit öncesinde unit test ve type checking yapacak şekilde yapılandırdım. Böylece, sorunlar daha commit aşamasında tespit edilip çözülüyordu.

---

Front End takımı olarak yaşadığımız sorunları ve bunları nasıl çözdüğümü genel hatlarıyla anlattım. Burada paylaştığım standartlar tamamen takımımızın ihtiyaçlarına göre belirlenmiştir. Her ekibin kendi ihtiyaçlarına göre farklı standartlar belirlemesi gerekebilir.

**Proje Yapılandırma dosyaları**

https://gist.github.com/mustafadalga/3b026306bae517c2d72ffa884185171c
