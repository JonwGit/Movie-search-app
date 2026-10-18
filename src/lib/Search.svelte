<script>
  import { createEventDispatcher } from 'svelte';
  import Modal from './Modal.svelte';
  import { searchResults } from './store.js';

  let searchQuery = '';
  let showModal = false;
  let errorMessage = '';

  const dispatch = createEventDispatcher();

  const apiKey = 'YOUR-API-KEY';

  async function fetchData() {
    searchResults.set([]);

    if (!searchQuery) {
      errorMessage = 'Anna hakusana.';
      showModal = true;
      return;
    }

    const url = `https://api.themoviedb.org/3/search/multi?api_key=${apiKey}&query=${encodeURIComponent(searchQuery)}&media_type=movie,tv`;

    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error('Elokuvien ja sarjojen haku epäonnistui.');
      }
      const data = await response.json();
      searchResults.set(
        data.results.filter(
          (item) => item.media_type === 'movie' || item.media_type === 'tv'
        )
      );
      showModal = $searchResults.length > 0;
      if ($searchResults.length > 0) {
        dispatch('searchResultsReady');
      }
    } catch (error) {
      errorMessage = error.message;
      showModal = true;
    }
  }

  function closeModal() {
    showModal = false;
    errorMessage = '';
  }
</script>

<div>
  <input
    type="text"
    bind:value={searchQuery}
    placeholder="Etsi elokuvia ja sarjoja"
  />
  <button on:click={fetchData}>Etsi</button>
</div>

<Modal Open={showModal} close={closeModal}>
  {#if $searchResults.length > 0}
    {#each $searchResults as item}
      <h3>{item.title || item.name}</h3>
      <img
        src={`https://image.tmdb.org/t/p/w500${item.poster_path || item.profile_path}`}
        alt={` ${item.title || item.name}`}
      />
      <p>{item.overview}</p>
    {/each}
  {:else}
    <p>{errorMessage}</p>
  {/if}
</Modal>
