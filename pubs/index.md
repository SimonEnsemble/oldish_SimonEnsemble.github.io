---
layout: default
---

<style>
#left {
  width: 150px;
  float: left;
  padding-right: 0px;
}
#right {
  margin-left: 175px;
  /* Change this to whatever the width of your left column is*/
}
.clear {
  clear: both;
}
</style>

<center>
    <img style="width:150px;" src="images/cover_1.png" alt="">
    <img style="width:150px;" src="images/cover_2.png" alt="">
    <img style="width:150px;" src="images/cover_3.png" alt="">
    <img style="width:150px;" src="images/cover_4.png" alt="">
</center>

<a href ="https://scholar.google.com/citations?user=eoR8MNMAAAAJ&hl=en`">Google Scholar</a>

# publications


{% for work in site.data.publications %}
  <div id="container">
 <div id="left">
        <center> 
            <img style="width:150px;" src="{{ work.image }}">
        </center>
    </div>
    <div id="right">
        <b>{{ forloop.rindex }}.</b>
        <quotations>❝</quotations>{{ work.title }}<quotations>❞</quotations><br>
        {{ work.authors }}.<br>
        <i>{{ work.journal }}</i>. ({{ work.date }}) <a href="{{ work.url }}">DOI</a>. {{ work.other }}
        <br>
    </div>
    <div class="clear"></div>
  </div>
  <hr>
{% endfor %}
