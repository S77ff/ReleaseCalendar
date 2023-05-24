<template>
  <div>
    <div id="app2">
      <div class="calendar-controls">
        <div v-if="message" class="notification is-success">{{ message }}</div>
        <div class="box">
          <div class="field">
            <br />
            <br />
            <label class="label">Start Day</label>

            <div class="control">
              <div class="select">
                <select v-model="startingDayOfWeek">
                  <option
                    v-for="(d, index) in dayNames"
                    :value="index"
                    :key="index"
                  >
                    {{ d }}
                  </option>
                </select>
              </div>
            </div>
          </div>
          <div class="field">
            <br />
            <br />
            <label class="label">Colour of Event</label>

            <div class="control">
              <div class="select">
                <select name="color" class="form-control" id="color">
                  <option value="">Choose</option>
                  <option style="color:#0071c5;" value="blue">&#9724; In Queue waiting for SIT</option>
                  <option style="color:#46f540;" value="green">&#9724; External Release</option>                         
                  <option style="color:turquoise;" value="turquoise">&#9724; Internal Release</option>
                  <option style="color:#e47a16;" value="orange">&#9724; Planned</option>
                  <option style="color:#cfa6f6;" value="purple">&#9724; SIT</option>
                  <option style="color:grey;" value="grey">&#9724; Handed to Service</option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <div class="box">
          <div class="field">
            <br />
            <label class="label"></label>
            <div class="control">
              <input
                v-model="newEventTitle1"
                placeholder="Product Name"
                class="input"
                type="text"
              />
            </div>
          </div>
        </div>
        <br />
        <div class="box">
          <div class="field">
            <label class="label"></label>
            <div class="control">
              <input
                v-model="teamBox"
                placeholder="Team Name"
                class="input"
                type="text"
              />
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="releaseBox"
                  placeholder="Release"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="statusBox"
                  placeholder="Status"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="targetRelease"
                  placeholder="Target Release"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="feature"
                  placeholder="Feature"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="featureToggleCandidates"
                  placeholder="Feature Toggle Candidates"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="additionalNotes"
                  placeholder="Additional Notes"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="productManager"
                  placeholder="PM"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="productOwner"
                  placeholder="PO"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="dependencies"
                  placeholder="Dependencies"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="box">
            <div class="field">
              <br />
              <label class="label"></label>
              <div class="control">
                <input
                  v-model="scrumMaster"
                  placeholder="SM"
                  class="input"
                  type="text"
                />
              </div>
            </div>
          </div>

          <br />
          <div class="field">
            <label class="label">Start date</label>

            <div class="control">
              <input v-model="newEventStartDate" class="input" type="date" />
            </div>
          </div>
          <br />
          <div class="field">
            <label class="label">End date</label>
            <div class="control">
              <input v-model="newEventEndDate" class="input" type="date" />
            </div>
          </div>
          <br />
          <button class="button is-info" @click="clickTestAddEvent">Add</button>
          <button @click="deleteEvent(event)">Delete</button>
          <button @click="updateEvent(event)">Update</button>
          <br />
          <br />
          <button @click="saveEvents(event)">Save</button>
          <button @click="loadEvents(event)">Load</button>
          <br />
          <br />
          <input type="file" id="fileInput" accept=".json">
          <br />
          <div class="control">
                <input
                  v-model="idBox"
                  placeholder="Id"
                  class="input"
                  type="text"
                  hidden
                />
          </div>
        </div>
      </div>

      <div class="calendar-parent">
        <calendar-view
          :events="events"
          :show-date="showDate"
          :time-format-options="{ hour: 'numeric', minute: '2-digit' }"
          :enable-drag-drop="true"
          :disable-past="disablePast"
          :disable-future="disableFuture"
          :show-event-times="showEventTimes"
          :display-period-uom="displayPeriodUom"
          :display-period-count="displayPeriodCount"
          :starting-day-of-week="startingDayOfWeek"
          :class="themeClasses"
          :period-changed-callback="periodChanged"
          :current-period-label="useTodayIcons ? 'icons' : ''"
          @drop-on-date="onDrop"
          @click-date="onClickDay"
          @click-event="onClickEvent"
        >
          <calendar-view-header
            slot="header"
            slot-scope="{ headerProps }"
            :header-props="headerProps"
            @input="setShowDate"
          />
        </calendar-view>
      </div>
    </div>
  </div>
</template>
<script>
// Load CSS from the published version
require("vue-simple-calendar/static/css/default.css");
require("vue-simple-calendar/static/css/holidays-us.css");
// Load CSS from the local repo

import {
  CalendarView,
  CalendarViewHeader,
  CalendarMathMixin,
} from "vue-simple-calendar";

