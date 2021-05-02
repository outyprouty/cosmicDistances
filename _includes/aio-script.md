<script>
  window.onload = function() {
    var lesson_sections = [
    {% for section in site.sections %}
    "{{ section.url}}"{% unless forloop.last %},{% endunless %}
    {% endfor %}
    ];
    var xmlHttp = [];  /* Required since we are going to query every section. */
    for (i=0; i < lesson_sections.length; i++) {
      xmlHttp[i] = new XMLHttpRequest();
      xmlHttp[i].section = lesson_sections[i];  /* To enable use this later. */
      xmlHttp[i].onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
          var article_here = document.getElementById(this.section);
          var parser = new DOMParser();
          var htmlDoc = parser.parseFromString(this.responseText,"text/html");
          var htmlDocArticle = htmlDoc.getElementsByTagName("article")[0];
          article_here.innerHTML = htmlDocArticle.innerHTML;
        }
      }
      var section_url = "{{ relative_root_path }}" + lesson_sections[i];
      xmlHttp[i].open("GET", section_url);
      xmlHttp[i].send(null);
    }
  }
</script>
{% comment %}
Create an anchor for every section.
{% endcomment %}
{% for section in site.sections %}
<article id="{{ section.url }}"></article>
{% endfor %}
