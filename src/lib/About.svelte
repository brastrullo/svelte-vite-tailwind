<script>
  import { data } from './utils/store';
  import ScheduleMe from './ScheduleMe.svelte';
  export let sectionsLoaded;

  sectionsLoaded.update(() => true);
</script>
<article class="prose">
  {#if $data}
    <p>this is the about article</p>
    <p class="mt-2 mb-0">Interests</p>
    <div class="mb-2">
      {#each $data.interests as interests}
        <span class="inline-block border border-solid border-black rounded-sm bg-white mr-1 mb-1 p-1 lowercase text-xs whitespace-nowrap">{interests}</span>
      {/each}
    </div>
    <ScheduleMe />
  {/if}
</article>