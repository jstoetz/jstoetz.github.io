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

    <div class="card">
      <h2>TITLE HEADING</h2>
      <h5>Title description, Sep 2, 2017</h5>
      <div class="fakeimg" style="height:200px;">Image</div>
      <p>Some text..</p>
      <p>Sunt in culpa qui officia deserunt mollit anim id est laborum consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco.</p>
    </div>
  </div>

  <div class="rightcolumn">
    <div class="card">
      <h2>About Me</h2>
      <div class="fakeimg" style="height:100px;">Image</div>
      <p>Some text about me in culpa qui officia deserunt mollit anim..</p>
    </div>
    <div class="card">
      <h3>Areas of Interest</h3>
      <a href="/R-Programming"><img src="/assets/img/web-programming.svg" style="width:64px;height:64px;"><a><br>
      R-Programming<br>
      <a href="/Data-Science"><img src="/assets/img/data-mining.svg" style="width:64px;height:64px;"><a><br>
      Data Science<br>
      <a href="/Real-Estate"><img src="/assets/img/building.svg" style="width:64px;height:64px;"><a><br>
      Real Estate<br>
    </div>

    <div class="card">
      <h3>Follow Me</h3>
      <a href="https://facebook.com/jstoetz"><img src="/assets/img/hollow-cut-facebook.svg" style="width:32px;height:32px;"><a>
      <a href="https://github.com/jstoetz"><img src="/assets/img/hollow-cut-github.svg" style="width:32px;height:32px;"><a>
      <a href="https://www.linkedin.com/in/jstoetz/"><img src="/assets/img/hollow-cut-linkedin.svg" style="width:32px;height:32px;"><a>
      <a href="https://twitter.com/jstoetz"><img src="/assets/img/hollow-cut-twitter.svg" style="width:32px;height:32px;"><a>
    </div>
  </div>


<div class="footer">
  <h2>Footer</h2>
</div>
