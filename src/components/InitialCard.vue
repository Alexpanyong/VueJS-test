<template>
    <div class="cardEditMode">
      <div class="newDayLabel" v-on:click="openCloseTheCard">How was your day, {{currentDate}}?</div>
      <div v-if="isCardOpen" class="newDayCard">
        <div class="dayTypeBox">
          <ul class="dayTypeOptions">
            <li :class="['dayType', dayTypeOption.dayType]" v-on:click="onClickDaytype" v-for="dayTypeOption in cardDayTypeOptions">
              {{dayTypeOption.dayType}}
            </li>
          </ul>
        </div>
        <div class="activeLevelBox" v-if="selectedDayType">
          <div class="activeLevelBoxTitle">Active level:</div>
          <ul class="activeLevelOptions">
            <li :class="['activeLevel', activeLevelOption.activeLevel]" v-on:click="onClickActiveLevel" v-for="activeLevelOption in cardActiveLevelOptions">
              {{activeLevelOption.activeLevel}}
            </li>
          </ul>
        </div>
        <div class="treatmentOptionBox" v-if="selectedActiveLevel">
          <div class="treatmentBoxTitle">
            {{this.treatmentOptions.length > 0 ? 'Treatment(s) used:' : 'No treatment available (you can switch on in settings)' }}
          </div>
          <ul class="treatmentOptions">
            <li :class="['treatment', treatmentOption.treatment]" v-on:click="onClickTreatment" v-for="treatmentOption in cardTreatmentOptions">
              {{treatmentOption.treatment}}
            </li>
          </ul>
        </div>
        <div class="buttonBox" v-if="selectedActiveLevel || selectedTreatment.length">
          <button class="ui button btnDone" v-on:click="onClickDone(postNewDay(initialCard[0], currentDate, selectedDayType[0].dayType, selectedActiveLevel[0].activeLevel, selectedTreatment))">Done</button>
        </div>
      </div>
    </div>
</template>

<script>
  export default {
    name: "initial-card",
    data () {
      return {
        isCardOpen: false,
        currentDate: this.getCurrentDate(),
        cardDayTypeOptions: this.dayTypeOptions,
        selectedDayType: null,
        cardActiveLevelOptions: this.activeLevelOptions,
        selectedActiveLevel: null,
        cardTreatmentOptions: this.treatmentOptions,
        selectedTreatment: []
      }
    },
    props: [
      'initialCard',
      'dayTypeOptions',
      'activeLevelOptions',
      'treatmentOptions',
      'postNewDay'
    ],
    methods: {
      getCurrentDate () {
        const currentDate = new Date();
        const currentYear = currentDate.getFullYear();
        const currentMonth = currentDate.getMonth() + 1;
        const filteredMonth = currentMonth < 10 ? '0' + currentMonth : currentMonth;
        const currentDay = currentDate.getDate();
        const filteredDay = currentDay < 10 ? '0' + currentDay : currentDay;
        return currentYear + '-' + filteredMonth + '-' + filteredDay;
      },

      openCloseTheCard () {
        this.isCardOpen = !this.isCardOpen;
      },

      resetDayTypeSelection (tar) {
        const theList = tar.parentElement.childNodes;
        for (let elm of theList) {
          elm.classList.remove('selected');
        }
        this.cardDayTypeOptions = this.dayTypeOptions;
      },

      onClickDaytype (e) {
          if (e.target.classList.value.includes('selected')) {
            this.selectedDayType = null;
            this.resetDayTypeSelection(e.target);
          } else {
            this.resetDayTypeSelection(e.target);
            e.target.classList.add('selected');
            const selectedObj = this.cardDayTypeOptions.filter(item => item.dayType === e.target.innerHTML.trim());
            this.selectedDayType = selectedObj.map(obj => ({...obj, isSelected: true}));
            this.cardDayTypeOptions = this.dayTypeOptions.map(item => item.dayType === this.selectedDayType[0].dayType ? ({...item, isSelected: true}) : item);
          }
          // console.log("Day type selected: ", this.selectedDayType);
          // console.log("Current state of Day Type selection: ", this.cardDayTypeOptions);
      },

      resetActiveLevelSelection (tar) {
        const theList = tar.parentElement.childNodes;
        for (let elm of theList) {
          elm.classList.remove('selected');
        }
        this.cardActiveLevelOptions = this.activeLevelOptions;
      },

      onClickActiveLevel (e) {
        if (e.target.classList.value.includes('selected')) {
          this.selectedActiveLevel = null;
          this.resetActiveLevelSelection(e.target);
        } else {
          this.resetActiveLevelSelection(e.target);
          e.target.classList.add('selected');
          const selectedObj = this.cardActiveLevelOptions.filter(item => item.activeLevel === e.target.innerHTML.trim());
          this.selectedActiveLevel = selectedObj.map(obj => ({...obj, isSelected: true}));
          this.cardActiveLevelOptions = this.activeLevelOptions.map(item => item.activeLevel === this.selectedActiveLevel[0].activeLevel ? ({...item, isSelected: true}) : item);
        }
        // console.log("Active level selected: ", this.selectedActiveLevel);
        // console.log("Current state of Active Level selection: ", this.cardActiveLevelOptions);
        console.log("Treatment options: ", this.cardTreatmentOptions);
      },

      onClickTreatment (e) {
        if (e.target.classList.value.includes('selected')) {
          e.target.classList.remove('selected');
          const theOptionIndex = this.selectedTreatment.findIndex(option => e.target.innerHTML.trim() === option.treatment);
          this.selectedTreatment.splice(theOptionIndex, 1);
        } else {
          e.target.classList.add('selected');
          const selectedObj = this.cardTreatmentOptions.filter(item => item.treatment === e.target.innerHTML.trim());
          this.selectedTreatment.push(selectedObj.map(obj => obj.treatment));
          for (let elm of this.selectedTreatment) {
            this.cardTreatmentOptions = this.cardTreatmentOptions.map(item => item.treatment === elm[0] ? ({
              ...item,
              isSelected: true
            }) : item);
          }
        }
        // console.log("Treatment selected: ", this.selectedTreatment);
        // console.log("Current state of Treatment selection: ", this.cardTreatmentOptions);
      },

      onClickDone (fn) {
        return fn;
      }
    }
  }
