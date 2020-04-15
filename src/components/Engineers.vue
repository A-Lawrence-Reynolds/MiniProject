<template>
  <div class="engineers">
    <h1>Engineers</h1>
    <!-- looping engineers arry and displaying the data for first/last name and timezone. -->
    <tbody
      v-for="engineers in engineers"
      :key="engineers.id"
    >
      <tr>
        <td class='FL-table'>{{ engineers.firstname  }} {{engineers.lastname }}</td>
        <td class='T-table'>{{ engineers.timezone }}</td>
      </tr>
    </tbody>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    // creating the engineers arry to be looped and rendered.  
    return {
      // schedules:[],
      engineers: [],
      errors: []
    };
  },

  async created() {
    try {
      // making axios call for engineers. 
      const response = await axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/engineers"
      );
      this.engineers = response.data;
      console.log(response);
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
<!--const res = await axios.get("https://kochava-ui-mini-project.herokuapp.com/schedule")
      this.schedules = res.data;
      console.log(res)
--> 