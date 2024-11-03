<script lang="ts">
    import Home from './Home.svelte';
    import { navigate } from 'svelte-native';
    import { onMount } from 'svelte';
    import { shareViaTwitter } from '@nativescript/social-share'
    import Activities from './Activities.svelte';
    import App from '~/App.svelte';
    
    export let id: string;
    let dataDetails: any;
    let dataPhotos: any;
    let dataTips: any;
    let infos: any;
    let name: any;
    let photos: string[] = [];
    let result: string;
    let tips: any;
    let condition = 0;

    onMount(() =>{
      getActivityDetails();
      getActivityPhotos();
      getActivityTips();
    })


    async function getActivityDetails(){
      const url = `https://api.foursquare.com/v3/places/${id}`;
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization: "fsq37/YXCNjIFB/YFIvHo3cioUWGe5oV0CyKKIw0jGH6sc8=",
        },
      };
      try {
        const response = await fetch(url, options);
        dataDetails = await response.json();
        let test = [dataDetails];
        name = test.map((item: {name: any}) => item.name);
        infos = test.map((item: {location: any}) => item.location);
        return dataDetails.results || [];
      } catch (error) {
        console.error(error);
        return [];
      }
    }

    async function getActivityPhotos(){
      const url = `https://api.foursquare.com/v3/places/${id}/photos`;
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization: "fsq37/YXCNjIFB/YFIvHo3cioUWGe5oV0CyKKIw0jGH6sc8=",
        },
      };
      try {
        const response = await fetch(url, options);
        dataPhotos = await response.json();
        let prefix: string[] = dataPhotos.map((item: {prefix: any}) => item.prefix);
        let suffix: string[] = dataPhotos.map((item: {suffix: any}) => item.suffix);
        let size = "200x200";
        let photoList;
        if (dataPhotos){
          if(dataPhotos.length == 1){
            photoList = prefix + size + suffix;
          }
          else{

            for (let i = 0; i < dataPhotos.length; i++)
            {
              photoList = dataPhotos.slice(1).map((_: any, i: any) => prefix[i + 1]+ size + suffix[i + 1]);
            }
            for (let i = 0; i < photoList.length; i++)
            {
              photos.push(photoList[i]);
            }
          }
          let laPhoto;
          if (photos.length !== 0){
            laPhoto = photos[0].split("//");
            result = laPhoto[0] + '//' + laPhoto.slice(1).join('/');
            condition = 0;
          }
          else{
            console.log("PHOTO LOSTO == ", photoList)
            laPhoto = photoList.replace(/\/\//g, "/");
            photos.push(laPhoto);
            condition = 1;
          }
          console.log("le résultat == ", result);
          return dataPhotos.results || [];
        }
      } catch (error) {
        console.error(error);
        return [];
      }
    }

    async function getActivityTips(){
      const url = `https://api.foursquare.com/v3/places/${id}/tips`;
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization: "fsq37/YXCNjIFB/YFIvHo3cioUWGe5oV0CyKKIw0jGH6sc8=",
        },
      };
      try {
        const response = await fetch(url, options);
        dataTips = await response.json();
        tips = dataTips.map((item: {text: any}) => item.text);
        return dataTips.results || [];
      } catch (error) {
        console.error(error);
        return [];
      }
    }

    function share(){
      shareViaTwitter("Je vais à " + name + " au " + infos[0].address + " à " + infos[0].locality + " dans le " + infos[0].postcode +" apparement c'est pas mal.")
    }

  </script>
  
  <page>
    <actionBar title="Details" class="action-bar" />
    <scrollView>
      <stackLayout class="container">
        <button text="Back to activities" on:tap={() => navigate({ page: Activities })} class="button-primary" />
        {#if dataDetails}
          <textView editable={false} text={name} class="name-text"></textView>
          {#if dataPhotos}
            <gridLayout columns="*, *, *">
              {#each photos as photo, index}
                <image src={photo} class="photo" col="{index % 3}" row="{Math.floor(index / 3)}" />
              {/each}
            </gridLayout>
          {/if}
          {#each infos as info}
            <stackLayout class="info-card">
              <textView editable={false}>
                <formattedString>
                  <span text={info.formatted_address} />
                </formattedString>
              </textView>
            </stackLayout>
          {/each}
          {#if dataTips}
            <textView editable={false} text="Tips:" class="tips-title"></textView>
            {#each tips as tip}
              <stackLayout class="tip-card">
                <textView editable={false}>
                  <span text={tip} />
                </textView>
              </stackLayout>
            {/each}
          {/if}
          <button text="share on twitter" on:tap={() => share()} />
        {/if}
      </stackLayout>
    </scrollView>
  </page>
  
  <style>
    .action-bar {
      background-color: #003580;
      color: white;
      font-weight: bold;
      text-align: center;
    }
    .container {
      padding: 20px;
      background-color: beige;
    }
    .button-primary {
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 5px;
      font-size: 16px;
    }
    .button-primary {
      background-color: #0071c2;
      color: white;
    }
    .info-card {
      padding: 15px;
      margin-top: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
      background-color: white;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .info-card textView {
      font-size: 14px;
      line-height: 20px;
      color: #333;
    }
    .tip-card {
    padding: 15px;
    margin-top: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: white;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .tip-card textView {
    font-size: 14px;
    line-height: 20px;
    color: #333;
  }
  .photo {
    margin-top: 20px;
    margin-bottom: 40px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .name-text {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  .tips-title {
    font-size: 16px;
    font-weight: bold;
    margin-top: 20px;
  }
  </style>
  
