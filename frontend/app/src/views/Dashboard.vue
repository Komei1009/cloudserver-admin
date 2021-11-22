<template>
  <v-container
    id="dashboard-view"
    fluid
    tag="section"
  >
    <v-row>
      <v-col cols="12">
        <v-row>
          <v-col
            v-for="(chart, i) in charts"
            :key="`chart-${i}`"
            cols="12"
            md="6"
            lg="6"
          >
            <material-chart-card
              :color="chart.color"
              :data="chart.data"
              :options="chart.options"
              :responsive-options="chart.responsiveOptions"
              :title="chart.title"
              :type="chart.type"
            >
              <template #subtitle>
                <div class="font-weight-light text--secondary">
                  <div v-html="chart.subtitle" />
                </div>
              </template>

              <template #actions>
                <v-icon
                  class="mr-1"
                  small
                >
                  mdi-clock-outline
                </v-icon>

                <span
                  class="text-caption grey--text font-weight-light"
                  v-text="chart.time"
                />
              </template>
            </material-chart-card>
          </v-col>
        </v-row>
      </v-col>

      <v-col
        v-for="({ actionIcon, actionText, ...attrs }, i) in stats"
        :key="i"
        cols="12"
        md="6"
        lg="3"
      >
        <material-stat-card v-bind="attrs">
          <template #actions>
            <v-icon
              class="mr-2"
              small
              v-text="actionIcon"
            />
            <div class="text-truncate">
              {{ actionText }}
            </div>
          </template>
        </material-stat-card>
      </v-col>

      <!-- <v-col
        cols="12"
        md="6"
      >
        <material-card
          color="orange"
          full-header
        >
          <template #heading>
            <div class="pa-8 white--text">
              <div class="text-h4 font-weight-light">
                Employees Stats
              </div>
              <div class="text-caption">
                New employees on 15th September, 2016
              </div>
            </div>
          </template>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="items"
            />
          </v-card-text>
        </material-card>
      </v-col> -->

      <!-- <v-col
        cols="12"
        md="6"
      >
        <material-card
          color="accent"
          full-header
        >
          <template #heading>
            <v-tabs
              v-model="tabs"
              background-color="transparent"
              slider-color="white"
              class="pa-8"
            >
              <span
                class="subheading font-weight-light mx-3"
                style="align-self: center"
              >Tasks:</span>
              <v-tab class="mr-3">
                <v-icon class="mr-2">
                  mdi-bug
                </v-icon>
                Bugs
              </v-tab>
              <v-tab class="mr-3">
                <v-icon class="mr-2">
                  mdi-code-tags
                </v-icon>
                Website
              </v-tab>
              <v-tab>
                <v-icon class="mr-2">
                  mdi-cloud
                </v-icon>
                Server
              </v-tab>
            </v-tabs>
          </template>
          <v-tabs-items
            v-model="tabs"
            background-color="transparent"
          >
            <v-tab-item
              v-for="n in 3"
              :key="n"
            >
              <v-card-text>
                <template v-for="(task, i) in tasks[tabs]">
                  <v-row
                    :key="i"
                    align="center"
                    class="flex-nowrap"
                  >
                    <v-col cols="1">
                      <v-list-item-action>
                        <v-simple-checkbox
                          v-model="task.value"
                          color="secondary"
                        />
                      </v-list-item-action>
                    </v-col>

                    <v-col
                      class="font-weight-light"
                      cols="8"
                      v-text="task.text"
                    />

                    <v-col
                      cols="auto"
                      class="text-right"
                    >
                      <v-icon class="mx-1">
                        mdi-pencil
                      </v-icon>

                      <v-icon
                        class="mx-1"
                        color="error"
                      >
                        mdi-close
                      </v-icon>
                    </v-col>
                  </v-row>
                </template>
              </v-card-text>
            </v-tab-item>
          </v-tabs-items>
        </material-card>
      </v-col> -->
    </v-row>
  </v-container>
</template>

