<script>
  import { requestPermissions, takePicture } from '@nativescript/camera';
  import { ImageSource } from '@nativescript/core';
  import { shareImage } from '@nativescript/social-share';

  let cameraImage;

  async function requestCameraPermissions() {
    try {
      const granted = await requestPermissions();
      if (granted) {
        console.log('ça marche');
      } else {
        console.log('ça marche pas');
      }
    } catch (error) {
      console.error('Erreur de permission', error);
    }
  }

  async function captureImage() {
    try {
      const image = await takePicture();
      const source = new ImageSource();
      await source.fromAsset(image);
      cameraImage = source;
    } catch (error) {
      console.error('Erreur de capture', error);
    }
  }

  function share() {
    if (cameraImage) {
      shareImage(cameraImage);
    } else {
      console.log('Capture annulée');
    }
  }

  requestCameraPermissions();
</script>

<page>
    <actionBar title="Capture image" />

    <stackLayout>
      <button text="Capture Image" on:tap={captureImage} />
      {#if cameraImage}
        <image src={cameraImage} />
        <button text="Share image" on:tap={share} />
      {/if}
    </stackLayout>
</page>
