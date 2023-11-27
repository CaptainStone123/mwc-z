<template>
  <div>
    <h1>Hello User!</h1>
  </div>
</template>

<script>
export default {
  mounted() {
    this.startNotification();
  },
  methods: {
    startNotification() {
      if ('Notification' in window) {
        if (Notification.permission !== 'denied') {
          this.requestNotificationPermission();
        }
      } else {
        console.log('This browser does not support desktop notification');
      }
    },
    requestNotificationPermission() {
      if (Notification.permission !== 'granted') {
        Notification.requestPermission().then(permission => {
          if (permission === 'granted') {
            this.sendNotification();
          }
        });
      } else {
        this.sendNotification();
      }
    },
    sendNotification() {
      if (Notification.permission === 'granted') {
        const notification = new Notification('Hi there!');
      }
      setTimeout(() => {
        this.sendNotification();
      }, 10000); // Sending notification every 10 seconds
    }
  }
};
</script>


<!-- <script>
import axios from 'axios';

export default {
  data() {
    return {
      latestData: {
        temperatura: null,
        humedad: null,
        co2: null,
        timestamp: null
      },
      maxProgress: 100,
      maxProgressCo2: 1000,
      progressColorTemperature: "#fdc500",
      progressColorHumidity: "#2596be",
      progressColorCo2: "#a1a8a7",
      radius: 54,
      temperatureStatus: "Normal",
      humidityStatus: "Normal", 
      co2Status: "Normal" 
    };
  },
  mounted() {
    this.fetchLatestData();
    // Poll for new data every 5 seconds (adjust the interval as needed)
    this.pollingInterval = setInterval(this.fetchLatestData, 5000);
    this.startNotification();
  },
  beforeDestroy() {
    // Clear the interval when the component is destroyed
    clearInterval(this.pollingInterval);
  },
  computed: {
    circumference() {
      return 2 * Math.PI * this.radius;
    },
    progressOffsetTemperature() {
      if (this.latestData.temperatura < 18) {
      this.temperatureStatus = "Low Temp";
    } else if (this.latestData.temperatura >=85 && this.latestData.temperatura < 24) {
      this.temperatureStatus = "Normal Temp";
    } else if (this.latestData.temperatura >= 24) {
      this.temperatureStatus = "High Temp";
    }
      return (this.latestData.temperatura / this.maxProgress) * this.circumference;
    },
    progressOffsetHumidity() {
    if (this.latestData.humedad < 85) {
      this.humidityStatus = "Low Humidity";
    } else if (this.latestData.humedad >= 85 && this.latestData.humedad < 95) {
      this.humidityStatus = "Normal Humidity";
    } else if (this.latestData.humedad >= 95) {
      this.humidityStatus = "High Humidity";
    }
      return (this.latestData.humedad / this.maxProgress) * this.circumference;
    },
    progressOffsetCo2() {
     if (this.latestData.co2 < 500) {
        this.co2Status = "Low Level";
      } else if (this.latestData.co2 >= 500 && this.latestData.co2 <= 800) {
        this.co2Status = "Normal Level";
      } else if (this.latestData.co2 >= 800) {
        this.co2Status = "High Level";
      }
      return (this.latestData.co2 / this.maxProgressCo2) * this.circumference;
    },
  },
  methods: {
    startNotification() {
      setInterval(() => {
        this.sendNotification();
      }, 10000); 
    },
    sendNotification() {
      if (Notification.permission === "granted") {
        const notification = new Notification("Hi there!");
      } else if (Notification.permission !== "denied") {
        Notification.requestPermission().then(permission => {
          if (permission === "granted") {
            const notification = new Notification("Hi there!");
          }
        });
      }
    },
    async fetchLatestData() {
      try {
        const response = await axios.get('https://mws-finals.vercel.app/latestData');
        this.latestData = response.data;
        console.log('Latest Data:', this.latestData);
      } catch (error) {
        console.error('Error fetching latest data:', error);
      }
    }
  }
};
</script>

