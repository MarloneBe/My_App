<script lang="ts">
    import Activities from "./Activities.svelte";
    import { navigate } from "svelte-native";
    import Weather from "./Weather.svelte";
    import Photos from "./Photos.svelte";
    
  </script>
  
  <page>
    <actionBar title="Home" class="action-bar" />
    <scrollView>
      <stackLayout class="container">
        <textView editable={false} class="welcome-text">
          Welcome to WAM, your personal activity finder
        </textView>
        <button text="Get activities nearby" on:tap={() => navigate({ page: Activities })} class="button-primary" />
        <button text="Get the weather" on:tap={() => navigate({page: Weather})} class="button-primary" />
        <button text="photos" on:tap={() => navigate({page: Photos})} class="button-primary" />
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
    .welcome-text {
      font-size: 18px;
      color: #333;
      margin-bottom: 20px;
      text-align: center;
    }
    .button-primary {
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
  </style>
  
  