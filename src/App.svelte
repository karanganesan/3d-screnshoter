<script>
  let files = [];

  const handleFiles = (e) => {
    Array.from(e.target.files).forEach((file) => {
      files = [...files, file];
    });
  };

  const downloadURI = (uri, name) => {
    var link = document.createElement("a");
    link.download = name;
    link.href = uri;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  };

  const handleDownload = () => {
    files.forEach((file) => {
      const image = document
        .getElementById(file.name)
        .toDataURL("image/png", 1);
      downloadURI(image, file.name.split(".")[0] + "_front.png");

      document.getElementById(file.name).addEventListener(
        "camera-change",
        () => {
          setTimeout(() => {
            const image = document
              .getElementById(file.name)
              .toDataURL("image/png", 1);
            downloadURI(image, file.name.split(".")[0] + "_back.png");
          }, 2000);
        },
        { once: true }
      );
      document
        .getElementById(file.name)
        .setAttribute("camera-orbit", "-180deg 90deg 105%");
    });
  };
</script>

Select 3D models to screenshot
<input multiple type="file" accept=".glb" on:change={handleFiles} />

<br />

Save screenshots
<button on:click={handleDownload}>Download</button>

<br />

{#each files as file}
  <p>{file.name}</p>
  <model-viewer
    environment-image="neutral"
    camera-orbit="0deg 90deg 105%"
    src={URL.createObjectURL(file)}
    id={file.name}
  />
{/each}

<svelte:head>
  <script
    type="module"
    src="https://unpkg.com/@google/model-viewer/dist/model-viewer.min.js"></script>
</svelte:head>

<style>
  model-viewer {
    width: 500px;
    height: 500px;
  }
</style>