<template>
  <section>
    <nav class="nav">
      <ul>
          <li>
          <img class="logo" src="../assets/mw_logo.png" alt="">
          <span><RouterLink to="/home" >Home </RouterLink> </span>
          <span><RouterLink to="/harvest" > Harvest</RouterLink></span>
         </li>
         <span><RouterLink to="/" >Log out</RouterLink> </span>
      </ul>
    </nav>
    <div class="body">
 
    <div class="data-display-container">
      DEVICE 1
      <section class="progress-container">
        <span class="label">
          <p>Temperature:</p>  <p>{{ temperatureStatus }}</p> 
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetTemperature" :style="{ stroke: progressColorTemperature }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.temperatura }}°C</div>
        </div>
      </section>

      <section class="progress-container">
        <span class="label">
          <p>Humidity:</p> <p>{{ humidityStatus }}</p>
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetHumidity" :style="{ stroke: progressColorHumidity }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.humedad }}%</div>
        </div>
      </section>

      <section class="progress-container">
        <span class="label">
          <p>Carbon Dioxide:</p> <p>{{co2Status}}</p>
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetCo2" :style="{ stroke: progressColorCo2 }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.co2 }} <br> PPM</div>
        </div>
      </section>
    </div>

    <br><br>

    <div class="data-display-container">
      DEVICE 2
      <section class="progress-container">
        <span class="label">
          <p>Temperature:</p>  <p>{{ temperatureStatus }}</p> 
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetTemperature" :style="{ stroke: progressColorTemperature }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.temperatura }}°C</div>
        </div>
      </section>

      <section class="progress-container">
        <span class="label">
          <p>Humidity:</p> <p>{{ humidityStatus }}</p>
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetHumidity" :style="{ stroke: progressColorHumidity }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.humedad }}%</div>
        </div>
      </section>

      <section class="progress-container">
        <span class="label">
          <p>Carbon Dioxide:</p> <p>{{co2Status}}</p>
        </span>
        <div class="progress-ring-wrapper">
          <svg class="progress-ring" width="120" height="120">
            <circle
              class="progress-overlay" cx="60" cy="60" r="54" stroke-width="12"/>
            <circle
              class="progress-circle"
              :stroke-dasharray="circumference" :stroke-dashoffset="circumference - progressOffsetCo2" :style="{ stroke: progressColorCo2 }" 
              cx="60" cy="60" r="54" stroke-width="12"/>
          </svg>
          <div class="progress-text">{{ latestData.co2 }} <br> PPM</div>
        </div>
      </section>
    </div>

    
  </div>
  </section>
 
</template>

<style>
.nav{
  width: 100vw;
  display: flex;
  height: 4rem;
  position: sticky;
  border-bottom: 1px solid #e7e7e7;
}
.nav ul {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 1.5rem 0 1.5rem;
}
.nav ul li{
  display: flex;
  align-items: center;
}
.nav ul li img{
  height: 50px;
 }

.nav ul li span b{
  margin: 0 .8rem 0 .8rem;
  font-weight: bold;
}
.log-out{
  color: black;
}

.nav ul li span, .nav ul li img{
  margin: 0 .5rem 0 .5rem;
}
.body {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  background-color: rgba(216, 216, 216, 0.2);
  height: 100%;
 }
.data-display-container{
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.progress-container {
  background-color: white;
  width: 18rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 5px 0px rgb(185, 185, 185);
  border-radius: 4%;
}

.label {
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.label p {
  padding: 1rem;
}

.progress-ring-wrapper {
  position: relative;
}

.progress-ring {
  transform: rotate(-90deg);
  border-radius: 50%;
  z-index: -1;
  margin: 0 1rem 1rem 1rem;
}

.progress-circle {
  fill: transparent;
  transition: stroke 0.3s ease;
}

.progress-text {
  position: absolute;
  top: 42%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 24px;
  font-weight: bold;
  color: black;
}

.progress-slider {
  width: 80%;
  margin-top: 20px;
}

.progress-overlay {
  fill: none;
  stroke: #ccc; /* Change this color to your desired gray color */
  opacity: 0.5; /* Adjust opacity as needed */
  transition: stroke 0.3s ease;
}
</style> -->
