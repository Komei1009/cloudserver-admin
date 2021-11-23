<template>
  <v-container
    id="regular-tables-view"
    fluid
    tag="section"
  >
    <v-btn
      color="info"
      default
      rounded
      @click="dialog = true"
      style="float: left;"
    >
      検索条件
    </v-btn>
    <view-intro
      heading="質問一覧"
      style="margin-right: 80px;"
    />
    <material-card
      icon="mdi-clipboard-text"
      icon-small
      title="一覧"
      color="accent"
    >
      <v-simple-table>
        <thead>
          <tr>
            <th class="primary--text">
              画像
            </th>
            <th class="primary--text">
              カテゴリ
            </th>
            <th class="primary--text">
              登録日時
            </th>
            <!-- <th class="primary--text">
              City
            </th>
            <th class="text-right primary--text">
              Salary
            </th> -->
          </tr>
        </thead>

        <tbody>
          <tr v-for="list in listDatas" @click="imageView(list.question.image)">
            <td><img :src=list.question.image style="width: 200px" alt=""></td>
            <td><div style="min-width: 100px">{{list.label}}</div></td>
            <td>{{toDate(list.question.createdAt)}}</td>
          </tr>
        </tbody>
        <!-- <tbody>
          <tr>
            <td>1</td>
            <td>Dakota Rice</td>
            <td>Niger</td>
            <td>Oud-Turnhout</td>
            <td class="text-right">
              $36,738
            </td>
          </tr>

          <tr>
            <td>2</td>
            <td>Minverva Hooper</td>
            <td>Curaçao</td>
            <td>Sinaas-Waas</td>
            <td class="text-right">
              $23,789
            </td>
          </tr>

          <tr>
            <td>3</td>
            <td>Sage Rodriguez</td>
            <td>Netherlands</td>
            <td>Baileux</td>
            <td class="text-right">
              $56,142
            </td>
          </tr>

          <tr>
            <td>4</td>
            <td>Philip Chaney</td>
            <td>Korea, South</td>
            <td>Overland Park</td>
            <td class="text-right">
              $38,735
            </td>
          </tr>

          <tr>
            <td>5</td>
            <td>Doris Greene</td>
            <td>Malawi</td>
            <td>Feldkirchen in Kärnten</td>
            <td class="text-right">
              $63,542
            </td>
          </tr>

          <tr>
            <td>6</td>
            <td>Mason Porter</td>
            <td>Chile</td>
            <td>Gloucester</td>
            <td class="text-right">
              $78,615
            </td>
          </tr>
        </tbody> -->
      </v-simple-table>
    </material-card>

    <!-- <div class="py-3" />

    <material-card
      dark
      icon="mdi-clipboard-plus"
      icon-small
      title="Table on Dark Background"
      color="accent"
    >
      <v-simple-table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Country</th>
            <th>City</th>
            <th class="text-right">
              Salary
            </th>
          </tr>
        </thead>

        <tbody>
          <tr>
            <td>1</td>
            <td>Dakota Rice</td>
            <td>Niger</td>
            <td>Oud-Turnhout</td>
            <td class="text-right">
              $36,738
            </td>
          </tr>

          <tr>
            <td>2</td>
            <td>Minverva Hooper</td>
            <td>Curaçao</td>
            <td>Sinaas-Waas</td>
            <td class="text-right">
              $23,789
            </td>
          </tr>

          <tr>
            <td>3</td>
            <td>Sage Rodriguez</td>
            <td>Netherlands</td>
            <td>Baileux</td>
            <td class="text-right">
              $56,142
            </td>
          </tr>

          <tr>
            <td>4</td>
            <td>Philip Chaney</td>
            <td>Korea, South</td>
            <td>Overland Park</td>
            <td class="text-right">
              $38,735
            </td>
          </tr>

          <tr>
            <td>5</td>
            <td>Doris Greene</td>
            <td>Malawi</td>
            <td>Feldkirchen in Kärnten</td>
            <td class="text-right">
              $63,542
            </td>
          </tr>

          <tr>
            <td>6</td>
            <td>Mason Porter</td>
            <td>Chile</td>
            <td>Gloucester</td>
            <td class="text-right">
              $78,615
            </td>
          </tr>
        </tbody>
      </v-simple-table>
    </material-card> -->
    <v-dialog
      v-model="dialog"
      max-width="500"
    >
    <v-card>
        <v-card-title>
          検索条件

          <v-spacer />

          <v-icon
            aria-label="Close"
            @click="dialog = false"
          >
            mdi-close
          </v-icon>
        </v-card-title>

        <v-card-text class="text-body-1 text-center">
          <v-row>
            <template>
              <v-row>
                <v-col
                  cols="12"
                  sm="6"
                  md="6"
                >
                  <v-menu
                    ref="menu"
                    v-model="menu"
                    :close-on-content-click="false"
                    :return-value.sync="start"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="start"
                        label="登録日時(From)"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="start"
                      no-title
                      scrollable
                    >
                      <v-spacer></v-spacer>
                      <v-btn
                        text
                        color="primary"
                        @click="menu = false"
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menu.save(start)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </v-col>
                <!-- <v-spacer></v-spacer> -->
                <v-col
                  cols="12"
                  sm="6"
                  md="6"
                >
                  <v-menu
                    ref="menu2"
                    v-model="menu2"
                    :close-on-content-click="false"
                    :return-value.sync="end"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="end"
                        label="登録日時(To)"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="end"
                      no-title
                      scrollable
                    >
                      <v-spacer></v-spacer>
                      <v-btn
                        text
                        color="primary"
                        @click="menu2 = false"
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menu2.save(end)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </v-col>
              </v-row>
            </template>
            <v-col cols="12">
              <input class="form-control" type="text" placeholder="カテゴリ名" v-model="category" aria-label="label" :style="'width: 100%'"/>
            </v-col>
          </v-row>

          <v-btn
            class="mt-6"
            color="info"
            depressed
            default
            rounded
            @click="onSearch()"
          >
            検索
          </v-btn>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
  export default {
    name: 'RegularTablesView',
    data: () => ({
      listDatas: [],
      dialog: false,
      start: '',
      end: '',
      category: '',
      menu: false,
      modal: false,
      menu2: false,
    }),
    methods: {
      getWhere() {
        var where = '';
        if (this.start != '') {
          if (where == '') {
            where += "?start=" + this.start + 'T00:00:00';
          } else {
            where += "&start=" + this.start + 'T00:00:00';
          }
        }
        if (this.end != '') {
          if (where == '') {
            where += "?end=" + this.end + 'T23:59:59';
          } else {
            where += "&end=" + this.end + 'T23:59:59';
          }
        }
        if (this.category != '') {
          if (where == '') {
            where += "?label=" + this.category;
          } else {
            where += "&label=" + this.category;
          }
        }
        return where;
      },
      onSearch() {
        var where = this.getWhere();
        fetch('https://f6jm3ys7og.execute-api.ap-northeast-1.amazonaws.com/group7/answer' + where)
        .then(response => response.text())
        .then(data => {
          var tableData =  JSON.parse(data);
          this.$set(this, 'listDatas', tableData);
          this.dialog = false;
        });
      },
      getAnswerData() {
        fetch('https://f6jm3ys7og.execute-api.ap-northeast-1.amazonaws.com/group7/answer')
        .then(response => response.text())
        .then(data => {
          var tableData =  JSON.parse(data);
          this.$set(this, 'listDatas', tableData);
        });
      },
      toDate(date) {
        var toDate = new Date(date);
        toDate = toDate.toLocaleString();
        return toDate;
      },
      imageView(url) {
        window.open(url, '_blank');
      }
    },
    mounted() {
      this.getAnswerData();
    },
  }
</script>
