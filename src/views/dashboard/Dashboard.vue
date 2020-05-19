<template>
  <v-container
    id="dashboard"
    fluid
    tag="section"
  >
    <v-row>
      <v-col
        cols="12"
        lg="4"
      >
        <base-material-chart-card
          v-if="boroughLoaded"
          :data="getBoroughData"
          :options="boroughChart.options"
          :responsive-options="boroughChart.responsiveOptions"
          color="#E91E63"
          hover-reveal
          type="Bar"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h4 class="card-title font-weight-light mt-2 ml-2">
            New York Boroughs
          </h4>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            Accidents by location
          </p>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-city
            </v-icon>
            <span class="caption grey--text font-weight-light">
              Brooklyn Manhattan Queens Bronx
            </span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        lg="4"
      >
        <base-material-chart-card
          :data="dailySalesChart.data"
          :options="dailySalesChart.options"
          color="success"
          hover-reveal
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h4 class="card-title font-weight-light mt-2 ml-2">
            Daily Sales
          </h4>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            <v-icon
              color="green"
              small
            >
              mdi-arrow-up
            </v-icon>
            <span class="green--text">55%</span>&nbsp;
            increase in today's sales
          </p>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">updated 4 minutes ago</span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        lg="4"
      >
        <base-material-chart-card
          :data="dataCompletedTasksChart.data"
          :options="dataCompletedTasksChart.options"
          hover-reveal
          color="info"
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h3 class="card-title font-weight-light mt-2 ml-2">
            Completed Tasks
          </h3>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            Last Last Campaign Performance
          </p>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">campaign sent 26 minutes ago</span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="info"
          icon="mdi-car"
          title="Accidents"
          sub-icon="mdi-calendar"
          :sub-text="monthString"
          :value="total_crashes"
        />
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="primary"
          icon="mdi-account-group"
          title="Pedestrians Accidents"
          :value="pedestrianCrashes"
          sub-icon="mdi-calendar"
          :sub-text="monthString"
        />
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="success"
          icon="mdi-account"
          title="Fatalities"
          :value="personsKilled"
          sub-icon="mdi-calendar"
          :sub-text="monthString"
        />
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="orange"
          icon="mdi-motorbike"
          title="Motorists Injured"
          :value="184"
          :sub-text="monthString"
        />
      </v-col>

      <v-col
        cols="12"
        md="6"
      >
        <base-material-card
          color="warning"
          class="px-5 py-3"
        >
          <template v-slot:heading>
            <div class="display-2 font-weight-light">
              Employees Stats
            </div>

            <div class="subtitle-1 font-weight-light">
              New employees on 15th September, 2016
            </div>
          </template>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="items"
            />
          </v-card-text>
        </base-material-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import Vue from 'vue'
  import axios from 'axios'
  import VueAxios from 'vue-axios'

  Vue.use(VueAxios, axios)
  export default {
    name: 'DashboardDashboard',

    data () {
      return {
        dailySalesChart: {
          data: {
            labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'],
            series: [
              [12, 17, 7, 17, 23, 18, 38],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
            chartPadding: {
              top: 0,
              right: 0,
              bottom: 0,
              left: 0,
            },
          },
        },
        dataCompletedTasksChart: {
          data: {
            labels: ['12am', '3pm', '6pm', '9pm', '12pm', '3am', '6am', '9am'],
            series: [
              [230, 750, 450, 300, 280, 240, 200, 190],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 1000, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
            chartPadding: {
              top: 0,
              right: 0,
              bottom: 0,
              left: 0,
            },
          },
        },
        boroughChart: {
          data: {
            labels: ['BKLN', 'MANH', 'QNS', 'BX', 'SI', 'UNKN'],
            series: [
              [0, 0, 0, 0, 0, 0],
            ],
          },
          options: {
            axisX: {
              showGrid: false,
            },
            low: 0,
            high: 2000,
            chartPadding: {
              top: 0,
              right: 5,
              bottom: 0,
              left: 0,
            },
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc: (value) => value[0],
              },
            }],
          ],
        },
        headers: [
          {
            sortable: false,
            text: 'ID',
            value: 'id',
          },
          {
            sortable: false,
            text: 'Name',
            value: 'name',
          },
          {
            sortable: false,
            text: 'Salary',
            value: 'salary',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Country',
            value: 'country',
            align: 'right',
          },
          {
            sortable: false,
            text: 'City',
            value: 'city',
            align: 'right',
          },
        ],
        items: [
          {
            id: 1,
            name: 'Dakota Rice',
            country: 'Niger',
            city: 'Oud-Tunrhout',
            salary: '$35,738',
          },
          {
            id: 2,
            name: 'Minerva Hooper',
            country: 'Curaçao',
            city: 'Sinaai-Waas',
            salary: '$23,738',
          },
          {
            id: 3,
            name: 'Sage Rodriguez',
            country: 'Netherlands',
            city: 'Overland Park',
            salary: '$56,142',
          },
          {
            id: 4,
            name: 'Philip Chanley',
            country: 'Korea, South',
            city: 'Gloucester',
            salary: '$38,735',
          },
          {
            id: 5,
            name: 'Doris Greene',
            country: 'Malawi',
            city: 'Feldkirchen in Kārnten',
            salary: '$63,542',
          },
        ],
        tabs: 0,
        list: {
          0: false,
          1: false,
          2: false,
        },
        number_of_motorist_injured: 0,
        number_of_pedestrians_injured: 0,
        number_of_persons_killed: 0,
        contributing_factor_vehicle_1: 0,
        contributing_factor_vehicle_2: 0,
        contributing_factor_vehicle_3: 0,
        contributing_factor_vehicle_4: 0,
        contributing_factor_vehicle_5: 0,
        total_crashes: 0,
        day_accidents: 0,
        crash_date: null,
        alcoholCrashes: 0,
        pedestrianCrashes: 0,
        motoristCrashes: 0,
        personsKilled: 0,
        dateFrom: '',
        dateTo: '',
        monthString: '',
        contributingFactors: {},
        borough: '',
        boroughLoaded: false,
      }
    },
    computed: {
      getBoroughData: function () {
        console.log(this.boroughChart.data)
        return this.boroughChart.data
      },
    },
    mounted () {
      this.getData()
    },
    methods: {
      complete (index) {
        this.list[index] = !this.list[index]
      },
      incrementBronx () {
        this.boroughChart.data.series[3] = this.boroughChart.data.series[3] + 1
      },
      getData () {
        const priorDate = new Date()
        priorDate.setDate(priorDate.getDate() - 35) // get previous 30 day
        const timeString = priorDate.toISOString().substring(0, priorDate.toISOString().length - 2) // change it into format that NY API can understand
        const params = {
          $where: 'crash_date >= \'' + timeString + '\'',
          $limit: 6000,
          $order: 'crash_date',
        }
        Vue.axios.get('https://data.cityofnewyork.us/resource/h9gi-nx95.geojson?', { params }).then((response) => {
          console.log(response.data.features)
          this.dateFrom = new Date(response.data.features[0].properties.crash_date) // get date from
          this.dateTo = new Date(response.data.features[response.data.features.length - 1].properties.crash_date) // get date to
          const toDate = this.dateTo.toString().split(' ')
          this.dateTo = toDate[0] + ' ' + toDate[1] + ' ' + toDate[2]
          const fromDate = this.dateFrom.toString().split(' ')
          this.dateFrom = fromDate[0] + ' ' + fromDate[1] + ' ' + fromDate[2]
          this.monthString = this.dateFrom + ' to ' + this.dateTo // from to date string label

          this.total_crashes = Object.keys(response.data.features).length // set total_crashes to crashes in last 30 days
          response.data.features.forEach(x => {
            // Record Motorist Injured
            if (x.properties.number_of_motorist_injured > 0) {
              this.motoristCrashes += parseInt(x.properties.number_of_motorist_injured)
            }
            // Record Pedestrian Injured
            if (x.properties.number_of_pedestrians_injured > 0) {
              this.pedestrianCrashes += parseInt(x.properties.number_of_pedestrians_injured)
            }
            // Record Persons Killed
            if (x.properties.number_of_persons_killed > 0) {
              this.personsKilled += parseInt(x.properties.number_of_persons_killed)
            }
            // Increment based on the location of accidents
            if (x.properties.borough === 'BROOKLYN') {
              this.boroughChart.data.series[0][0] += 1
            } else if (x.properties.borough === 'MANHATTAN') {
              this.boroughChart.data.series[0][1] += 1
            } else if (x.properties.borough === 'QUEENS') {
              this.boroughChart.data.series[0][2] += 1
            } else if (x.properties.borough === 'BRONX') {
              this.boroughChart.data.series[0][3] += 1
            } else if (x.properties.borough === 'STATEN ISLAND') {
              this.boroughChart.data.series[0][4] += 1
            } else if (x.properties.borough === null) {
              this.boroughChart.data.series[0][5] += 1
            }
            this.boroughLoaded = true
          })
        })
      },
    },
  }
</script>
