---
layout: libros

---
<div class="small-wrapper">
  <div class="about-container">
    <section class="about-header">
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
