<script>
  import { fade } from 'svelte/transition';
</script>

<!-- HTML -->
<div
  class="modal-container"
  transition:fade={{ duration: 500 }}>
  <div class="modal-content">
    <slot />
  </div>
</div>
