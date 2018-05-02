<template>
  <div class="cardViewMode" v-on:click="openCloseTheCard">
    <div class="colLeft">
      <div :class="['dayType', card.dayType]">{{card.dayType}}</div>
    </div>

    <div class="colRight">
      <div class="date">{{card.date}}</div>
      <div :class="['activeLevel', card.activeLevel]">{{card.activeLevel}}</div>
      <div class="treatmentWrap" v-if="card.treatment.length>0">
        <ul class="treatment">
          <li v-for="selectedOption in card.treatment">{{selectedOption}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "posted-card",
    data () {
      return {
        isCardOpen: false
      }
    },
    props: [
      'card',
      'dayTypeOptions',
      'treatmentOptions'
    ],
    methods: {
      openCloseTheCard () {
        this.isCardOpen = !this.isCardOpen;
      }
    }
  }
</script>

<style scoped>
  .cardViewMode{
    display: flex;
    width: 100%;
  }

  .dayType {
    display: inline-block;
    position: relative;
    width: 50px;
    height: 50px;
    margin: 10px auto;
    line-height: 50px;
    text-align: center;
    font-size: 14px;
    font-weight: bold;
    vertical-align: middle;
    color: #888888;
    -webkit-border-radius: 50%;
    border-radius: 50%;
    border: 4px solid #888888;
  }
  .dayType.Good {
    color: #9cd5dd;
    border-color: #9cd5dd;
  }
  .dayType.Okey {
    color: #ffdb32;
    border-color: #ffdb32;
  }
  .dayType.Mig {
    color: #ff9e48;
    border-color: #ff9e48;
  }
  .dayType.null {
    color: #666666;
    border-color: #666666;
  }

  .colLeft {
    display: inline-block;
    flex: 1;
  }

  .colRight {
    display: inline-block;
    flex: 6;
    width: 100%;
  }

  .date {
    display: block;
    position: relative;
    margin: 6px 10px;
  }

  .activeLevel {
    display: block;
    position: relative;
    width: 85%;
    margin: 6px 10px;
    padding: 6px 10px;
    text-align: center;
    font-size: 14px;
    font-weight: bold;
    color: #2d576f;
    -webkit-border-radius: 6px;
    border-radius: 6px;
  }
  .activeLevel.Full {
    background-color: #96d7de;
  }
  .activeLevel.Half {
    background-color: #ffdd29;
  }
  .activeLevel.None {
    background-color: #ff9d42;
  }

  .treatmentWrap {
    display: block;
    width: 100%;
    padding: 0 8px;
  }
  .treatment {
    list-style: none;
    padding-left: 0;
  }
  .treatment li {
    display: inline-block;
    margin: 2px;
    padding: 6px 10px;
    font-weight: bold;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    background-color: #2d576f;
  }
</style>
