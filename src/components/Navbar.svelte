<script>
  import { Router, Link } from 'svelte-navigator';
</script>

<Router>
  <nav class="nav">
    <div class="navbar">
      <Link to="/" class="logo">assistant</Link>
      <Link to="/todo">Todo</Link>
    </div>
  </nav>
</Router>
