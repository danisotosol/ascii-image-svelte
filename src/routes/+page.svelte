<script>

  import "./page.css";
  import { onMount } from 'svelte';
  let imagePathh = "../static/dnk.jpg";
  let asciiArt = '';
  let nameValue = '';

  function handleFileChange(event) {
    nameValue = event.target.value;
  }

  function handleFileUpload() {;
    let imagePath = nameValue;
    console.log(nameValue);

    let scaleFactor = 0.3;

    const imageToAscii = async (img) => {
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      
      const newWidth = Math.ceil(img.width * 0.5); // Round up to ensure we don't lose detail
      const newHeight = Math.ceil(img.height * 0.25);

      canvas.width = newWidth;
      canvas.height = newHeight;
      ctx.drawImage(img, 0, 0, newWidth, newHeight);

      const imageData = ctx.getImageData(0, 0, newWidth, newHeight).data;

      let asciiString = '';
      for (let i = 0; i < imageData.length; i += 4) {
        const grayscale = imageData[i] * 0.3 + imageData[i + 1] * 0.59 + imageData[i + 2] * 0.;
        const asciiChar = '#;:-=+*#$@'.charAt(Math.floor(grayscale / 255 * 10));
        asciiString += asciiChar;
        if ((i / 4 + 1) % newWidth === 0) {
          asciiString += '\n';
        }
      }
      asciiArt = asciiString;
    };

    const img = new Image();
    img.crossOrigin = 'Anonymous';
    img.src = imagePath;
    imageToAscii(img);

    onMount(async () => {
      await new Promise((resolve) => {
        img.onload = () => resolve(img);
      });
    });
    
  }


</script>

<h1>ASCII Art Generator</h1>
<h2>Image:</h2>
<img src={imagePathh} alt="xd" style="max-width: 100%; max-height: 400px;"> <!-- Ensure the image doesn't exceed a certain size -->
<h2>ASCII:</h2>
<pre>{asciiArt}</pre>
<form>
  <input type="file" bind:value={nameValue} on:change={handleFileChange} />
</form>
<button on:click={handleFileUpload}>Upload</button>