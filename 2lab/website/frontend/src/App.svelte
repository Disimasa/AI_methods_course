<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Raleway:wght@400;800&family=Roboto&display=swap" rel="stylesheet">

<script>
  import { Pulse } from 'svelte-loading-spinners'
  import axios from 'axios'

  const apiUrl = 'https://api-inference.huggingface.co/models/sberbank-ai/mGPT'
  const apiToken = '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'

  let loading = false;
  let resultsVisible = false;
  let result = ''
  let textBeginning = ''
  function generateText() {
    loading = true;
    fetch(
        apiUrl,
        {
          headers: { Authorization: `Bearer ${apiToken}` },
          method: "POST",
          body: JSON.stringify(textBeginning),
        }
    )
    .then(response => response.json())
    .then(data => {
      result = data[0].generated_text
      loading = false
      resultsVisible = true
    })
    .catch(error => {
      alert(error.error)
    })
  }

</script>
<main>
  <h2>Генерация текста с помощью Sber GPT3</h2>
  <textarea
      class="text-input"
      placeholder="Введите начало текста"
      bind:value={textBeginning}
  />
  <button id="submit-button" on:click={generateText}>Отправить</button>
  {#if loading}
    <Pulse color="#282828"/>
  {:else if resultsVisible}
    <h2>Результат</h2>
    <textarea class="text-input" bind:value={result}></textarea>
  {/if}
</main>


<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: start;
    min-height: 90vh;
  }

  .text-input {
    width: 100%;
    min-height: 100px;
  }

  #submit-button {
    background-color: #282828;
    font-family: 'Raleway', sans-serif;
    font-weight: 400;
    color: #FFFFFF;
    border-radius: 8px;
    padding: 20px 45px;
    margin: 20px auto;
  }
</style>