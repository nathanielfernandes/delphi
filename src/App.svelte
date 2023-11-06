<script>
  import { fade } from "svelte/transition";
  import cookie from "./assets/cookie.svg";

  let thinking = false;

  let fortune = "";
  let numbers = [];
  let theme = "";

  let image_url = "";

  async function getFortune() {
    thinking = true;

    const res = await fetch("https://apollo.ncp.nathanferns.xyz/fortune");
    const data = await res.json();

    fortune = data.fortune;
    numbers = data.numbers;
    theme = data.theme;

    thinking = false;

    if (data["image"]) {
      image_url = data["image"];
      return;
    }

    const image_res = await fetch(
      "https://apollo.ncp.nathanferns.xyz/fortune/image"
    );
    const url = await image_res.json();

    image_url = url;
  }
</script>

<main class="m-4 sm:m-10 bg-white p-4 sm:p-6 rounded-lg drop-shadow-lg">
  <h1 class="text-lg font-bold text-blue-500">delphi</h1>

  {#if fortune === ""}
    {#if thinking}
      <h2 class="text-lg mb-5 animate-pulse">the oracle is thinking...</h2>
    {:else}
      <h2 class="text-lg mb-5">click the cookie to get your daily fortune</h2>

      <button
        on:click={getFortune}
        class="flex items-center px-4 py-2 rounded-md drop-shadow-lg bg-blue-500 text-white font-bold justify-between w-60 hover:bg-blue-400 active:bg-blue-600 transition-all duration-100"
      >
        <div>Generate Fortune</div>
        <img src={cookie} class="w-10" alt="Fortune Cookie" />
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
            class="flex items-center justify-center w-10 h-10 mr-2 mb-2 text-lg font-bold text-blue-500 bg-slate-100 rounded-md shadow-md"
          >
            {number}
          </div>
        {/each}
      </div>

      {#if image_url === ""}
        <div class="mt-5 animate-pulse text-lg">Generating image...</div>
      {:else}
        <div class="mt-5">
          <img src={image_url} class="w-full rounded-md" alt={fortune} />
        </div>
      {/if}

      <h2 class="mt-5 text-sm text-neutral-700">
        come back tomorrow for another unique fortune
      </h2>
    </div>
  {/if}
</main>

<style>
  :global(body) {
    margin: auto;
    max-width: 800px;
    background-color: #f1f5f9;
  }
</style>
