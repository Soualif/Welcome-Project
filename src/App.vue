<template>
  
  
  <div id="app">
    <div id="main-frame">
      <div id="main-container" class="flex-container"> 
        
        <div class="header">
          <!-- Main title -->
          <div class="top-title">
            <h1>
              {{ title }}
            </h1>
          </div>
          <!-- Current date subtitle -->
          <h1 class="date">
            {{ currentDate }}
          </h1>
        </div>
        <div class="entries-container">
          <ul v-if="entries && entries.length" class="entries-list" >
            <li  class="entry-item" v-for="entry in entries" :key="entry.id">
              <!--
              <p class="item item-time">{{ entry[0] }} Uhr, {{ entry[1].replaceAll("/", ".") }} </p>
              <p class="item item-title">{{ entry[2] }}</p>
              <p class="item item-description">{{ entry[3] }}</p>
              -->

              <!-- check out vue components docs -->
              <eventEntry :entry="entry" />
             </li>
          </ul>
          <h1 v-else>Zur Zeit keine Events!</h1> 
        </div>
      </div>
      <div class="footer">
        <img :src="logoSTZH" alt="Logo STZH"  style="width: 260px;" class=""> 
        <img :src="logOpp" alt="Logo Opportunity" style="width: 236px;" class=""> 
        <img :src="logoSAG" alt="Logo SAG" style="width: 273px;" class=""> 
      </div>
    </div>
  </div>
</template>

<script>

const leadingZero = function(n) {
  if (n <= 9) {
    return "0" + n;
  }
  return n
};

import axios from "axios"; // axios is a library for making HTTP requests to the backend
import eventEntry from "./components/eventEntry.vue";
export default {
  name: 'App',
  components: { eventEntry },
  data() {
	return {
    title: "Welcome to Opportunity",
    sheet_id: "1eYj2fR4zENBDj44B_2eNcLb9q5nw5jyuhQlhweDottM",
    api_token: "AIzaSyDj-0jsLeXGGG7krtGwhojubzhsjKZ4rOI",
    entries: [],
    currentDate: "",
    logoSTZH: require('./assets/STZH_SEB_Logo.png'),
    logOpp: require('./assets/Opportunity.png'),
    logoSAG: require('./assets/SAG_Logo_De.png')
  };
},

// My API-Key: AIzaSyDj-0jsLeXGGG7krtGwhojubzhsjKZ4rOI
// Project Name on console.cloud.google.com: "Welcome Screen Opportunity"
// https://docs.google.com/spreadsheets/d/1eYj2fR4zENBDj44B_2eNcLb9q5nw5jyuhQlhweDottM/edit?usp=share_link
 

  computed: {
    gsheet_url() {
  return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
  },
},


  methods: {

    // =========== static example: ==============
    /*  
      getData() {
        this.entries = [ 
          // time, date, title, description
          ["8.25", "11/11/1111", "Feier", "failure"],
          ["17.25", "22/22/2222", "wedding", "of a paper clip"]
        ] 
      },
    */


    /* ========= default version: ============= */
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },
   /*  =============== */

   
    /* ========== filtered and sorted by date (work in progress) =============
    getData() {
          axios.get(this.gsheet_url).then((response) => {

             // Erhalte die Zeilen aus der Antwort des API-Aufrufs
            const rows = response.data.valueRanges[0].values;

            // Erhalte das aktuelle Datum
            const currentDate = new Date();

            // Filtere die Zeilen, die ein Datum haben, das heute oder später ist
            const filteredRows = rows.filter(row => {
              
              const dateParts = row[1].split("/");
              const rowDate = new Date(`${dateParts[2]}-${dateParts[1]}-${dateParts[0]}`);
              return rowDate >= currentDate;

            });

             // Sortiere die gefilterten Zeilen nach Datum
            this.entries = filteredRows.sort((row1, row2) => {

              const dateParts1 = row1[1].split("/");
              const dateParts2 = row2[1].split("/");
              const date1 = new Date(`${dateParts1[2]}-${dateParts1[1]}-${dateParts1[0]}`);
              const date2 = new Date(`${dateParts2[2]}-${dateParts2[1]}-${dateParts2[0]}`);
              return date1 - date2;
              
            });
          });
        },
    ================ */


    
    updateCurrentDate() {
      let today = new Date();
      this.counter++;
      this.currentDate = `${leadingZero(today.getDate())}.${leadingZero(today.getMonth())}.${today.getFullYear()}`;
      this.currentTime = ``;
      console.log(this.currentDate)
    },
    refreshData() {
      this.updateCurrentDate();
      this.getData();
    },
  },
  mounted() {
    this.refreshData(); // get first initial data then wait for the next
    setInterval(this.refreshData, 1800000); // wait 30 min for next update
  },
};

