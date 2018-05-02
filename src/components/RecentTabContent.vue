<template>
  <div class="recentTabContent" v-bind:onload="fetchRecentTabData()">
    <ul class="cardsWrap">

      <li class="card" v-for="card in cards">
        <PostedCard
          v-if="card.dayExists"
          v-bind:card="card"
          v-bind:dayTypeOptions="dayTypeOptions"
          v-bind:treatmentOptions="treatmentOptions"
        />
      </li>

      <li class="card">
        <InitialCard
          v-bind:initialCard="initialCard"
          v-bind:dayTypeOptions="dayTypeOptions"
          v-bind:activeLevelOptions="activeLevelOptions"
          v-bind:treatmentOptions="treatmentOptions"
          v-bind:postNewDay="postNewDay"
        />
      </li>

    </ul>
  </div>
</template>

<script>
  import InitialCard from './InitialCard'
  import PostedCard from './PostedCard'
  export default {
    name: "recent-tab-content",
    data () {
      return {
        cards: null,
        initialCard: [
          {
            date: this.currentDate,
            dayExists: false,
            dayType: '',
            activeLevel: '',
            treatment: [],
            notes: ''
          }
        ],
        dayTypeOptions: [
          { dayType: 'Good' },
          { dayType: 'Okey' },
          { dayType: 'Mig' }
        ],
        activeLevelOptions: [
          { activeLevel: 'Full' },
          { activeLevel: 'Half' },
          { activeLevel: 'None' }
        ],
        treatmentOptions: this.treatmentOptionsFromSetting.length > 0 ?
          this.treatmentOptionsFromSetting.filter(item => item.status === true).map(option => ({treatment: option.name})) :
          this.availableTreatmentOptions,
        recentTabDataResponse: [
          {
            date: '2017-11-24',
            dayExists: true,
            dayType: 'Good',
            activeLevel: 'Full',
            treatment: [ 'Sumatriptan', 'Topiramate'],
            notes: ''
          },
          {
            date: '2017-11-23',
            dayExists: true,
            dayType: 'Okey',
            activeLevel: 'Half',
            treatment: ['Sumatriptan', 'Running'],
            notes: ''
          },
          {
            date: '2017-11-22',
            dayExists: true,
            dayType: 'Mig',
            activeLevel: 'None',
            treatment: ['Sleeping', 'Lemon Tea'],
            notes: ''
          },
          {
            date: '2017-11-21',
            dayExists: true,
            dayType: 'Good',
            activeLevel: 'Full',
            treatment: ['Dark Room Rest'],
            notes: ''
          }
        ]
      }
    },
    props: [
      'availableTreatmentOptions',
      'treatmentOptionsFromSetting'
    ],
    methods: {
      fetchRecentTabData () {
        this.cards = this.recentTabDataResponse;
        // console.log("fetched recent tab data!!!", this.recentTabDataResponse);
      },

      postNewDay (item, date, dayType, activeLevel, treatment) {
        if (item.date === this.cards[this.cards.length - 1].date) {
          this.cards.pop();
        }
        item.dayExists = true;
        item.date = date;
        item.dayType = dayType;
        item.activeLevel = activeLevel || '';
        item.treatment = treatment.map(item => item[0]);
        this.isCardOpen = false;
        this.cards.push(item);
      }
    },
    components: {
      InitialCard,
      PostedCard
    }
  }
</script>

<style scoped>
  .cardsWrap {
    display: block;
    position: relative;
    list-style: none;
    margin: 0;
    padding-left: 0;
  }

  .card {
    display: block;
    position: relative;
    box-sizing: border-box;
    text-align: left;
    margin: 8px;
    padding: 14px 20px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    background-color: #313b4b;
  }
</style>
