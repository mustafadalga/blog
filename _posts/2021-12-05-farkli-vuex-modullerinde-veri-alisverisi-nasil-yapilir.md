---
layout: post
title: "Farklı vuex modüllerinde veri alışverişi nasıl yapılır"
date: 2021-12-05 07:32:08 +0000
slug: farkli-vuex-modullerinde-veri-alisverisi-nasil-yapilir
tags: [vuejs, modules, vuex, state-management, vue3]
lang: tr
---

Bir [önceki](https://medium.com/heybooster/vuex-on-large-projects-how-to-structure-it-78c45c2b3f16) blog yazısında büyük proje’lerde vuex modüllerinin kullanılmasının öneminden bahsetmiştik.

Bu blog yazısında ise bu vuex modülleri arasında nasıl veri alışverişi yapabileceğimizi anlatacağım.

Vuex modüllerinde işlemler yaparken aşağıdaki gibi senaryolar ile karşılabiliriz:

*   Actions içerisinde başka bir modülün state’lerine erişip değişkenlerini kullanma
*   Actions içerisinde başka bir modüldeki verileri değiştirmek, silmek veya eklemek için bu modülün mutations’larına kullanma
*   Mutation içerisinde başka bir modülün state’lerini kullanma
*   Mutation içerisinde başka bir modülün mutationlarını kullanma
*   Getters içerisinde başka bir modülün state’lerini kullanma
*   Getters içerisinde başka bir modülün getters’lerini kullanma

Yukarıdaki senaryoları örnekler üzerinde uygulayacağız.

#### **Actions içerisinde başka bir modülün state’lerine erişip değişkenlerini kullanma**

Bir filmi bookmark’e eklemek isteğimizde movies modülündeki **addMovieBookmark** action’unu kullanmaktayız.Filmi bookmark’e eklemek için movieID ile email gerekmektedir.Fakat kullanıcının email adresi **auth** modülünde tutulmaktadır. Bu api isteğini yapabilmek için auth modülündeki state’lere erişerek email adresine almamız gerekmektedir. auth modülündeki email adresine erişmek için rootState kullanıyoruz.

**rootState** ile farklı bir modüldeki state’lere erişebiliriz.

@/modules/movies/actions.js

#### **Actionsların başka bir modüldeki verileri değiştirmek,silmek veya eklemek için bu modülün mutationlarına kullanma**

Movies modülündeki addMovieBookmark action’unu kullanarak bir filmi bookmark’e eklerken bir hata ile karşılaştığında kullanıcıya bir uyarı veya hata mesajı göstermemiz gerekiyor. Hata veya uyarı ile ilgili bilgileri de **notifications** modülünde tutmaktayız.

Dolayısıyla notification modülündeki **setNotification** mutation’unu kullanarak yeni bir notification oluşturacağız.

Aşağıda görüldüğü gibi notification modülündeki setNotification mutation’una erişmek için commit’te 3. bir arguman olarak **{ root: true }** ekledik.

@/modules/movies/actions.js

#### **Mutationların içerisinde başka bir modülün state’lerini kullanma**

Movies modülündeki addMovieBookmark action’unu kullanarak bir filmi bookmark’e ekledikten sonra aynı zamanda vuexteki bookmarks durumunu değiştirmemiz gerekiyor.

Filmin bookmark durumunu değiştirmek için **changeMovieBookmarkStatus** mutation’unu kullanmaktayız. Bir filmin bookmark durumunu vuex’ten değiştirmek için bookmark’e eklenmiş filmin email adresi ile kullanıcının email adresinin aynı olup olmadığını kontrol etmemiz gerekiyor.

Dolayısıyla **changeMovieBookmarkStatus** mutation’una o kullanıcının email adresini de göndermemiz gerekiyor.

@/modules/movies/actions.js

**changeMovieBookmarkStatus** mutation’una filme eklenen bookmark email ve kullanıcının email adresini auth modülünden alarak gönderdik.

@/modules/movies/mutations.js

#### Mutations’ların içerisinde başka bir modülün mutationlarını kullanma

changeMovieBookmarkStatus mutation’unu kullanarak bir filmin bookmark durumunu değiştirmek istediğimizde eğer ki bu filmin email adresi ile kullanıcının email adresi farklı ise bookmark durumunu vuex’ten değiştirmememiz gerekiyor ve aynı zamanda kullanıcıya bir uyarı mesajı göstermemiz gerekiyor.

Fakat uyarı mesajları ile ilgili bilgiler notifications modülünde tutulmamaktadır.Dolayısıyla changeMovieBookmarkStatus içerisinden notification modülündeki setNotification mutation’unu kullanmamız gerekiyor.

Dolayısıyla setNotification modülüne erişebilmek için de bir arguman olarak commit’i **changeMovieBookmarkStatus**’a göndermemiz gerekiyor.

@/modules/movies/actions.js

changeMovieBookmarkStatus mutation’unda data.commit diyerek setNotification mutation’unu kullandık.

@/modules/movies/mutations.js

#### Getters içerisinde başka bir modülün state’lerini kullanma

Bir getters methodunda başka bir modülün state’lerini kullanmak için getters methodumuza 2 argument daha eklememiz gerekiyor: **getters** ve **rootState**

Aşağıdaki örnekteki gibi 2 argumanı ekledikten sonra rootState ile auth modülüne erişerek kullanıcının email adresini aldık ve işledik.

@/modules/movies/getters.js

#### Getters içerisinde başka bir modülün getters’lerini kullanma

Kullanıcının profilinin gösterildiği sayfa’da kullanıcının bilgilerini ve favori filmlerini göstermek istiyoruz. Bunu işlemi yapabilmek için movies ve auth modüllerindeki bilgileri kullanmamız gerekiyor.

Bir getters methodunda başka bir modülün getters’lerini kullanmak için getters methodumuza 3 argument daha eklememiz gerekiyor: **getters**, **rootState** ve **rootGetters**

Aşağıdaki örnekteki gibi getters, rootState ve rootGetters argumanlarını ekledikten sonra rootGetters ile movies modülündeki getBookmarkedMovies eriştik ve getters methodunu kullandık.

@/modules/auth/getters.js

Bu blog yazısı ile ilgili olarak hazırladığım örnek proje’yi [github](https://github.com/mustafadalga/vuex-on-large-projects) hesabımda, [demo](https://vuex-on-large-projects.netlify.app/)’yu da netlify’da yayınladım..
