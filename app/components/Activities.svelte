<script lang="ts">
    import { navigate } from "svelte-native";
    import Home from "./Home.svelte";
    import Details from "./Details.svelte";
    import { onMount } from "svelte";
  
    let data: any;
    let name: string;
    let activityID: string;
    interface nameAndID {
  nom: string;
  id: string;
}

let nameAndID: nameAndID[] = [];

onMount(() => {
  getActivities();
})
    async function getActivities() {
      const url = "https://api.foursquare.com/v3/places/nearby";
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization: "fsq37/YXCNjIFB/YFIvHo3cioUWGe5oV0CyKKIw0jGH6sc8=",
        },
      };
      try {
        const response = await fetch(url, options);
        data = await response.json();
        name = data.results.map((item: { name: any }) => item.name);
        activityID = data.results.map((item: {fsq_id: any}) => item.fsq_id);
        
        for(let i = 0; i < name.length; i++){
          nameAndID.push({ nom: name[i], id: activityID[i]});
        }
        console.log(" NAME ET ID == ", nameAndID)
        return data.results || [];
      } catch (error) {
        console.error(error);
        return [];
      }
    }
    
  
  </script>
  
  <page>
    <actionBar title="Activities" class="action-bar" />
    <scrollView>
      <stackLayout class="container">
        <button text="Home menu" on:tap={() => navigate({ page: Home })} class="button-primary" />
        {#if data}
          {#each nameAndID as NameID}
            <button class="list-names" text={NameID.nom} on:tap={() => navigate({page: Details, props: {id: NameID.id}})} />
          {/each}
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
      align-items: center;
      background-color: beige;
    }
    .button-primary, .list-names {
      width: 90%;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 5px;
      font-size: 16px;
      text-align: center;
    }
    .button-primary {
      background-color: #0071c2;
      color: white;
    }
    .list-names {
      background-color: #f1f1f1;
      color: #333;
      border: 1px solid #ddd;
    }
  </style>
  