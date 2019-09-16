
<div class="row">
  <div class="leftcolumn">
    <div class="card">
      <h2>Latest Posts</h2>
       <ul>
        {% for post in site.posts %}
          <li>
           <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
           <p>{{ post.excerpt }}</p>
           </li>
            {% endfor %}
        </ul>
    </div>

  </div>
  <div class="rightcolumn">
    <div class="card">
      <a href="/about"><h2>About Me</h2><a>
      <a href="/about"><img src="/assets/img/pic.jpg" style="height:100px;">jstoetz</div><a>
      <p>Learn more about the goals of this site.</p>
    </div>
    <div class="card">
      <h3>Areas of Interest</h3>
      <a href="/R-Programming"><img src="/assets/img/web-programming.svg" style="width:64px;height:64px;"><a><br>
      R-Programming<br>
      <a href="/Data-Science"><img src="/assets/img/data-mining.svg" style="width:64px;height:64px;"><a><br>
      Data Science<br>
      <a href="/Real-Estate"><img src="/assets/img/building.svg" style="width:64px;height:64px;"><a><br>
      Real Estate<br>

    <div class="card">
      <a href="https://facebook.com/jstoetz"><img src="/assets/img/hollow-cut-facebook.svg" style="width:32px;height:32px;"><a>
      <a href="https://github.com/jstoetz"><img src="/assets/img/hollow-cut-github.svg" style="width:32px;height:32px;"><a>
      <a href="https://www.linkedin.com/in/jstoetz/"><img src="/assets/img/hollow-cut-linkedin.svg" style="width:32px;height:32px;"><a>
      <a href="https://twitter.com/jstoetz"><img src="/assets/img/hollow-cut-twitter.svg" style="width:32px;height:32px;"><a>
    
