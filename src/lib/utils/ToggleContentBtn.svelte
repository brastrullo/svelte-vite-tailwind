<script>
  export let classList = '';
  export let show = false;
  export let callback = (args) => console.log('no callback provided');
  const toggle = () => {
    show = !show;
    callback(show);
  };
</script>
<div class={`${classList}`}>
  <div on:click={toggle}>
    <slot name="button">
      Provide Button
    </slot>
  </div>
  {#if show}
    <slot name="content">Provide content</slot>
  {/if}
</div>