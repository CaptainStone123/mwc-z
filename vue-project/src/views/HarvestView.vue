
<script>
export default {
  data() {
    return {
      isMenuOpen: false,
      selectedDate: '',
      totalHarvest: '',
      goodQuality: '',
      lowQuality: '',
      records: [], // Array to store records
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    clearRecords() {
      // Clear the records array
      this.records = [];

      // Clear records from localStorage
      localStorage.removeItem('harvestRecords');
    },
    addRecord() {
      if (this.selectedDate && this.totalHarvest && this.goodQuality && this.lowQuality) {
        const record = {
          date: this.selectedDate,
          totalHarvest: this.totalHarvest,
          goodQuality: this.goodQuality,
          lowQuality: this.lowQuality,
        };

        this.records.push(record);
        this.saveRecordsToLocalStorage(this.records);

        // Clear input fields
        this.selectedDate = '';
        this.totalHarvest = '';
        this.goodQuality = '';
        this.lowQuality = '';
      }
    },
    saveRecordsToLocalStorage(records) {
      localStorage.setItem('harvestRecords', JSON.stringify(records));
    },
    loadRecordsFromLocalStorage() {
      const savedRecords = localStorage.getItem('harvestRecords');
      if (savedRecords) {
        this.records = JSON.parse(savedRecords);
      }
    },
  },
  mounted() {
    this.loadRecordsFromLocalStorage();
  },
};
</script>

<template>
  <div class="main">
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

    <div class="container">
    <!-- <button @click="clearRecords">Clear All</button> -->
    <div class="input-container">
      <h2>Insert Record</h2>
     <input class="input" type="date" v-model="selectedDate" />
       <input class="input" type="number" v-model="totalHarvest" placeholder="Total Harvest (kg)" />
      <input class="input" type="number" v-model="goodQuality" placeholder="Good Quality (kg)" />
      <input class="input" type="number" v-model="lowQuality" placeholder="Low Quality (kg)" />
      <button class="input-btn" @click="addRecord">Add</button>
    </div>
    
    
    <!-- Display records -->
    <div class="table-container">
    <h2>Harvest Record</h2>
    <table class="table">
       <thead>
        <tr>
          <th>Date</th>
          <th>Total Yield</th>
          <th>Good Quality </th>
          <th>Low Quality</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(record, index) in records" :key="index">
          <td>{{ record.date }}</td>
          <td>{{ record.totalHarvest }} kg</td>
          <td>{{ record.goodQuality }} kg</td>
          <td>{{ record.lowQuality }} kg</td>
        </tr>
      </tbody>
    </table>
    </div>
     
  </div>

  </div>
   
</template>

<style>

.nav-links {
  list-style: none;
  padding: 0;
  display: none;
}

.nav-links.open {
  display: none;
}

.nav-links li {
  margin: 0.5rem 0;
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-size: 1.2rem;
}
/* sad */
.nav{
  background-color: white;
}
.nav ul li span, .nav ul li img{
  margin: 0 .5rem 0 .5rem;
}
.main{
  height: 100%;
  /* background-color: #f3f5f7; */
}
a{
  color: black;
  list-style: none;
}
.container{
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  height: 100%;
  padding: 2rem;
}
 

.input-container, .table-container{
  display: flex;
  flex-direction: column;
  gap: .5rem;
  width: 18rem;
  background-color: white;
  padding: 1rem;
  box-shadow: 0 2px 5px 0px rgb(185, 185, 185);
  border-radius: 4%;
}

.table {
    border-collapse: collapse;
    font-size: .6rem;
    background-color: white;
 
  }

  th,  td {
    width: 20%;
    text-align: left;
    padding: 8px;
     border-bottom: 1px solid #ddd;
  }

.input, .input-date{
  border-radius: 5px;
  background-color: #f3f5f7;
  border: none;
  padding: .5rem 1rem .5rem 1rem
}

.input-btn{
   border-radius: .5rem;
  border: none;
  color: white;
  background-color: #01bf63;
  padding: .5rem 1rem .5rem 1rem

}

.input-btn:hover{
  background-color: #06a357;
}
</style>