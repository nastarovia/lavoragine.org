---
layout: default-with-header
title: libros
permalink: /libros/
---
<div class="small-wrapper">
  <div class="about-container">
    <section class="about-header">
        <div class="author-image-container">
            <img src="{{site.baseurl}}/assets/img/{% if site.author_photo %}{{site.author_photo}}{% endif %}" alt="{{site.author}}">
        </div>
        <div class="text-center">
            <a href="{{site.resume_url}}"> Descarga nuestro catálogo</a>
            </div>
    </section>
    <section class="about-body">
        {{ site.user_description_short | markdownify }}
        {{ site.user_description_appendix | markdownify }}
    </section>
  </div> <!-- End About Container -->
</div> <!-- End Small Wrapper -->
