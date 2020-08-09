<script>
  import { createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";

  const dispatch = createEventDispatcher();
</script>

<style lang="scss">
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.6);
    .modal {
      background: white;
      padding: 20px;
      border-radius: 20px;
    }
  }
</style>

<div class="modal-bg" transition:fade>
  <div class="modal" transition:fly={{ y: -100 }}>
    <button
      on:click={() => {
        dispatch('close');
      }}>
      Close
    </button>
    <slot />
  </div>
</div>
