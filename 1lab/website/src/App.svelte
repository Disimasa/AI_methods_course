<script>
  import { EveryPixelApi, FacePlusPlusApi, PresentIdApi } from './_adapters'
  import FileInput from './lib/FileInput.svelte'
  import { Pulse } from 'svelte-loading-spinners'
  import { onMount } from 'svelte'
  let files = [];
  let image = {};

  let faces = [];
  let loading = false;
  let resultsVisible = false;

  let selectedApi = 1
  let apis = [
    { id: 0, text: 'Every Pixel Api', api: new EveryPixelApi() },
    { id: 1, text: 'Face Plus Plus Api', api: new FacePlusPlusApi() },
    { id: 2, text: 'Present Id Api', api: new PresentIdApi() }
  ]

  function sendFile() {
    loading = true;
    document.getElementById('result-window').scrollIntoView({block: 'end', behavior: 'smooth'});
    apis[selectedApi].api.findFacesInFile(files[0]).then(facesFrames => showResults(facesFrames))
  }

  function showResults(facesFrames) {
    faces = facesFrames
    showImage(files[0])
  }

  function handleClick() {
    if (!files[0]) {
      alert('Сначала выберите файл')
      return
    }
    sendFile();
  }

  function showImage(file) {
    const reader = new FileReader()
    reader.onload = () => {
      image.src = reader.result
    }
    resultsVisible = true
    loading = false
    reader.readAsDataURL(file)
  }

  function calculateFrame(face) {
    return `left: ${face.left}px; top: ${face.top}px; width: ${face.width}px; height: ${face.height}px`
  }
</script>
<main>
  <div class="first-view">
    <div style="display: flex; align-items: center">
      <h2>Детекция лиц с помощью сервиса</h2>
      <select class="api-select" bind:value={selectedApi}>
        {#each apis as apiOption}
          <option value={apiOption.id}>
            {apiOption.text}
          </option>
        {/each}
      </select>
    </div>
      <FileInput
          width="100%"
          label="Загрузите файлы (jpg, png)"
          icon="/download.svg"
          bind:files={files}
      />

    <button id="submit-button" on:click={handleClick}>Отправить</button>
  </div>
  <div id="result-window">
    {#if loading}
      <Pulse color="#282828"/>
    {:else if resultsVisible}
      <div id="result-block">
        <img id="result-img" bind:this={image} src="" alt="Result" />
        {#each faces as face}
          <div class="face-frame" style="{calculateFrame(face)}"></div>
        {/each}
      </div>
    {/if}
  </div>
</main>
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Raleway:wght@400;800&family=Roboto&display=swap" rel="stylesheet">
<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .first-view {
    height: 90vh;
  }

  input {
    display: none;
  }

  label {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  label:hover {
    cursor: pointer;
  }

  label p {
    font-family: 'Raleway', sans-serif;
    font-weight: 400;
    font-size: 20px;
    color: #313131;
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
  #submit-button:hover {
    cursor: pointer;
  }

  #statistics-block h1 {
    font-family: Raleway;
    margin: 0;
  }

  #result-block {
    position: relative;
    width: fit-content;
    height: fit-content;
  }

  #result-block img {
    z-index: 9;
  }

  #result-block canvas {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 10;
  }

  .face-frame {
    position: absolute;
    border: 3px solid #24fc03;
  }

  .api-select {
    font-size: 20px;
    max-height: 40px;
    margin-left: 10px;
    padding: 5px 10px;
  }

  #result-window {
    min-height: 75vh;
  }
</style>