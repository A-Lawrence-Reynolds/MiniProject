<template>
  <div class="engineers">
    <h1>Engineers</h1>
    <!-- looping engineers arry and displaying the data for first/last name and timezone. -->
    <tbody>
      <tr v-for="engineers in engineers" :key="engineers.id">
        <td class='FL-table'>{{ engineers.firstname  }} {{engineers.lastname }}</td>
        <td class='T-table'>{{ engineers.timezone }}</td>
      </tr>
      <tr v-for="schedules in schedules" :key="schedules.id">
        <p>{{moment(schedules).format('MMMM Do YYYY,h:mm:ss a')}}</p>
        <p class='method'>{{date(schedules)}}</p>
        <p class='filter'>{{schedules| moment}}</p>
      </tr>
    </tbody>
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment'

export default {
  data() { // creating the engineers/ schedules arry's to be looped and rendered.  
    return {
      schedules:[],
      engineers: [],
      errors: []
    };
  },
   async created() {
    try {
      // making axios call for engineers. 
      const response = await axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/engineers")
      this.engineers = response.data;
      console.log(response);
      
      const res = await axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/schedules")
      this.schedules = res.data;
      console.log(res);

    } catch (e) {
      // pushing errors to the errors arry.
      this.errors.push(e);
      // displaying the arry of errors in the console.
      console.error('List of errors:', this.errors)
    }

  },

  // exporting to app.
  name: "Engineers"
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.FL-table{
  background-color: rebeccapurple;
  color: #fff;
}
.T-table{
  background-color: grey;
  color: #fff;

}
</style>
<!--
--> 