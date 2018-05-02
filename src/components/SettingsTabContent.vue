<template>
  <div class="settingsTabContent" v-bind:onload="fetchSettingsTabsData()">
    <div class="settingTitle">Treatment options:</div>
    <ul class="settingsWrap">
      <li class="setting" v-for="setting in settings">
        <div :class="['treatmentSwitch', (setting.status ? 'on' : 'off')]" v-on:click="onClickToggle(setInitialCardTreatmentOptions(availableOptions), setting.name, setting.status)">
          {{setting.status ? 'on' : 'off'}}
        </div>
        <div class="treatmentName">{{setting.name}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "settings-tab-content",
    data () {
      return {
        settings: null,
        targetStatus: null,
        updatedSettings: [],
        availableOptions: []
      }
    },
    props: [
      'treatmentOptionsFromSetting',
      'settingsTabDataResponse',
      'setInitialCardTreatmentOptions',
      'updateInitialCardTreatment'
    ],
    methods: {
      fetchSettingsTabsData () {
        this.settings = this.treatmentOptionsFromSetting.length > 0 ? this.treatmentOptionsFromSetting : this.settingsTabDataResponse;
        // console.log("setting data:", this.settings);
      },

      updateTreatmentSwitch (targetStatus) {
        return targetStatus = !targetStatus;
      },

      onClickToggle (fn, targetName, targetStatus) {
        this.updatedSettings = this.settings.map(item => item.name === targetName ? ({...item, status: this.updateTreatmentSwitch(targetStatus)}) : item);
        this.availableOptions = this.updatedSettings.filter(item => item.status === true);
        this.updateInitialCardTreatment(this.availableOptions, this.updatedSettings);
        // console.log("Updated setting: ", this.updatedSettings );
        console.log("+++ Current treatment options available: ", this.availableOptions );
        return fn;
      }
    }
  }
</script>

<style scoped>
  .settingTitle {
    margin: 6px 0 0 0;
    padding: 0 16px;
    text-align: left;
    font-weight: bold;
  }

  .settingsWrap {
    display: block;
    position: relative;
    list-style: none;
    margin: 4px 0;
    padding-left: 0;
  }

  .setting {
    display: block;
    position: relative;
    margin: 8px;
    padding: 6px 10px;
    text-align: left;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    background-color: #313b4b;
  }

  .setting > div {
    display: inline-block;
    margin: 4px 6px;
  }
  .setting .treatmentSwitch {
    position: relative;
    width: 30px;
    height: 30px;
    padding: 0;
    font-weight: bold;
    text-align: center;
    line-height: 30px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    border: 2px solid rgba(223, 232, 241, 0.5);
  }
  .setting .treatmentSwitch:hover {
    cursor: pointer;
  }
  .setting .treatmentSwitch.on {
    color: #313b4b;
    background-color: rgba(223, 232, 241, 1);
    border: 2px solid rgba(223, 232, 241, 1);
  }
  .setting .treatmentSwitch.off {
    color: rgba(223, 232, 241, 0.5);
  }
</style>
