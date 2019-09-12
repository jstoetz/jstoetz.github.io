<!DOCTYPE html>
<html lang="{{ site.lang | default: "en-US" }}">
  <head>
    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

{% seo %}
    <link rel="stylesheet" href="{{ "/assets/css/style.css?v=" | append: site.github.build_revision | relative_url }}">
    <link rel="stylesheet"
          href="https://fonts.googleapis.com/css?family=Montserrat">
    <link href="https://fonts.googleapis.com/css?family=Lato:700|Roboto:500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="/assets/css/fixed-navigation-bar.css">
    <link rel="stylesheet" href="/assets/css/home-page.css">
    <!--[if lt IE 9]>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.3/html5shiv.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <nav class="fixed-nav-bar">
      <div id="menu" class="menu">
        <a class="sitename" href="/">@jstoetz</a>
        <!-- Example responsive navigation menu  -->
          <a class="show" href="#menu">Menu</a><a class="hide" href="#hidemenu">Menu</a>
            <ul class="menu-items">
              <li><a href="/blog/">Blog</a></li>
              <li><a href="/about/">About</a></li>
              <li><a href="https://twitter.com/jstoetz">Twitter</a></li>
              <li><a href="https://www.facebook.com/jstoetz">Facebook</a></li>
          </ul>
      </div>
    </nav>

      <footer>
        {% if site.github.is_project_page %}
        <p>This project is maintained by <a href="{{ site.github.owner_url }}">{{ site.github.owner_name }}</a></p>
        {% endif %}
        <p><small>Hosted on GitHub Pages &mdash; Theme by <a href="https://github.com/orderedlist">orderedlist</a></small></p>
      </footer>

    <script src="{{ "/assets/js/scale.fix.js" | relative_url }}"></script>
    {% if site.google_analytics %}
    <script>
      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
      (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
      m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
      })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
      ga('create', '{{ site.google_analytics }}', 'auto');
      ga('send', 'pageview');
    </script>
    {% endif %}
  </body>
</html>
