<!-- App.svelte -->
<script>
  import { Router, Route } from 'svelte-navigator';

  // Components
  import Navbar from './components/Navbar.svelte';
  import Footer from './components/Footer.svelte';

  // views
  import Home from './views/Home.svelte';
  import Todo from './views/Todo.svelte';
</script>

<Router primary={false}>
  <div>
    <Navbar />
    <Route path="/">
      <Home />
    </Route>
    <Route path="/todo/">
      <Todo />
    </Route>
    <Footer />
  </div>
</Router>
