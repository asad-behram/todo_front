<script>
  import date from 'date-and-time';
  const now = new Date();
  const dateTime = date.format(now, 'ddd, MMM DD YYYY HH:mm')
</script>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <a href="/">
    <span class="navbar-brand">TODO LIST</span>
  </a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavDropdown">
    <ul class="navbar-nav">
      <li class="nav-item active">
        <span class="nav-link" href=""> {dateTime} <span class="sr-only">(current)</span></span>
      </li>
    </ul>
  </div>
</nav>