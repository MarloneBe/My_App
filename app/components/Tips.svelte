<script lang="ts">
    import Home from './Home.svelte';
    import { navigate } from 'svelte-native';
    import { AnyBulkWriteOperation } from 'mongodb';

    let tips: any;
    let data: any;
    async function getActivityTips(){
      const url = "https://api.foursquare.com/v3/places/4bcf3d6c462cb713354fd607/tips";
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
        tips = data.map((item: {text: any}) => item.text);
          console.log("DETAILS = ", data);
        return data.results || [];
      } catch (error) {
        console.error(error);
        return [];
      }
    }

  </script>

<page>
  <actionBar title="Tips" class="action-bar" />
  <scrollView>
    <stackLayout class="container">
      <button text="To Home directly" on:tap={() => navigate({ page: Home })} class="button-primary" />
      <button text="Get activity tips" on:tap={getActivityTips} class="button-secondary" />
      {#if data}
        {#each tips as tip}
          <stackLayout class="tip-card">
            <textView editable={false} text={tip}></textView>
          </stackLayout>
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
  }
  .button-primary, .button-secondary {
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
    font-size: 16px;
  }
  .button-primary {
    background-color: #0071c2;
    color: white;
  }
  .button-secondary {
    background-color: #febb02;
    color: white;
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
</style>