<script>
  // Utilities
  import { get } from 'vuex-pathify'
  import Vue from 'vue'

  const lineSmooth = Vue.chartist.Interpolation.cardinal({
    tension: 0,
  })

  export default {
    name: 'DashboardView',
    data: () => ({
      charts: [{
        type: 'Bar',
        color: 'primary',
        title: '上位質問カテゴリ',
        subtitle: '',
        time: '',
        data: {
          labels: [],
          series: [
            [],
          ],
        },
        options: {
          axisX: {
            showGrid: false,
          },
          low: 0,
          high: 0,
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
              labelInterpolationFnc: function (value) {
                return value[0]
              },
            },
          }],
        ],
      }, {
        type: 'Line',
        color: 'success',
        title: '日付別質問数',
        subtitle: '',
        time: 'updated 4 minutes ago',
        data: {
          labels: [],
          series: [
            [],
          ],
        },
        options: {
          lineSmooth,
          low: 0,
          high: 0, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0,
          },
        },
      // }, {
      //   type: 'Line',
      //   color: 'info',
      //   title: 'Completed Tasks',
      //   subtitle: 'Last Campaign Performance',
      //   time: 'campaign sent 26 minutes ago',
      //   data: {
      //     labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'],
      //     series: [
      //       [12, 17, 7, 17, 23, 18, 38],
      //     ],
      //   },
      //   options: {
      //     lineSmooth,
      //     low: 0,
      //     high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
      //     chartPadding: {
      //       top: 0,
      //       right: 0,
      //       bottom: 0,
      //       left: 0,
      //     },
      //   },
      }],
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
      stats: [
        {
          actionIcon: '',
          actionText: '',
          color: '#FD9A13',
          icon: 'mdi-message-question-outline',
          title: '総質問数',
          value: '',
        },
        {
          actionIcon: '',
          actionText: '',
          color: 'primary',
          icon: 'mdi-podium-gold',
          title: '最多質問',
          value: '75.521',
        },
        {
          actionIcon: '',
          actionText: '',
          color: 'success',
          icon: 'mdi-message-question-outline',
          title: '総質問数(7日間)',
          value: '',
        },
        {
          actionIcon: '',
          actionText: '',
          color: 'info',
          icon: 'mdi-podium-gold',
          title: '最多質問(７日間)',
          value: '+245',
        },
      ],
      // tabs: 0,
      tasks: {
        0: [
          {
            text: 'Sign contract for "What are conference organizers afraid of?"',
            value: true,
          },
          {
            text: 'Lines From Great Russian Literature? Or E-mails From My Boss?',
            value: false,
          },
          {
            text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
            value: false,
          },
          {
            text: 'Create 4 Invisible User Experiences you Never Knew About',
            value: true,
          },
        ],
        1: [
          {
            text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
            value: true,
          },
          {
            text: 'Sign contract for "What are conference organizers afraid of?"',
            value: false,
          },
        ],
        2: [
          {
            text: 'Lines From Great Russian Literature? Or E-mails From My Boss?',
            value: false,
          },
          {
            text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
            value: true,
          },
          {
            text: 'Sign contract for "What are conference organizers afraid of?"',
            value: true,
          },
        ],
      },
      todayDate: "",
      oneWeekAgoDate: ""
    }),
    methods: {
      getChartMax(max) {
        if (max < 5) {
          return 5;
        } else if (max < 10) {
          return 10;
        } else if (max < 25) {
          return 25;
        } else if (max < 50) {
          return 50;
        } else if (max < 100) {
          return 100;
        }
        return max + 100;
      },
      setBarGraphValue(data) {
        var labels = [];
        data.forEach(val => {
          var find = labels.find(value => value.label == val.label);
          if (find == undefined) {
            labels.push({'label': val.label, num: 1});
          } else {
            find.num++;
          }
        });

        // 数順ソート
        labels.sort(function (a, b) {
          return b.num - a.num;
        });

        var setLabels = [];
        var setNum = [];
        for (var i = 0; i < labels.length; i++) {
          if (i >= 10) {
            break;
          }
          setLabels.push(labels[i].label);
          setNum.push(labels[i].num);
        }
        this.$set(this.stats[3], 'value', setLabels[0]);
        this.$set(this.charts[0].options, 'high', this.getChartMax(setNum[0]));
        this.$set(this.charts[0].data, 'labels', setLabels);
        this.$set(this.charts[0].data.series, 0, setNum);
        this.$set(this.charts[0], 'time', this.oneWeekAgoDate + ' ~ ' + this.todayDate);
      },
      setLineGraphValue(data) {
        var labels = [];
        var start = new Date(this.oneWeekAgoDate);
        for (var i = 0; i < 7; i++) {
          labels.push({'label': start.getMonth() + 1 + '/' + start.getDate(), 'checkDate':  start.getMonth() + 1 + '-' + start.getDate(), 'num': 0});
          start.setDate(start.getDate() + 1);
        }
        labels.forEach(label => {
          data.forEach(val => {
            if (val.question.createdAt.indexOf(label.checkDate) > -1) {
              label.num++;
            }
          });
        });
        var setLabels = [];
        var setNum = [];
        for (var i = 0; i < labels.length; i++) {
          if (i >= 10) {
            break;
          }
          setLabels.push(labels[i].label);
          setNum.push(labels[i].num);
        }

        // 数順ソート
        labels.sort(function (a, b) {
          return b.num - a.num;
        });
        this.$set(this.charts[1].options, 'high', this.getChartMax(labels[0].num));
        this.$set(this.charts[1].data, 'labels', setLabels);
        this.$set(this.charts[1].data.series, 0, setNum);
        this.$set(this.charts[1], 'time', this.oneWeekAgoDate + ' ~ ' + this.todayDate);
      },
      getToday() {
        var today = new Date();
        this.todayDate = today.getUTCFullYear() + '/' + (today.getUTCMonth() + 1) + "/" + today.getUTCDate();
        return today.getUTCFullYear() + '-' + (today.getUTCMonth() + 1) + "-" + today.getUTCDate() + 'T23:59:59';
      },
      getOneWeekAgo() {
        var today = new Date();
        today.setDate(today.getDate() - 6);
        this.oneWeekAgoDate = today.getUTCFullYear() + '/' + (today.getUTCMonth() + 1) + "/" + today.getUTCDate();
        return today.getUTCFullYear() + '-' + (today.getUTCMonth() + 1) + "-" + today.getUTCDate() + 'T00:00:00';
      },
      getOneWeekData() {
        var today = this.getToday();
        var oneWeekAgo = this.getOneWeekAgo();
        fetch('https://f6jm3ys7og.execute-api.ap-northeast-1.amazonaws.com/group7/answer?start=' + oneWeekAgo + '&end=' + today)
        .then(response => response.text())
        .then(data => {
          var graphData =  JSON.parse(data);
          this.setBarGraphValue(graphData);
          this.setLineGraphValue(graphData);
          this.$set(this.stats[2], 'value', String(graphData.length));
        });
      },
      getAllData() {
        fetch('https://f6jm3ys7og.execute-api.ap-northeast-1.amazonaws.com/group7/answer')
        .then(response => response.text())
        .then(data => {
          var graphData =  JSON.parse(data);
          this.$set(this.stats[0], 'value', String(graphData.length));
          var labels = [];
          graphData.forEach(val => {
            var find = labels.find(value => value.label == val.label);
            if (find == undefined) {
              labels.push({'label': val.label, num: 1});
            } else {
              find.num++;
            }
          });

          // 数順ソート
          labels.sort(function (a, b) {
            return b.num - a.num;
          });
          this.$set(this.stats[1], 'value', labels[0].label);
        });
      }
    },
    mounted() {
      // ビュー全体がレンダリングされた後にのみ実行されるコード
      this.getOneWeekData();
      this.getAllData();
    },
    computed: {
      sales: get('sales/sales'),
      totalSales () {
        return this.sales.reduce((acc, val) => acc + val.salesInM, 0)
      },
    },
  }
</script>