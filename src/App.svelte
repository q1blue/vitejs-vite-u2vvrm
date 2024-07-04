<script>
  import { onMount } from 'svelte';
  import lunr from 'lunr';

  let index;
  let query = '';
  let results = [];

  const documents = [
    {
      id: 1,
      title: 'Example Document 1',
      body: 'This is an example document about Svelte and Lunr.js',
    },
    {
      id: 2,
      title: 'Example Document 2',
      body: 'This is another example document about Svelte and Lunr.js',
    },
  ];

  onMount(() => {
    index = lunr(function () {
      this.field('title');
      this.field('body');
      this.ref('id');

      documents.forEach((doc) => {
        this.add(doc);
      });
    });
  });

  function search() {
    if(query !== ''){
      results = index.search(query).map((result) => {
      return documents.find((doc) => doc.id === parseInt(result.ref));
    });
    } else{
      results = []
    }
    
  }
</script>

<div>
  <input bind:value={query} on:input={search} />

  <ul>
    {#each results as result}
      <li>{result.title}</li>
    {/each}
  </ul>
</div>
