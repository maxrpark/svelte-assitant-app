<!-- script -->
<script>
  // imports
  import axios from 'axios';
  import { onMount } from 'svelte';
  import ProjectCard from '../components/ProjectCard.svelte';

  // variables
  let url = 'https://assitant-app.netlify.app/api/projects-api';
  let projects = [];
  let title = 'Loading...';

  onMount(async () => {
    try {
      const response = await axios.get(url);
      projects = await response.data;
      // console.log(data);
      title = 'Assitant';
    } catch (error) {
      title = 'Assitant';
    }
  });
</script>

<!-- template -->
<div class="section">
  <h2 class="title">{title}</h2>
  <h3 class="title">Same App with different frameworks or languages</h3>
  <div class="container">
    {#each projects as project (project.id)}
      <ProjectCard {project} />
    {/each}
  </div>
</div>

<!-- styles -->
<style>
</style>
