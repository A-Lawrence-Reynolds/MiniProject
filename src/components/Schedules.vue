<!--
<template>
  <div class='schedule'>

  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    // creating the schedules arry to be looped.
    return {
      schedules: [],
      errors: []
    };
  },
  async created() {
    try {
      // making axios call for schedules.
      const response = await axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/schedules"
      );
      this.engineers = response.data;
      console.log(response);

    } catch (e) {
      // pushing errors to the errors arry.
      this.errors.push(e);
      // displaying the arry of errors in the console.
      console.error("List of errors:", this.errors);
    }
  },
  name: "Schedule"
};
</script>
-->