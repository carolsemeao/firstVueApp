<template>
  <div id="app">
    <!-- greeting with current date -->
    <h1 class="site-title">{{ title }}</h1>
    <span class="site-description">{{ currentDate }}</span>
    <!-- {{updateCurrentDate()}} -->
    <!-- list items if events happen -->
    <ul class="entry-list">
      <li v-for="entry in filteredEntries" :key="entry.id" class="entry-item">
        <span class="entry-daytime"
          >{{ entry[0] }} Uhr, {{ entry[1].replaceAll("/", ".") }}</span
        >
        <h3 class="entry-title">{{ entry[2] }}</h3>
        <span class="entry-description">{{ entry[3] }}</span>
      </li>
    </ul>

    <!-- footer -->
    <footer>
      <img
        src="./assets/STZH_SEB_Logo.png"
        alt="Stadt Zürich Sozial Betriebe und Einrichtungen"
      />
      <img src="./assets/Opportunity.png" alt="Logo von Opportunity Zürich" />
      <img
        src="./assets/SAG_Logo_De.png"
        alt="Logo von Stiftung Arbeitsgestaltung"
      />
    </footer>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      title: "Welcome to Opportunity",
      currentDate: "",
      gsheet_url:
        "https://sheets.googleapis.com/v4/spreadsheets/1thnl4frHxR5kB5F-k6u-sxpRKzPwUnrZPNMJAANkulk/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyCw24BYVeWoepBOsfQDdqdrxNOHsyFnqFk",
      entries: [],
    };
  },
  computed: {
    // computed properties are like data properties, but with a method combined, it gets executed automatically, instead of calling a function explicitly
    filteredEntries() {
      return [...this.entries].slice(1); //remove first item of array
    },
  },
  methods: {
    //google sheets API connection
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
        /* console.log(response); */
      });
    },
    //update the current date automatically
    updateCurrentDate() {
      let today = new Date();
      const date = `${today.getDate()}.${
        today.getMonth() + 1
      }.${today.getFullYear()}`;
      this.currentDate = date;
    },
    //refresh the google sheet and current day
    refreshData() {
      this.getData();
      this.updateCurrentDate();
    },
  },

  mounted() {
    this.refreshData(); //get first initial data and then wait for the next update
    setInterval(() => {
      this.refreshData();
    }, 1800000); // wait 30mins for next update
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap");
/* css styles go here */

body {
  background: #e8eff4;
}

/* ------------------------------
  desktop view (vertical view)
-----------------------------*/
@media screen and (max-width: 1080px) and (max-height: 1920px) {
  #app {
    font-family: "Inter", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin: 60px;
  }
  .site-title {
    font-size: 62px;
    font-weight: 900;
    margin: 80px 0 20px 0;
  }

  .site-description {
    font-weight: 500;
    font-size: 62px;
    margin: 0;
    color: #9aa7b1;
  }

  .entry-list {
    padding-left: 0;
  }

  .entry-item {
    padding: 35px 40px;
    margin: 40px 0;
    font-size: 28px;
    line-height: 1.3;
    list-style: none;
    background: #0f05a0;
  }

  .entry-daytime {
    font-weight: 900;
    color: #eb5e00;
  }
  .entry-title {
    font-weight: 900;
    font-size: inherit;
    color: #ffbfab;
    margin: 0;
  }
  .entry-description {
    font-weight: 500;
    color: #ffbfab;
  }

  footer {
    background: #ffffff;
    display: flex;
    box-sizing: border-box;
    justify-content: space-between;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 40px;
  }

  footer img {
    height: 50px;
  }
}

/* --------------------------------------
  normal desktop view (landscape mode)
--------------------------------------*/
@media screen and (min-width: 1081px) {
  #app {
    font-family: "Inter", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin: 60px;
  }
  .site-title {
    font-size: 60px;
    font-weight: 900;
    margin: 80px 0 20px 0;
  }

  .site-description {
    font-weight: 500;
    font-size: 60px;
    margin: 0;
    color: #9aa7b1;
  }

  .entry-list {
    padding-left: 0;
  }

  .entry-item {
    padding: 35px 40px;
    margin: 40px 0;
    font-size: 26px;
    line-height: 1.3;
    list-style: none;
    background: #0f05a0;
  }

  .entry-daytime {
    font-weight: 900;
    color: #eb5e00;
  }
  .entry-title {
    font-weight: 900;
    font-size: inherit;
    color: #ffbfab;
    margin: 0;
  }
  .entry-description {
    font-weight: 500;
    color: #ffbfab;
  }

  .entry-item:last-of-type {
    margin-bottom: 200px;
  }

  footer {
    background: #ffffff;
    display: flex;
    box-sizing: border-box;
    justify-content: space-between;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 40px;
  }

  footer img {
    height: 50px;
  }
}

/* --------------
  mobile view
---------------*/

@media screen and (max-width: 700px) {
  #app {
    font-family: "Inter", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    text-align: center;
    margin: 20px;
  }
  .site-title {
    font-size: 50px;
    font-weight: 900;
    margin: 50px 0 20px 0;
    text-align: center;
  }

  .site-description {
    font-weight: 500;
    font-size: 50px;
    margin: 0;
    color: #9aa7b1;
  }

  .entry-list {
    padding-left: 0;
  }

  .entry-item {
    padding: 35px 25px;
    margin: 40px 0;
    font-size: 24px;
    line-height: 1;
    list-style: none;
    background: #0f05a0;
  }

  .entry-daytime {
    font-weight: 900;
    color: #eb5e00;
  }
  .entry-title {
    font-weight: 900;
    font-size: inherit;
    color: #ffbfab;
    margin: 20px 0 0 0;
  }
  .entry-description {
    font-weight: 500;
    color: #ffbfab;
  }

  footer {
    background: #ffffff;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 40px;
    position: static;
    width: 100%;
    box-sizing: border-box;
  }

  footer img {
    height: 50px;
    margin-bottom: 30px;
  }

  footer img:last-child {
    margin-bottom: 0;
  }
}
</style>