</script>

<style scoped>
  .cardEditMode {
    display: block;
    width: 100%;
    background-color: #313b4b;
  }
  .cardEditMode ul {
    list-style: none;
    padding-left: 0;
  }

  .newDayLabel {
    display: block;
    position: relative;
    width: 100%;
    padding: 8px 0;
    font-size: 18px;
    font-weight: bold;
    text-align: center;
  }
  .newDayLabel:hover {
    cursor: pointer;
  }

  .newDayCard {
    display: block;
    position: relative;
    width: 100%;
  }

  .dayTypeOptions {
    text-align: center;
  }
  .dayTypeOptions li {
    display: inline-block;
    position: relative;
    width: 50px;
    height: 50px;
    margin: 10px 20px;
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
  .dayTypeOptions li:hover {
    cursor: pointer;
  }
  .dayTypeOptions .Good {
    color: rgba(156, 213, 221, 0.5);
    border-color: rgba(156, 213, 221, 0.3);
  }
  .dayTypeOptions .Good.selected {
    color: rgba(156, 213, 221, 1);
    border-color: rgba(156, 213, 221, 1);
  }
  .dayTypeOptions .Okey {
    color: rgba(255, 219, 50, 0.5);
    border-color: rgba(255, 219, 50, 0.3);
  }
  .dayTypeOptions .Okey.selected {
    color: rgba(255, 219, 50, 1);
    border-color: rgba(255, 219, 50, 1);
  }
  .dayTypeOptions .Mig {
    color: rgba(255, 158, 72, 0.5);
    border-color: rgba(255, 158, 72, 0.3);
  }
  .dayTypeOptions .Mig.selected {
    color: rgba(255, 158, 72, 1);
    border-color: rgba(255, 158, 72, 1);
  }
  .dayTypeOptions .null {
    color: #666666;
    border-color: #666666;
  }

  .activeLevelBox {
    margin: 8px 0;
  }
  .activeLevelBoxTitle {
    font-weight: bold;
  }
  .activeLevelOptions {
    list-style: none;
    padding-left: 0;
  }
  .activeLevelOptions li {
    display: inline-block;
    margin: 2px;
    padding: 6px 10px;
    font-weight: bold;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    border: 2px solid #dfe8f1;
  }
  .activeLevelOptions li:hover {
    cursor: pointer;
  }
  .activeLevelOptions .Full {
    color: rgba(156, 213, 221, 0.5);
    border-color: rgba(156, 213, 221, 0.5);
  }
  .activeLevelOptions .Full.selected {
    color: #2d576f;
    border-color: rgba(156, 213, 221, 1);
    background-color: rgba(156, 213, 221, 1);
  }
  .activeLevelOptions .Half {
    color: rgba(255, 219, 50, 0.5);
    border-color: rgba(255, 219, 50, 0.5);
  }
  .activeLevelOptions .Half.selected {
    color: #2d576f;
    border-color: rgba(255, 219, 50, 1);
    background-color: rgba(255, 219, 50, 1);
  }
  .activeLevelOptions .None {
    color: rgba(255, 158, 72, 0.5);
    border-color: rgba(255, 158, 72, 0.5);
  }
  .activeLevelOptions .None.selected {
    color: #2d576f;
    border-color: rgba(255, 158, 72, 1);
    background-color: rgba(255, 158, 72, 1);
  }

  .treatmentOptionBox {
    margin: 8px 0;
  }
  .treatmentBoxTitle {
    font-weight: bold;
  }
  .treatmentOptions {
    list-style: none;
    padding-left: 0;
  }
  .treatmentOptions li {
    display: inline-block;
    margin: 2px;
    padding: 6px 10px;
    font-weight: bold;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    border: 2px solid #dfe8f1;
  }
  .treatmentOptions li:hover {
    cursor: pointer;
  }
  .treatmentOptions li.selected {
    border: 2px solid #2d576f;
    background-color: #2d576f;
  }

  .buttonBox {
    text-align: right;
  }
  .btnDone {
    padding: 6px 10px;
    font-size: 14px;
    font-weight: bold;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    background-color: rgba(223, 232, 241, 1);
  }
  .btnDone:hover {
    cursor: pointer;
  }
</style>
