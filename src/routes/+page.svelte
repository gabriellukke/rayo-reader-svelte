<!-- App.svelte -->
<script lang="ts">
  let wordsPerMinute = 300;
  let isPlaying = false;
  let text = "";
  let wordIndex = 0;
  let words = [] as string[];
  let timer: NodeJS.Timeout;
  let error: string | null = null;
  
  function startApp() {
    if (text.trim() === "") {
      error = "Please enter some text to play";
      return;
    }

    if (wordsPerMinute < 1) {
      error = "Words per minute should be greater than 0";
      return;
    }

    error = null;

    words = text.split(" ");
    isPlaying = true;
    wordIndex = 0;
    displayWords();
  }

  function displayWords() {
    if (wordIndex < words.length) {
      text = words[wordIndex];
      wordIndex += 1;
      const timePerWord = 60000 / wordsPerMinute; // Time per word in milliseconds
      timer = setTimeout(displayWords, timePerWord);
    } else {
      isPlaying = false;
    }
  }

  function stopApp() {
    clearTimeout(timer);
    isPlaying = false;
  }

  function configureSettings() {
    // Todo
  }
</script>

<main class="mx-auto max-w-4xl p-4">
  <h1 class="text-2xl font-bold mb-4">Rayo Reader</h1>

  <div class="mb-4">
    {#if !isPlaying}
      <textarea
        rows="6"
        class="w-full p-2 border border-gray-300 rounded focus:outline-none focus:ring focus:border-blue-300"
        placeholder="Enter your text here"
        bind:value={text}
      ></textarea>
      {#if error}
        <p class="text-red-500 text-sm mt-2"> {error} </p>
      {/if}
    {/if}
  </div>

  <div class="mb-4">
    {#if !isPlaying}
      <div class="flex flex-col md:flex-row">
        <div class="md:w-1/2">
          <div class="mb-4">
            <label for="wpmInput" class="block text-sm font-medium text-gray-700">Words per Minute</label>
            <input
              type="number"
              id="wpmInput"
              min="100"
              max="1000"
              class="w-16 p-2 border border-gray-300 rounded focus:outline-none focus:ring focus:border-blue-300"
              bind:value={wordsPerMinute}
            />
          </div>
        </div>
        <div class="md:w-1/2">
          <div class="mt-4">
            <button
              class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
              on:click={isPlaying ? stopApp : startApp}
            >
              {#if isPlaying}
                Stop
              {:else}
                Play
              {/if}
            </button>
          </div>
          <div class="mt-4">
            <button
              class="px-4 py-2 bg-gray-300 text-gray-700 rounded hover:bg-gray-400"
              on:click={configureSettings}
            >
              Configure
            </button>
          </div>
        </div>
      </div>
    {/if}
  </div>

  {#if isPlaying}
    <p class="text-5xl font-semibold text-center"> {text} </p>
  {/if}
</main>
