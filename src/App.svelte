<script>
  import { fade } from "svelte/transition";
  import cookie from "./assets/cookie.svg";

  let thinking = false;

  let fortune = "";
  let numbers = [];
  let theme = "";

  async function getFortune() {
    thinking = true;

    const res = await fetch("https://apollo.ncp.nathanferns.xyz/fortune");
    const data = await res.json();

    fortune = data.fortune;
    numbers = data.numbers;
    theme = data.theme;

    thinking = false;
  }
</script>

<main class="mx-10 my-5">
  <h1 class="text-lg font-bold text-blue-500">delphi</h1>

  {#if fortune === ""}
    {#if thinking}
      <h2 class="text-lg mb-5 animate-pulse">the oracle is thinking...</h2>
    {:else}
      <h2 class="text-lg mb-5">click the cookie to get your daily fortune</h2>

      <button
        on:click={getFortune}
        class="flex items-center justify-center w-40 h-40 p-5 bg-neutral-100 rounded-full shadow-lg"
      >
        <img src={cookie} class="w-40" alt="Fortune Cookie" />
      </button>
    {/if}
  {:else}
    <div transition:fade>
      <h2 class="text-lg">the oracle has spoken</h2>

      <h2 class="text-2xl font-bold">
        <span class="text-blue-500">"</span>{fortune}<span class="text-blue-500"
          >"</span
        >
      </h2>

      <div class="flex flex-wrap mt-5">
        {#each numbers as number}
          <div
            class="flex items-center justify-center w-10 h-10 mr-2 mb-2 text-lg font-bold text-blue-500 bg-neutral-100 rounded-md shadow-md"
          >
            {number}
          </div>
        {/each}
      </div>

      <h2 class="mt-8">come back tomorrow for another unique fortune</h2>
    </div>
  {/if}
</main>
