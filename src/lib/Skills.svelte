<script>
  import { data } from './utils/store';
  import ToggleContentBtn from './utils/ToggleContentBtn.svelte';
  import { fly } from 'svelte/transition';
</script>
<div id="skills-container" class="font-secondary">
  <span class='inline-block mb-3'>I have over 6 years experience as a front end developer. Prior to that I was in Architecture where I did 3D modelling. I'm now adding motion design to my skillset.</span>
  <ToggleContentBtn classList='mb-2'>
    <button slot="button" class="flex justify-center justify-self-end items-center mr-1 text-sm font-bold p-2  border-2 border-black text-black uppercase" >tech＋</button>
    <div slot="content" transition:fly={{ y: -500, duration: 250 }}>
      {#each $data.skills as skills}
        <div class="flex my-1">
          <p class="flex justify-center items-center mr-1 text-sm w-20 bg-black text-white uppercase">{skills.label}</p>
          <p class="flex flex-wrap w-full h-auto pt-1">
            {#each skills.item.split(',') as item }
              <span class="text-xs border px-1 min-w-min max-w-max h-4 py-0 box-content mr-1 mb-1">
                {item}
              </span>
            {/each}
          </p>
        </div>
      {/each}
    </div>
  </ToggleContentBtn>
  <ToggleContentBtn>
    <button slot="button" class="flex justify-center justify-self-end items-center mr-1 text-sm font-bold p-2  border-2 border-black text-black uppercase" >design＋</button>
    <div slot="content" transition:fly={{ y: -500, duration: 250 }}>
      <ul>
        <li>
          <ul>
            <li>Programs</li>
            <li>Sketch</li>
            <li>After Effects</li>
            <li>Illustrator</li>
          </ul>
        </li>
        <li>
          <ul>
            <li>Design Skills</li>
            <li>Prototyping</li>
          </ul>
        </li>
      </ul>
    </div>
  </ToggleContentBtn>
</div>