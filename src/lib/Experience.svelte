<script>
  import { data } from './utils/store';
  import { writable } from 'svelte/store';
  import { onMount } from 'svelte';
  import { fly, fade } from 'svelte/transition';
  import Chevron from './icons/Chevron.svelte';
  import CounterClockwise from './icons/CounterClockwise.svelte';
  import InfoCircle from './icons/iCircle.svelte';
  import debounce from 'lodash.debounce';
  import throttle from 'lodash.throttle';
  let hscrollContainer;
  let containerRect;
  let currentIndex;
  let reloading = writable(false);

  const objectArr = writable([]);
  export const closeAllDetails = () => {
    objectArr.update((arr) => {
      arr.forEach((obj) => {
        obj.show = false;
      });
      return arr;
    });
  };

  const toggleDetails = (itemIndex) => {
    objectArr.update((arr) => {
      return arr.slice().map((item, i) => {
        return i === Number(itemIndex) ? { ...item, show: !item.show } : item;
      });
    });
    console.log({ $objectArr });
  };

  const reloadPanels = throttle(() => {
    console.log('asdfss', {$reloading, currentIndex})
    if (!$reloading) {
      reloading.set(true)
      console.log({$reloading, hscrollContainer})
      hscrollContainer.scrollTo({
        left: 0
      });
      currentIndex = 0;
      if (currentIndex !== $objectArr.length) {
        setTimeout(() => {
          reloading.set(false);
          console.log({$reloading})
        }, 250);
      }
    }
  }, 500, { leading: false, trailing: true } )

  const callback = (entries, observer) => {
    entries.forEach((entry) => {
      const isIntersecting = entry.isIntersecting;
      if (isIntersecting) {
        currentIndex = Number(entry.target.dataset.index);
        if (currentIndex === $objectArr.length) {
          // FIX: sometimes runs twice (2 entries: scroll in/out)
          reloadPanels()
        }
      }
    });
  };

  let observer = new IntersectionObserver(callback, {
    root: document.querySelector('#hscroll'),
    rootMargin: '0px',
    threshold: 0.5,
  });

  const resizeEvent = () => {
    if (hscrollContainer) {
      containerRect = hscrollContainer.getBoundingClientRect();
      console.log('resize', containerRect);
    }
  };

  const imgPos = ['right top', 'center', 'right top', 'center', 'left top'];
  const isLastCarouselPosition = (i) =>
    i !== null && i === $data.work.length - 1;

  const scrollToOrigin = () => {
    hscrollContainer.scrollTo({
      left: 0
    });
  }

  const slideImgRight = () => {
    hscrollContainer.scrollBy({
      left: containerRect.width,
      behavior: 'smooth',
    });
  };
  const slideImgLeft = () => {
    hscrollContainer.scrollBy({
      left: -containerRect.width,
      behavior: 'smooth',
    });
  };

  const handleClickOnDetails = (e) => {
    toggleDetails(currentIndex);
  };

  $: if (hscrollContainer) {
    containerRect = hscrollContainer.getBoundingClientRect();
  }

  onMount(() => {
    objectArr.set($data.work.map((obj, i) => ({ id: i, show: false })));
    document.querySelectorAll('.hscroll-item').forEach((target) => {
      observer.observe(target);
    });
    window.addEventListener(
      'resize',
      debounce(resizeEvent, 200, { leading: false })
    );
  });
</script>

