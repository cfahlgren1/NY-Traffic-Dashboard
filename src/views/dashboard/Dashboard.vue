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
          v-if="chartLoaded"
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
            Accidents by Location
          </h4>

          <p class="d-inline-flex font-weight-light ml-2 mt-1" />

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-city
            </v-icon>
            <span class="caption grey--text font-weight-light">
              Brooklyn Manhattan Queens Bronx Staten Island
            </span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        lg="4"
      >
        <base-material-chart-card
          v-if="chartLoaded"
          :data="dailyAccidentsChart.data"
          :options="dailyAccidentsChart.options"
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
            Weekly
          </h4>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            <span>Traffic Accidents by Day of Week</span>&nbsp;
          </p>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">{{ current_time }}</span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        lg="4"
      >
        <base-material-chart-card
          v-if="chartLoaded"
          :data="vehicleChart.data"
          :options="vehicleChart.options"
          :responsive-options="vehicleChart.responsiveOptions"
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
            Accidents by Vehicle Type
          </h4>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            SUV/Van | Sedan | Truck | Motorcycle/Bike/Scooter | Other
          </p>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">
              {{ current_time }}
            </span>
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
          icon="mdi-hospital-building"
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
          icon="mdi-bandage"
          title="Motorists Injured"
          :value="motoristCrashes"
          sub-icon="mdi-calendar"
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
              Contributing Factor to Accidents
            </div>

            <div class="subtitle-1 font-weight-light">
              Accidents with Fatalities and Contributing Factor
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
        dailyAccidentsChart: {
          data: {
            labels: ['S', 'M', 'T', 'W', 'T', 'F', 'S'],
            series: [
              [0, 0, 0, 0, 0, 0, 0],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 1400,
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
        vehicleChart: {
          data: {
            labels: ['SU', 'SE', 'T', 'MC', 'Other'],
            series: [
              [0, 0, 0, 0, 0],
            ],
          },
          options: {
            axisX: {
              showGrid: false,
            },
            low: 0,
            high: 3500,
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
            text: 'Contributing Factor',
            value: 'contributing_factor',
          },
          {
            sortable: false,
            text: 'Vehicle',
            value: 'vehicle',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Injuries',
            value: 'injuries',
            align: 'center',
          },
          {
            sortable: false,
            text: 'Borough',
            value: 'borough',
            align: 'left',
          },
        ],
        items: [],
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
        chartLoaded: false,
        current_time: 'loading',
      }
    },
    computed: {
      getBoroughData: function () {
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
        priorDate.setDate(priorDate.getDate() - 40) // get previous 30 day
        const timeString = priorDate.toISOString().substring(0, priorDate.toISOString().length - 2) // change it into format that NY API can understand
        const params = {
          $where: 'crash_date >= \'' + timeString + '\'',
          $limit: 6000,
          $order: 'crash_date',
        }
        Vue.axios.get('https://data.cityofnewyork.us/resource/h9gi-nx95.geojson?', { params }).then((response) => {
          this.dateFrom = new Date(response.data.features[0].properties.crash_date) // get date from
          this.dateTo = new Date(response.data.features[response.data.features.length - 1].properties.crash_date) // get date to
          const toDate = this.dateTo.toString().split(' ')
          this.dateTo = toDate[0] + ' ' + toDate[1] + ' ' + toDate[2]
          const fromDate = this.dateFrom.toString().split(' ')
          this.dateFrom = fromDate[0] + ' ' + fromDate[1] + ' ' + fromDate[2]
          this.monthString = this.dateFrom + ' to ' + this.dateTo // from to date string label
          var listID = 0

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
            // if contributing factor for vehicle 1 was specified, add it to the list
            if (x.properties.contributing_factor_vehicle_1 !== null && x.properties.contributing_factor_vehicle_1 !== 'Unspecified') {
              this.items.push({
                id: ++listID,
                contributing_factor: x.properties.contributing_factor_vehicle_1,
                vehicle: x.properties.vehicle_type_code1,
                injuries: x.properties.number_of_persons_injured,
                borough: x.properties.borough,
              })
              // if contributing factor for vehicle 2 was specified, add it to the list
              if (x.properties.contributing_factor_vehicle_2 !== null && x.properties.contributing_factor_vehicle_2 !== 'Unspecified') {
                this.items.push({
                  id: ++listID,
                  contributing_factor: x.properties.contributing_factor_vehicle_2,
                  vehicle: x.properties.vehicle_type_code2,
                  injuries: x.properties.number_of_persons_injured,
                  borough: x.properties.borough,
                })
              }
              // catagorization of vehicles
              const sportsUtilitySet = new Set(['Station Wagon/Sport Utility Vehicle', 'Refrigerated Van', 'Ambulance', 'Van', 'AMBUL', 'AMBULANCE', 'ambulance'])
              const truckSet = new Set(['truck', 'Box Truck', 'PICK-', 'PK', 'SKID LOADE', 'mailtruck', 'Stake Truck', 'Stake or Rack', 'Concrete Mixer', 'Bulk Agriculture', 'Pick-up Truck', 'Pickup-Truck', 'Chassis Cab', 'Dump', 'TRAILER', 'Tractor Truck Diesel', 'Beverage Truck', 'Trailer', 'Garbage or Refuse', 'Flat Bed', 'camper tra', 'FDNY TRUCK', 'Bus', 'Carry ALL', 'Tow Truck / Wrecker', 'FDNY Truck', 'PICK UP', 'FLATBED', 'Tractor Truck Gasoline', 'PICK -', 'UTIL', 'Tanker'])
              const sedanSet = new Set(['4 dr sedan', 'Taxi', 'Sedan', 'Convertible'])
              const motorSet = new Set(['Motorbike', 'Motorcycle', 'Dirt bike', 'DIRT BIKE', 'Moped', 'Bike', 'E-Sco', 'Motorscooter', 'E-Scooter', 'E-Bike'])
              if (sportsUtilitySet.has(x.properties.vehicle_type_code1)) {
                this.vehicleChart.data.series[0][0] += 1
              } else if (sedanSet.has(x.properties.vehicle_type_code1)) {
                this.vehicleChart.data.series[0][1] += 1
              } else if (truckSet.has(x.properties.vehicle_type_code1)) {
                this.vehicleChart.data.series[0][2] += 1
              } else if (motorSet.has(x.properties.vehicle_type_code1)) {
                this.vehicleChart.data.series[0][3] += 1
              } else {
                this.vehicleChart.data.series[0][4] += 1
              }
              if (sportsUtilitySet.has(x.properties.vehicle_type_code2)) {
                this.vehicleChart.data.series[0][0] += 1
              } else if (sedanSet.has(x.properties.vehicle_type_code2)) {
                this.vehicleChart.data.series[0][1] += 1
              } else if (truckSet.has(x.properties.vehicle_type_code2)) {
                this.vehicleChart.data.series[0][2] += 1
              } else if (motorSet.has(x.properties.vehicle_type_code2)) {
                this.vehicleChart.data.series[0][3] += 1
              } else {
                this.vehicleChart.data.series[0][4] += 1
              }
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
            const today = new Date(x.properties.crash_date) // create date object for finding day of the week
            if (today.getDay() === 0) {
              this.dailyAccidentsChart.data.series[0][0] += 1
            } else if (today.getDay() === 1) {
              this.dailyAccidentsChart.data.series[0][1] += 1
            } else if (today.getDay() === 2) {
              this.dailyAccidentsChart.data.series[0][2] += 1
            } else if (today.getDay() === 3) {
              this.dailyAccidentsChart.data.series[0][3] += 1
            } else if (today.getDay() === 4) {
              this.dailyAccidentsChart.data.series[0][4] += 1
            } else if (today.getDay() === 5) {
              this.dailyAccidentsChart.data.series[0][5] += 1
            } else if (today.getDay() === 6) {
              this.dailyAccidentsChart.data.series[0][6] += 1
            }
            this.chartLoaded = true // update borough chart
            this.current_time = 'updated at ' + new Date().toLocaleTimeString()
          })
        })
      },
    },
  }
</script>
