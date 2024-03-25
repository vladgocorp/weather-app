<script>
	//declaration of variables
	let city = '';           //stores the name of the city entered       
	let data;                //Stores weather data obtained from the API
	let error;               //Stores error messages in case of problems during the API request
	let currentTime = '';    //Stores the current time
	let loading = false;     ////Indicates if the application is in the process of loading data               
  
	async function handleSearch() {                              // Function to manage the weather search by city
	  try {
		loading = true;
		const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=4c61a0f86d0a3ac879f92f471e4c3329`);
		if (!response.ok) {
		  throw new Error('error getting data');
		}
		data = await response.json();
		error = null;
	  } catch (error) {
		console.error(error);
		error = error.message;
	  } finally {
		loading = false;
	  }
	}
	  function getCurrentTime() {                                 //Function to get the current time and update it every second
    const now = new Date();
    const hours = now.getHours().toString().padStart(2, '0');
    const minutes = now.getMinutes().toString().padStart(2, '0');
    const seconds = now.getSeconds().toString().padStart(2, '0');
    currentTime = `${hours}:${minutes}:${seconds}`;
  {

}
	}
	getCurrentTime();                                               // Get the current time when the page loads
  setInterval(getCurrentTime, 1000);                                // Update time every second
  </script>
  
<main>
    <div class="container">                  <!--main container-->
        <div class="content">                <!--contend-->
            <div class="search-box">         <!--search box--> 
                <input type="text" id="city" bind:value={city} placeholder="City" style="text-align: center;">     <!--city ​​entrance area-->
                <button on:click={handleSearch}>Search</button>    <!-- Search button -->
				
            </div>
            
            {#if loading}              <!-- Show loading message -->
                <p>Loading...</p>      
            {:else}
                {#if error}            <!-- Show error message -->
                    <p style="color: red">{error}</p>
                {:else}                
                    {#if data}         <!-- Show weather data -->
                        <div class="weather-info">
							<img src={`https://openweathermap.org/img/wn/${data.weather[0].icon}.png`} alt="Weather icon">    <!-- Weather icon -->
							<p>Temperature</p>            <!--temperature-->
							<p>{Math.round(data.main.temp - 273.15)}°C</p>
							<p>City: {data.name}</p>      <!--city name-->
                            <p>Description: {data.weather[0].description}</p>         <!--description weather-->
							<p>Current Time: {currentTime}</p>       <!--current time -->
                            
                        </div>
                    {/if}
                {/if}
            {/if}
        </div>
    </div>
</main>

<style>
	/* General styles for the main content */
    main {  
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    /* Styles for the main container */
    .container {
        background-color: #9dd1ff;
        border-radius: 50px;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        padding: 60px;
        max-width: 500px;
        width: 400%;
    }
    /* Styles for the content inside the container */
    .content {
        text-align: center;
    }
    /* Styles for the search box */
    .search-box {
        margin-bottom: 20px;
    }
    /* Styles for the city input field */
    .search-box input {
        padding: 10px;
        border: 2px solid #ccc;
        border-radius: 5px;
        width: 70%;
    }
    /* Styles for the search button */
    .search-box button {
        padding: 10px 20px;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }
    /* Styles for the search button on hover */
    .search-box button:hover {
        background-color: #45a049;
    }
    /* Styles for the weather information section */
    .weather-info {
        margin-top: 20px;
    }
</style>
