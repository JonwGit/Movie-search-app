<script>
  export let Open;
  export let close;
</script>

{#if Open}
  <div class="backdrop" on:click={close}>
    <div class="modal" on:click|stopPropagation>
      <slot />
    </div>
  </div>
{/if}

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  .modal {
    background: white;
    padding: 1rem;
    border-radius: 8px;
    width: 50%;
    max-height: 80vh;
    overflow-y: auto;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
</style>
