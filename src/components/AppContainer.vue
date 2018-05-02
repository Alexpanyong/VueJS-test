<template>
  <div class="appContainer" v-bind:onload="fetchSettingsFromServer()">
    <div class="tabNav">
      <ul>
        <li v-bind:class="{selected: showRecentContent}" v-on:click="showRecentTabContent">Recent</li>
        <li v-bind:class="{selected: showSettingContent}" v-on:click="showSettingTabContent">Settings</li>
      </ul>
    </div>
    <div class="contentWrap">
      <RecentTabContent
        v-if="showRecentContent"
        v-bind:availableTreatmentOptions="availableTreatmentOptions"
        v-bind:treatmentOptionsFromSetting="treatmentOptionsFromSetting"
      />
      <SettingsTabContent
        v-if="showSettingContent"
        v-bind:treatmentOptionsFromSetting="treatmentOptionsFromSetting"
        v-bind:settingsTabDataResponse="settingsTabDataResponse"
        v-bind:setInitialCardTreatmentOptions="setInitialCardTreatmentOptions"
        v-bind:updateInitialCardTreatment="updateInitialCardTreatment"
      />
    </div>
  </div>
</template>

<script>
  import RecentTabContent from './RecentTabContent'
  import SettingsTabContent from './SettingsTabContent'
  export default {
    name: "AppContainer",
    data () {
      return {
        showRecentContent: true,
        showSettingContent: false,
        availableTreatmentOptions: [],
        treatmentOptionsFromSetting: [],
        settingsTabDataResponse: [
          {
            name: 'Sumatriptan',
            customType: 'others',
            status: true
          },
          {
            name: 'Topiramate',
            customType: 'preventative',
            status: true
          },
          {
            name: 'Dark Room Rest',
            customType: 'relief',
            status: true
          },
          {
            name: 'Lemon Tea',
            customType: 'relief',
            status: false
          },
          {
            name: 'Sleeping',
            customType: 'relief',
            status: false
          },
          {
            name: 'Running',
            customType: 'relief',
            status: false
          }
        ]
      }
    },
    methods: {
      fetchSettingsFromServer () {
        this.availableTreatmentOptions = this.settingsTabDataResponse.filter(item => item.status === true).map(option => ({treatment: option.name}))
      },

      showRecentTabContent () {
        this.showRecentContent = true;
        this.showSettingContent = false;
      },

      showSettingTabContent () {
        this.showRecentContent = false;
        this.showSettingContent = true;
      },

      updateInitialCardTreatment (availableItems, updatedSetting) {
        this.availableTreatmentOptions = availableItems;
        this.treatmentOptionsFromSetting = updatedSetting;
        console.log("update available treatment options:", this.availableTreatmentOptions);
        console.log("update treatment options from settings:", this.treatmentOptionsFromSetting);
      },

      setInitialCardTreatmentOptions (options) {
        this.availableTreatmentOptions = options;
        console.log("=== Previous availableTreatmentOptions array : ", options);
      }
    },
    components: {
      RecentTabContent,
      SettingsTabContent
    }
  }
</script>

<style scoped>
  .appContainer {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    min-width: 320px;
    /*max-width: 750px;*/
    min-height: 480px;
    /*max-height: 1344px;*/
    margin: 0 auto;
    color: #dfe8f1;
    background-color: #426680;
    /*-webkit-border-radius: 10px;*/
    /*border-radius: 10px;*/
    overflow: auto;
  }

  .appContainer * {
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently supported by Chrome and Opera */
  }

  .tabNav {
    font-size: 20px;
  }

  .tabNav li.selected {
    font-weight: bold;
    border-bottom: 3px solid #dfe8f1;
  }

  ul {
    display: block;
    position: relative;
    list-style: none;
    padding-left: 0;
    margin: 0;
  }

  ul li {
    display: inline-block;
    position: relative;
    margin: 10px 16px;
    padding: 10px 14px;
  }

  ul li:hover {
    cursor: pointer;
  }
</style>
