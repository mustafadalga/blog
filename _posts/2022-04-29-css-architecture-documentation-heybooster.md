---
layout: post
title: "CSS Architecture Documentation | Heybooster"
date: 2022-04-29 11:54:09 +0000
slug: css-architecture-documentation-heybooster
tags: [css3, css-frameworks, css, css-animation, css-architecture]
lang: en
---


I have created a document to set a standard for writing CSS code at [Heybooster](https://heybooster.ai/). I also wanted to share this documentation as open source.

* We use SCSS as CSS Preprocessing.
    
* Our CSS architecture is generally based on the [BEM](http://getbem.com/). methodology. However, in some cases, we may be able to use OOCSS or SMACSS as alternative solutions.
    
* We create and use utility classes for frequently used css properties.
    
* We write [component-scoped CSS](https://vuejs.org/api/sfc-css-features.html#scoped-css) . Thus , Our SCSS/CSS codes will apply to elements of the current components only.
    

### Class Naming Rules

For naming rules, we used the following resources:

* [https://philipwalton.com/articles/css-architecture/](https://philipwalton.com/articles/css-architecture/)
    
* [https://gist.github.com/necolas/1309546](https://gist.github.com/necolas/1309546)
    

We usually follow the BEM methodology to naming the template classes:

class-name { }  
class-name--modifier-name { }  
class-name\_\_child-element { }  
class-name\_\_child-element--modifier-name { }

We use prefix by scope to associate classes:

\# Layout Rules  
l-auth  
l-app

\# State Rules  
is-urgent  
is-perfect  
is-open  
is-opportunity

\# View Rules  
v-dashboard  
v-my-insight  
v-kpi-management

\# Component Rules  
c-modal  
c-create-kpi  
c-update-kpi  
c-button

\# Popup Rules  
p-add-new-member  
p-delete-kpi  
p-request-an-insight

\# Non-styled JavaScript Hooks  
js-gridjs-pages  
js-search-input  
js-current-page

*Originally published at* [*heybooster.*](https://insight.heybooster.ai/css-architecture-documentation/)
