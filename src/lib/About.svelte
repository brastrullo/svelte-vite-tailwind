<script>
  import ScheduleMe from './ScheduleMe.svelte';
  export let data;
  export let sectionsLoaded;

  sectionsLoaded.update(() => true);
</script>
<article class="prose">
  {#if $data}
    <p>this is the about article</p>
    <p>Interests</p>
    {#each $data.interests as interests}
      <span class="border border-solid border-black rounded-sm bg-white mr-1 mb-1 p-1 lowercase text-xs">{interests}</span>
    {/each}
    <ScheduleMe />
  {/if}
</article>