<template>

  <div class="section-1">

    <div>
      <h1>Engineers</h1>
    </div>
    <div class="onCallDiv" v-if="details">
      <tbody class="display-table">
        <tr>
          <td class=''>{{ details.engineer.firstname  }} {{details.engineer.lastname }}</td>
        </tr>
        <tr>
          <td class=''> timezone: {{ details.engineer.timezone }}</td>
        </tr>
        <tr>
          <td class="onCallTime">{{details.schedule.start}} -- {{details.schedule.end}} {{details.schedule.zone}} - </td>

        </tr>
        <tr>off set: (UTC {{ details.schedule.offset }})</tr>
      </tbody>
    </div>
    <div v-else>
      <p class="selectText">{{oncallText}}</p>
    </div>
    <!-- looping engineers array and displaying the data for first/last name and timezone. -->
    <div class='engineers-'>
      <tbody>
        <tr v-for="engineer in engineers" :key="engineer.id">
        <td class='FL-table' v-on:click='showDetails(engineer)'>{{engineer.firstname}} {{engineer.lastname}}</td>
        </tr>
      </tbody>
    </div>
  </div>
</template>

<script>
// imports //
import axios from "axios";
import moment from "moment-timezone";

export default {
  module:{
    rules:[
      {
        test:/\.scss$/,
        use:[
          'vue-style-loader',
          'css-loader',
          'scss-loader'
        ]
      }
    ]
  },
  data() {
    // creating the engineers/ schedules array's to be looped and rendered.//
    return {
      details: null,
      schedules: [],
      engineers: [],
      errors: [],
      oncallText:
        "Please select you name from the list below"
    };
  },
  // API Call //
  async created() {
    try {
      // making axios call for engineers.//
      const engineersRequest = axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/engineers"
      );
      //// making axios call for schedules.
      const schedulesRequest = axios.get(
        "https://kochava-ui-mini-project.herokuapp.com/schedules"
      );
      // wiating for .all to resolve
      const [engineersResponse, schedulesResponse] = await Promise.all([
        engineersRequest,
        schedulesRequest
      ]);

      // pushing data to there respective arrays in Data()//
      this.engineers = engineersResponse.data;
      this.schedules = schedulesResponse.data;
    } catch (e) {
      // pushing errors to the errors array.//
      this.errors.push(e);
      // displaying the array of errors in the console.//
      console.error("List of errors:", this.errors);
    }
  },

  methods: {
    showDetails(engineer) {
      const schedule = this.schedules.find(s => s.userid === engineer.id);
      // using moment to format time for start and end //
      const onCallStart = moment
        .unix(schedule.start)
        .format("MMM Do YYYY, h:mm a");
      const onCallEnd = moment.unix(schedule.end).format("MMM Do YYYY, h:mm a");

      //setting time zone to respective users timezone //
      const onCallZone = moment()
        .tz(moment.tz.guess())
        .format("z");

      // showing the offset top utc //
      const onCallUTCOffset = moment
        .tz(schedule.start, engineer.timezone)
        .format("Z");

      this.details = {
        engineer,
        schedule: {
          ...schedule,
          start: onCallStart,
          end: onCallEnd,
          zone: onCallZone,
          offset: onCallUTCOffset
        }
      };
    }
  },
  // exporting to app.//
  name: "Engineers"
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.engineers {
  display: flex;
  flex-direction: column;
  padding: 10px;
  margin: 0 auto ;
  

}
.onCallDiv{
  display: flex;
  background-color: black;
  color:#fff;
  text-align: center;
  align-content: center;
  justify-content: center;
  
}
.selectText{
  background-color:#aaae8e
}

</style>
<!--
--> 