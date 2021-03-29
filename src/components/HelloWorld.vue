<template>
  <div>
    <input type="number" v-model="filter.homeValue">

    <div v-for="fixture in records" :key="fixture.id">
      {{fixture.id}}
      <div v-for="market in fixture.marketStats" :key="market.name"
      class="mb-12">
      <!-- <b-badge :variant="market.wonBet| badgeColor">{{market.odd}} ({{market.result}})</b-badge> -->
      <span> {{market.wonBet | badgeColor}} {{market.odd}} ({{market.result}})</span>
      </div>  
    </div>
  </div>
  <!-- <b-row>
    <b-col>
    <div>
    <b-button v-b-toggle.sidebar-1><font-awesome-icon icon="bars">filters</font-awesome-icon></b-button>
    <b-sidebar id="sidebar-1" title="filters" shadow>
      <div class="px-3 py-2">
        <div>
          <label for="sb-small">Home Value</label>
          <b-form-spinbutton min="0" max="400" step="10"
          v-model="filterSubmitted.homeValue" size="sm" placeholder="--" class="mb-2"></b-form-spinbutton>
          <label for="sb-small">Away Value</label>
          <b-form-spinbutton min="0" max="400" step="10"
          v-model="filterSubmitted.awayHome" size="sm" placeholder="--" class="mb-2"></b-form-spinbutton>
          <b-button block variant="success" @click="applyFilter">Apply Filter</b-button>
      </div>
      </div>
    </b-sidebar>
  </div>
  <div v-if="isBusy" class="text-center text-danger my-2">
      <b-spinner class="align-middle"></b-spinner>
      <strong>Loading...</strong>
    </div>
    
    <b-card class="fixture" no-body v-for="fixture in recordsFiltered" :key="fixture.id" >
    <template #header>
      {{fixture.id}} <strong>{{fixture.eventDate | moment}}</strong>
      <b-img height="50" :src="fixture.leagueLogo"/> 
       <strong>{{fixture.homeTeam}}-{{fixture.awayTeam}}</strong>
      {{fixture.leagueName}}({{fixture.leagueCountry}})
    </template>
      <div v-for="market in fixture.marketStats" :key="market.name"
      class="mb-12">
      <b-card class="market">
        <b-row class="mb-12">
          <b-col sm="12" class="text-sm-left"><b>{{market.name}}
            <b-badge :variant="market.wonBet | badgeColor">{{market.odd}} ({{market.result}})</b-badge></b>
          </b-col>
        </b-row>
        
      </b-card>         
      <br/>
    </div>
    <b-card-footer>
      {{fixture.statusName}} ({{fixture.score}})
    </b-card-footer>
    </b-card>
    </b-col>
  </b-row> -->
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  name: "Fixtures",
  props: {
    title: String,
  },
  data() {
    return {
      isBusy:true,
      filter:{
          homeValue : 0,
          awayValue : 0,
      },
      filterSubmitted:{
          homeValue : null,
          awayValue : null,
      },
      fields: ['id', 'homeTeam', 'awayTeam', 'eventDate', 'leagueName', 'leagueCountry','leagueLogo'
        , 'statusName', 'score'],
      records:[],
      recordsFiltered:[],
      };
  },
  filters: {
    moment(date) {
      return moment(date).format('HH:mm:ss');
    },
    badgeColor(won){
      console.log('badgeColor')
      if(won === true) 
        return 'success' 
        else if (won === false)
          return 'danger';
        else return 'dark';
      }
  },
  methods: {
    // oddValue: function(percentage, odd){
    //   if(percentage && odd){
    //     return percentage / ((1 / odd) * 100) * 100
    //   }
    //   return null;
    // },
    // getOddValueVariant(result){
    //   console.log('getOddValueVariant')
    //   if(result === true) 
    //    return 'success' 
    //   else if (result === false)
    //     return 'danger';
    //   else return 'dark';
    // },
    // applyMarketFilter(market){
    //   console.log('applyMarketFilter')
    //   let homeFilter = this.filter.homeValue>0 ? this.filter.homeValue : 0;
    //   let awayFilter = this.filter.awayValue>0 ? this.filter.awayValue : 0;
    //   if(homeFilter === 0 && awayFilter == 0)
    //     return true;

    //   return (market.allSeason.homeStats.oddValue>homeFilter
    //   ||  market.competition.homeStats.oddValue>homeFilter
    //   ||  market.homeOrVisitor.homeStats.oddValue>homeFilter)
    //   &&  (market.competition.awayStats.oddValue>awayFilter
    //   ||  market.allSeason.awayStats.oddValue>awayFilter
    //   ||  market.homeOrVisitor.awayStats.oddValue>awayFilter);
    // },
    // shouldShowFixture(fixture){
    //   console.log('shouldShowFixture')
    //   //  .some(element => {
    //   //   if(this.applyMarketFilter(element) == true){
    //   //     return true;
    //   //   }
    //   // });
    //   for (var market in fixture.marketStats) {
    //       if(this.applyMarketFilter(market) == true)
    //         return true;
    //   }
    //   return false;
    // },
    // applyFilter(){
    //   console.log('applyFilter')
    //     this.filterSubmitted.homeValue = this.filter.homeValue;
    //     this.filterSubmitted.awayValue = this.filter.awayValue;
    //     this.recordsFiltered = this.records.filter(this.shouldShowFixture);
    //     // this.recordsFiltered = this.records;
    // }
  },
  mounted () {
    axios
      .get(process.env.VUE_APP_API_URL + "/fixtures")
      .then(response => {
        this.records = response.data;
        // this.applyFilter();
        this.isBusy = false;
      })
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
