<script lang="ts">
    import { alert } from "@nativescript/core";
    let data: any;
    let temperature: any;
    let apiKey = "1b8c44c2195c3e02989ed66724e777af";
async function getWeather(){
      const url = `https://api.openweathermap.org/data/2.5/weather?q=Paris&appid=${apiKey}`;
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
        },
      };
      try {
        const response = await fetch(url, options);
        data = await response.json();
        temperature = data.main.temp - 273,15;
        let roundedTemperature = Math.floor(temperature);
          console.log("TEMPERATURE = ", roundedTemperature);
          alert(`Temperature: ${roundedTemperature}°c`)
        return data.results || [];
      } catch (error) {
        console.error(error);
        return [];
      }
    }

    getWeather();

</script>