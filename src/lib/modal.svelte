<script>
  import { createEventDispatcher } from 'svelte'
  import { fade, fly } from 'svelte/transition'
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props} */
  let { children } = $props();

  const dispatch = createEventDispatcher()
</script>

<div class="modal-bg" transition:fade>
  <div class="modal" transition:fly={{ y: -100 }}>
    <button
      onclick={() => {
        dispatch('close')
      }}
    >
      Close
    </button>
    {@render children?.()}
  </div>
</div>