const d = new Date();
const currentMs = d.getTime();
console.log(currentMs)


/* ============= Additional feature to research ==============

//Neue Version
        


hide expired event entries: 

let currentMs = currentDate to timestamp;
let entryDateMs = entry[1] to ms

if currentMs > {
  hide this entry in entries
}

const entryMs(date) => {
  const d = new Date(date);
  return d.getTime();
}
entryMs(entry[1]);
console.log(entryMs;)



=========================== */



  /*components: {
    HelloWorld
  }
};*/
</script>

<style>
     
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap');



 * {
   font-family: Inter, sans-serif, Helvetica;
   box-sizing: border-box;
   white-space: normal;
 }


 #app {
  background-color: #ffffff;
 }
 
  #main-frame {
    max-width: 1080px;
    height: 1920px;
    margin: auto;
  }

 .flex-container {
  display: flex;
  align-items:flex-start;
  width: 100%;
  height: 100%;
  justify-content: flex-start;
  flex-direction: column;
  align-content: flex-start;
  padding-inline: 60px;
  margin: auto;
  overflow: hidden;
  
}

#main-container {
  /*background-color: #e8eff4b7;*/
  background-color: #E8EFF4;
}

.header {
  background-color: #E8EFF4;
  position: sticky;
    top: 0;
    margin: 0;
    width: 100%;
}

.entries-container {
   /*display: inline-block;*/
   display: flex;
   flex-direction: column;
   background-color: inherit;
   width: 100%;
   height: auto;
   margin-inline: auto;
   padding-block: 1rem;
   padding-inline: auto;
 }

 ul.entries-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
 }

.entry-item {
   /*display: inline-block;*/
   background-color: #0F05A0;
   width: 100%;
   height:fit-content;
   margin-inline: auto;
   margin-bottom: 40px;
   padding-block: 2rem;
   padding-inline: 2rem;
   /*margin: 0 3rem 1.5rem;*/
 }



 .top-title > h1 {
   color: #323D4A;
   font-weight: 900;
   font-size: 62px;
   line-height: 75px;
   margin-bottom: 0;
   margin-top: 50px;
 }

 .date {
   color: #9AA7B1;
   font-size: 62px;
   line-height: 72px;
   font-weight: 500;
   font-style: normal;
   display: block;
   margin-block: 21px;
 }

 .item {
   
   font-size: 28px;
   line-height: 36px; 
   margin-bottom:0;
   margin-top: 5px;
 }
 
 .item-time {
   color: #EB5E00;
   font-weight: 900;
   padding:0;
 }
 .item-title {
   color: #FFBFAB;
   font-weight: 900;
 }
 .item-description {
   color: #FFBFAB;
   font-weight: 500;
 }

 

 .footer {
    display: flex;
    align-items:center;
    flex-direction: row;
    justify-content: space-between;
    padding-inline: 40px;
    position: sticky;
    bottom: 0;
    margin: 0;
    width: 100%;
    height: 130px;
    background-color: white;
 }

 .logo {
   height: 55px;
 }
</style>