export default {
  components: { 
    CalendarView, 
    CalendarViewHeader 
  },
  props: {
    events: Array,
  },
  mixins: [CalendarMathMixin],
  data() {
    return {
      /* Show the current month, and give it some fake events to show */
      showDate: this.thisMonth(1),
      message: "Hello",
      startingDayOfWeek: 0,
      disablePast: false,
      disableFuture: false,
      displayPeriodUom: "month",
      displayPeriodCount: 1,
      showEventTimes: true,
      newEventTitle: "",
      newEventStartDate: "",
      newEventEndDate: "",
      useDefaultTheme: true,
      useHolidayTheme: false,
      useTodayIcons: false,
    };
  },
  computed: {
    userLocale() {
      return this.getDefaultBrowserLocale;
    },
    dayNames() {
      return this.getFormattedWeekdayNames(this.userLocale, "long", 0);
    },
    themeClasses() {
      return {
        "theme-default": this.useDefaultTheme,
        "holiday-us-traditional": this.useHolidayTheme,
        "holiday-us-official": this.useHolidayTheme,
      };
    },
  },
  mounted() {
    this.newEventStartDate = this.isoYearMonthDay(this.today());
    this.newEventEndDate = this.isoYearMonthDay(this.today());
    this.statusBox = this.initializeColorBox();
  },
  methods: {
    initializeColorBox() {
      const colorBox = document.getElementById('color');

      colorBox.addEventListener('change', (event) => {
        const color = event.target.value;
        console.log('Colour:', color);
        event.target.style.color = color;
      }, false);
    },
    updateEvent(event) {
      this.deleteEvent(event)
      const id = this.clickTestAddEvent()
      this.saveEventsToLocalStorage()
      this.updateStatus();
      this.idBox = id;
    },
    loadEvents() {
      console.log('1')
      const fileInput = document.getElementById('fileInput');
      
      console.log('2')
      const file = fileInput.files[0]; // Get the file object

      if (file) {
        const reader = new FileReader();

        reader.onerror = function () {
          console.error('Error occurred while loading the file.');
        };

        reader.readAsText(file); // Pass the file object to readAsText()

        reader.onload = function (event) {
          const contents = event.target.result;
          const events = JSON.parse(contents);

          // Save events to localStorage
          localStorage.setItem('calendar-events', JSON.stringify(events));

          console.log('Events loaded successfully.');
        };

        // Refresh the screen
        location.reload();
      }
    },
    saveEvents() {
      // Retrieve events from localStorage
        const events = JSON.parse(localStorage.getItem('calendar-events'));

      // Create a Blob with the JSON data
      const blob = new Blob([JSON.stringify(events)], { type: 'application/json' });

      // Create a temporary anchor element
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);

      // Set the file name
      const fileName = 'events.json';
      a.download = fileName;

      // Programmatically click the anchor element to trigger the download
      document.body.appendChild(a);
      a.click();

      // Cleanup
      document.body.removeChild(a);
      URL.revokeObjectURL(a.href);
    },
    deleteEvent() {
      if (this.idBox) {
        this.message = `I want to delete ${this.idBox}`;
        this.removeItemOnce(this.events, this.idBox)
      }
      this.saveEventsToLocalStorage()
    },
    removeItemOnce(arr, value) {
      const index = arr.findIndex(item => item.id === value);
      if (index > -1) {
        arr.splice(index, 1);
      }

      return arr;
    },
    saveEventsToLocalStorage() {
      localStorage.setItem('calendar-events', JSON.stringify(this.events));
    },
    periodChanged(range, eventSource) {
      // Demo does nothing with this information, just including the method to demonstrate how
      // you can listen for changes to the displayed range and react to them (by loading events, etc.)
      console.log(eventSource);
      console.log(range);
    },
    thisMonth(d, h, m) {
      const t = new Date();
      return new Date(t.getFullYear(), t.getMonth(), d, h || 0, m || 0);
    },
    onClickDay(d) {
      this.message = `You clicked: ${d.toLocaleDateString()}`;
      const year = d.getFullYear();
      const month = String(d.getMonth() + 1).padStart(2, '0');
      const day = String(d.getDate()).padStart(2, '0');

      this.newEventStartDate = `${year}-${month}-${day}`;
      this.newEventEndDate = `${year}-${month}-${day}`;
    },
    onClickEvent(e) {
      this.message = `You clicked: ${e.id} which has date ${e.startDate.toLocaleDateString()}`;
      this.onClickDay(e.startDate)
      e = e.originalEvent;

      this.newEventTitle1 = e.title;
      this.teamBox = e.team;
      this.releaseBox = e.release;
      this.statusBox = e.status;
      this.targetRelease = e.targetRelease;
      this.feature = e.feature;
      this.featureToggleCandidates = e.featureToggleCandidates;
      this.additionalNotes = e.additionalNotes;
      this.productManager = e.productManager;
      this.productOwner = e.productOwner;
      this.dependencies = e.dependencies;
      this.scrumMaster = e.scrumMaster;
      this.idBox = `${e.id}`;
    },
    setShowDate(d) {
      this.message = `Changing calendar view to ${d.toLocaleDateString()}`;
      this.showDate = d;
    },
    onDrop(event, date) {
      this.message = `You dropped ${event.id} on ${date.toLocaleDateString()}`;
      // Determine the delta between the old start date and the date chosen,
      // and apply that delta to both the start and end date to move the event.
      const eLength = this.dayDiff(event.startDate, date);
      event.originalEvent.startDate = this.addDays(event.startDate, eLength);
      event.originalEvent.endDate = this.addDays(event.endDate, eLength);
    },
    updateStatus() {
      const colorBox = document.getElementById('color');
      colorBox.addEventListener('change', (event) => {
        const colourLabel = event.target.selectedIndex;
        this.statusBox = this.statusBox ? this.statusBox : this.getTextOnly(event.target.options[colourLabel].text);
      }, false);
    },
    getTextOnly(str) {
      var firstSpaceIndex = str.indexOf(' ');
      if (firstSpaceIndex !== -1) {
        return str.substring(firstSpaceIndex + 1);
      } else {
        return '';
      }
    },
    clickTestAddEvent() {
      console.log('start');

      const newEventTitle = (this.newEventTitle1 !== undefined) ? `${this.newEventTitle1}` : '';
      const teamB = (this.teamBox !== undefined) ? `${this.teamBox}` : '';
      const releaseB = (this.releaseBox !== undefined) ? `${this.releaseBox}` : '';
      const statusB = (this.statusBox !== undefined) ? `${this.statusBox}` : '';
      const targetReleaseB = (this.targetRelease !== undefined) ? `${this.targetRelease}` : '';
      const feature = (this.feature !== undefined) ? `${this.feature}` : '';
      const featureToggleCandidates = (this.featureToggleCandidates !== undefined) ? `${this.featureToggleCandidates}` : '';
      const additionalNotes = (this.additionalNotes !== undefined) ? `${this.additionalNotes}` : '';
      let productManager = (this.productManager !== undefined) ? `${this.productManager}` : '' ;
      let productOwner = (this.productOwner !== undefined) ? `${this.productOwner}` : '';
      const dependencies =  (this.featureToggleCandidates !== undefined) ? `${this.dependencies}` : '';
      let scrumMaster = (this.scrumMaster !== undefined) ? `${this.scrumMaster}` : '';

      console.log(teamB);
      if (teamB === 'Kronos') {
        console.log(teamB === 'Kronos')
        if (this.productManager === undefined) {
          console.log(this.productManager);
          productManager = 'Rachael Armstrong-Magee'
          this.productManager = productManager;
        }
        if (this.productOwner === undefined) {
          productOwner = 'Reham Khalil'
          this.productOwner = productOwner;
        }
        if (this.scrumMaster === undefined) {
          scrumMaster = 'Mohamed el Wakil'
          this.scrumMaster = scrumMaster;
        }
      }

      var selectElement = document.getElementById("color");

      // Get the selected value
      var selectedColour = selectElement.value ? selectElement.value : "";

      // Print the selected value
      console.log(selectedColour);

      var newId = "e" + Math.random().toString(36).substring(2, 10)

      // eslint-disable-next-line
      this.events.push({
        startDate: this.newEventStartDate,
        endDate: this.newEventEndDate,
        title: newEventTitle,
        team: teamB,
        release: releaseB,
        status: statusB,
        targetRelease: targetReleaseB,
        feature: feature,
        featureToggleCandidates: featureToggleCandidates,
        additionalNotes: additionalNotes,
        productManager: productManager,
        productOwner: productOwner,
        dependencies: dependencies,
        scrumMaster: scrumMaster,
        id: newId,
        classes: selectedColour
      });
      this.message = "You added an event!";
      this.saveEventsToLocalStorage();
      console.log('end');

      return newId;
    },
  },
};
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  background-color: #f7fcff;
}
#app2 {
  display: flex;
  flex-direction: row;
  font-family: Calibri, sans-serif;
  width: 80vw;
  min-width: 30rem;
  max-width: 100rem;
  min-height: 40rem;
  margin-left: auto;
  margin-right: auto;
}
.calendar-controls {
  margin-right: 1rem;
  min-width: 14rem;
  max-width: 14rem;
}
.calendar-parent {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  overflow-x: hidden;
  overflow-y: hidden;
  max-height: 80vh;
  background-color: white;
}
/* For long calendars, ensure each week gets sufficient height. The body of the calendar will scroll if needed */
.cv-wrapper.period-month.periodCount-2 .cv-week,
.cv-wrapper.period-month.periodCount-3 .cv-week,
.cv-wrapper.period-year .cv-week {
  min-height: 6rem;
}

.theme-default .cv-event.green {
  background-color: #e0f0e0;
  border-color: #d7e7d7;
}

.theme-default .cv-event.turquoise {
  background-color: turquoise;
  border-color: turquoise;
}

.theme-default .cv-event.blue {
  background-color: #298edc;
  border-color: #298edc;
}

.theme-default .cv-event.grey {
  background-color: darkgrey;
  border-color: grey;
}

</style>