<div class="relative">
  <div
    bind:this={hscrollContainer}
    on:click={handleClickOnDetails}
    id="hscroll"
    class="hscroll-container overflow-y-hidden overflow-x-scroll"
  >
    {#if $reloading}
      <div transition:fade={{duration: 400}} class={`reload-panel absolute z-50 justify-center align-middle inline-block h-full w-full border-box`}>
        <article
          data-index={$objectArr.length}
          class="flex flex-col justify-between cursor-pointer bg-gray-200 opacity-50 text-left h-full rounded-md p-4 "
        >
          <CounterClockwise
            width="8rem"
            height="8rem"
            className="animate-spin text-gray-600 m-[auto]"
          />
        </article>
      </div>
    {/if}
    {#each $data.work as work, i}
      <div
        data-index={i}
        class={`container-${i} relative justify-start align-top inline-block h-full border-box mr-2 px-4 ${$reloading ? 'opacity-0' : ''}`}
      >
        <article
          data-index={i}
          style={`background: linear-gradient(
            rgba(0, 0, 0, 0.25),
            rgba(0, 0, 0, 0.25)
          ), url(${work.main_img.url}) no-repeat ${imgPos[i]} / cover`}
          class="hscroll-item flex flex-col justify-between cursor-pointer text-left bg-gray-200 rounded-md p-4 "
        >
          <div class="main-text">
            <p class="text-3xl whitespace-normal text-white">{work.position}</p>
            <span class="text-sm text-gray-200">{work.company}</span>
          </div>
          <div style={`background: no-repeat center url(${work.sub_img.url})`} class={`logo-${i} animate-pulse absolute inset-0 m-[auto] max-h-1/2 w-4/5 drop-shadow opacity-60 hover:opacity-80`}/>
          {#if $objectArr.length > 0 && $objectArr[i].show}
            <div
              transition:fly={{ y: 500 }}
              class="text-container bg-white rounded-lg bg-opacity-90 px-6 py-4 relative bottom-0 cursor-default z-0"
            >
              <div class="flex flex-col my-2">
                {#each work.details as details}
                  <p class="whitespace-normal text-sm text-left my-2">
                    {details.item}
                  </p>
                {/each}
              </div>
              <ul class="flex flex-row flex-wrap">
                {#each work.skills as skill}
                  <li
                    class="cursor-pointer text-xs border border-blue-700  text-blue-700 rounded-sm px-1 mr-1 mb-1 z-20"
                  >
                    {skill}
                  </li>
                {/each}
              </ul>
              {#if work.otherSkills.length > 0}
                <ul class="flex flex-row flex-wrap">
                  {#each work.otherSkills as skill}
                    <li
                      class="cursor-pointer text-xs border border-indigo-700  text-indigo-700 rounded-sm px-1 mr-1 mb-1 z-20"
                    >
                      {skill}
                    </li>
                  {/each}
                </ul>
              {/if}
            </div>
          {/if}
        </article>
      </div>
    {/each}
    <div data-index={$objectArr.length} class="blank-panel hscroll-item justify-start align-top inline-block h-full border-box">
      <article
        data-index={$objectArr.length}
        class="flex flex-col justify-between cursor-pointer bg-gray-300 text-left h-full rounded-md p-4 "
      >
      <div class="animate-pulse duration-75 text-gray-200">
            <div class="bg-gray-200 h-8 w-20 mb-2 rounded-lg">.</div>
            <div class="bg-gray-200 h-6 w-10 rounded-lg">.</div>
          </div>
      </article>
    </div>
  </div>
  <span
    class="toggle-details-text flex flex-row items-center align-text-bottom align-items text-xs text-gray-400 absolute -bottom-6 left-0"
    ><InfoCircle colour="gray" />Click to toggle details</span
  >
  {#if currentIndex > 0 && currentIndex < $objectArr.length}
    <button
      on:click={slideImgLeft}
      class="absolute h-19 w-11 top-1/2 -ml-4 rounded-r-lg left-0 p-2 bg-gray-700 bg-opacity-20 hover:bg-opacity-50 text-opacity-10"
      ><Chevron
        direction="left"
        className="text-opacity-40 text-white"
      /></button
    >
  {/if}
  {#if isLastCarouselPosition(currentIndex)}
    <button
      on:click={reloadPanels}
      class="absolute h-19 w-11 top-1/2 -mr-4 rounded-l-lg right-0 p-2 bg-gray-700 bg-opacity-20 hover:bg-opacity-50 text-opacity-10"
    >
      <CounterClockwise
        height="3rem"
        width="2rem"
        className="text-opacity-40 text-white"
      />
    </button>
  {:else if (currentIndex < $objectArr.length)}
    <button
      on:click={slideImgRight}
      class="absolute h-19 w-11 top-1/2 -mr-4 rounded-l-lg right-0 p-2 bg-gray-700 bg-opacity-20 hover:bg-opacity-50 text-opacity-10"
    >
      <Chevron direction="right" className="text-opacity-40 text-white" />
    </button>
  {/if}
</div>

<style>
  .hscroll-container {
    white-space: nowrap;
    width: calc(100vw - (2 * 1rem));
    scroll-snap-type: x mandatory;
    scroll-snap-type: mandatory;
  }

  .hscroll-item {
    height: calc(100vh - 3rem - 6rem);
    width: calc(100vw - (2 * 1rem));
    scroll-snap-align: center;
    scroll-snap-stop: always;
    font-family: 'Open Sans', sans-serif;
    z-index: -10; /* makes .text-container unclickable 'z-0' */
  }
  .toggle-details-text {
    font-family: 'Open Sans', sans-serif;
  }
</style>
