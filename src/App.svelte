<script>
  import { fly, fade, slide } from 'svelte/transition';
  import MovieSearch from './lib/Search.svelte';

  let showMovieSearch = false;
  let loading = true;
  let rambo = false;

  const handleMouseenter = () => (rambo = true);
  const handleMouseleave = () => (rambo = false);

  function toggleMovieSearch() {
    showMovieSearch = !showMovieSearch;
    loading = showMovieSearch;
    if (showMovieSearch) setTimeout(() => (loading = false), 2000);
  }
</script>

<svelte:body
  on:mouseenter={handleMouseenter}
  on:mouseleave={handleMouseleave}
/>

<img class:showing={rambo} alt="Rambo" src="/images/rambo.png" />

<label class="toggle">
  <input
    type="checkbox"
    on:change={toggleMovieSearch}
    bind:checked={showMovieSearch}
  />
  <span>Click</span>
</label>

{#if showMovieSearch}
  <div transition:fade={{ duration: 1000 }}>
    {#if loading}
      <div class="loading">Lataillaan...</div>
    {:else}
      <h1 in:fly={{ y: 200, duration: 2000 }}>Elokuvat</h1>
      <h2 in:fade={{ duration: 1500 }}>Hae elokuvia mitä haluat katsoa</h2>
      <div in:slide={{ duration: 1000 }}>
        <MovieSearch />
      </div>
    {/if}
  </div>
{/if}

<style>
  .toggle {
    position: absolute;
    top: 20px;
    left: 20px;
    font-size: 0.8em;
    display: flex;
    align-items: center;
    transform: scale(2);
    margin-right: 10px;
  }

  .loading {
    font-size: 2em;
    text-align: center;
    margin-top: 100px;
  }

  img {
    position: absolute;
    left: 0;
    bottom: -60px;
    transform: translate(-80%, 0) rotate(-30deg);
    transform-origin: 100% 100%;
    transition: transform 0.4s;
  }

  .showing {
    transform: translate(-15%, 0) rotate(0deg);
  }

  :global(body) {
    overflow: hidden;
    animation: moveBackground 40s linear infinite;
    background: linear-gradient(150deg, #009fff, #ec2f4b, #009fff, #ffff00);
    background-size: 600% 600%;
  }

  @keyframes moveBackground {
    from {
      background-position: 0 0;
    }
    to {
      background-position: 100% 0;
    }
  }
</style>
