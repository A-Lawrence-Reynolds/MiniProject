<template>

  <div class="section-1">

    <div>
      <h1 class='heading-text'>Oncall Schedule</h1>
    </div>
    <div class="onCallDiv" v-if="details">
      <tbody class="display-table">
        <tr class='oncall-tr'>
          <td class=''>{{ details.engineer.firstname  }} {{details.engineer.lastname }}</td>
        </tr>
        <!-- <tr class='oncall-tr'>
          <td class=''> timezone: {{ details.engineer.timezone }}</td>
        </tr> -->
        <tr class='oncall-tr'>
          <td class="onCallTime">{{details.schedule.start}} -- {{details.schedule.end}} {{details.schedule.zone}}</td>

        </tr>
        <tr class='oncall-tr'><td> UTC Offset: {{ details.schedule.offset }}</td></tr>
      </tbody>
    </div>
    <div class="selectText" v-else>
      <p >{{oncallText}}</p>
    </div>
    <!-- looping engineers array and displaying the data for first/last name and timezone. -->
    <div class='engineers'>
      <tbody class="engineers-tbody">
        <tr v-for="engineer in engineers" :key="engineer.id">
        <td class='grow-text' v-on:click='showDetails(engineer)'>{{engineer.firstname}} {{engineer.lastname}}</td>
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
  padding: 10px 30px;
  margin: 0 auto ;
  border: transparent 2px solid;

  .grow-text:hover
{
        cursor:pointer;
        -webkit-transform: scale(1.1);
        -ms-transform: scale(1.1) ;
        transform: scale(1.1) ;
        background-color: #fff;
        border: #607B7D 2px solid;
        padding: 2px;
        
}

.engineers-tbody{
  display: flex;
  justify-content: space-around;
}
}
.onCallDiv  {
  display: flex;
  background-color:#607B7D;
  color:#fff;
 
  height: 50vh;
  .display-table{
    width: -webkit-fill-available;
    border: #fff solid 1px;
    margin: 20px;
    justify-content: center;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .oncall-tr{
    padding :12px;
    border-bottom: solid #fff 1px;
    width: 50vw;

  }
 
}
// style for text to select a name below //
.selectText{
  display: flex;
  background-color:#607B7D;
  color:#fff;
  text-align: center;
  align-content: center;
  justify-content: center;
  height: 10vh;
  padding :10px;
  }






// animation to grow a names text when hovering over it //
 
</style>
<!--
--> 