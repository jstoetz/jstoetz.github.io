<body>
   

 <div class="header">
  <h2>Data Science for Fun (and Profit!)</h2>
  
</div>

<div class="row">
  <div class="leftcolumn">
    <div class="card">
      <h2>Blog</h2>
      <h5>Read All Blog Posts</h5>
      <ul>
      {% for post in site.posts %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
      </li>
      {% endfor %}
    </ul>

    </div>
 </div>
  <div class="rightcolumn">
    <div class="card">
      <h2>About Me</h2>
      <div class="fakeimg" style="height:100px;">Image</div>
      <p>Some text about me in culpa qui officia deserunt mollit anim..</p>
    </div>
    <div class="card">
      <h3>More Information</h3>
      <div><a href="/R-Programming"><img src="/assets/img/web-programming.svg" alt="R Programming">R Programming</a></div><br>
      <div><a href="/Data-Science"><img src="/assets/img/data-mining.svg" alt="Data Science">Data Science</a></div><br>
      <div><a href="/Real-Estate"><img src="/assets/img/building.svg" alt="Building">Real Estate</a></div>
    </div>
    <div class="card">
      <h3>Follow Me</h3>
      <p><a href="https://twitter.com/jstoetz"><img src="/assets/img/hollow-cut-twitter.svg"></a></p>
      <p><a href="https://facebook.com/jstoetz"><img src="/assets/img/hollow-cut-facebook.svg"></a></p>
      <p><a href="https://www.linkedin.com/in/jstoetz/"><img src="/assets/img/hollow-cut-linkedin.svg"></a></p>
      <p><a href="https://www.github.com/jstoetz"><img src="/assets/img/hollow-cut-github.svg"></a></p>
    </div>
  </div>
</div>

<div class="footer">
  <h2>Footer</h2>
</div>
