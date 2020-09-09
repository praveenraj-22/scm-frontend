<template>
<v-container fluid fill-height class="grey lighten-3">
  <v-slide-y-transition mode="out-in">
    <v-layout row wrap>
      <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
        <v-toolbar flat color="grey lighten-2">

          <th width="20%">
            <v-select :items="branch" v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-select>
          </th>

          <v-spacer></v-spacer>
          <v-select :items="bill_status" v-model="SetStatus" label="Bill Status" id="SelEntity" item-text="shortCode" item-value="text"></v-select>

          <v-spacer></v-spacer>
          <v-menu absolute ref="menu1" :close-on-content-click="false" v-model="menu1" :nudge-right="40" :return-value.sync="fromdate" lazy transition="scale-transition" offset-y full-width min-width="150px">
            <v-text-field slot="activator" v-model="fromdate" placeholder="Select From Date" prepend-inner-icon="event" readonly></v-text-field>
            <v-date-picker color="primary" v-model="fromdate" no-title scrollable :min="minDate" :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="menu1 = false" style="outline:none">Cancel</v-btn>
              <v-btn flat color="primary" @click="$refs.menu1.save(fromdate)" style="outline:none">Ok</v-btn>
            </v-date-picker>
          </v-menu>

          <v-spacer></v-spacer>
          <v-menu absolute ref="menu2" :close-on-content-click="false" v-model="menu2" :nudge-right="40" :return-value.sync="todate" lazy transition="scale-transition" offset-y full-width min-width="150px">
            <v-text-field slot="activator" v-model="todate" placeholder="Select To Date" prepend-inner-icon="event" readonly></v-text-field>
            <v-date-picker color="primary" v-model="todate" no-title scrollable :min="minDate" :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="menu2 = false" style="outline:none">Cancel</v-btn>
              <v-btn flat color="primary" @click="$refs.menu2.save(todate)" style="outline:none">Ok</v-btn>
            </v-date-picker>
          </v-menu>

          <v-btn rounded color="primary" dark @click="apiRequestschpc(fromdate,todate,SetStatus,SetBranch)">Generate</v-btn>


        </v-toolbar>
        <loading :active.sync="isLoading" :is-full-page="fullPage" color="#7f0000" loader="bars"></loading>

        <!-- Vuetify Data table -->

        <template>
          <v-card-title>
            <v-toolbar-title>Petty Cash</v-toolbar-title>
            <v-spacer></v-spacer>



            <v-spacer></v-spacer>
            <v-text-field v-model="search" v-if="strch" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
          </v-card-title>

          <v-data-table :headers="mainHeaders"
                         :items="strch"
                         item-key="branch"
                         hide-actions
                         expand
                         :single-expand="singleExpand"
                         class="elevation-1">
            <template slot="items" scope="props">
              <tr @click="props.expanded = !props.expanded">
                <td class="text-xs">{{ props.item.branch }}</td>
                <td class="text-xs">{{ props.item.Bill_date }}</td>
              </tr>
            </template>
            <template slot="expand" scope="props">
              <v-card class="elevation-10">
                <v-card-text>

                  <v-data-table :headers="subHeaders"
                                :items="strch"
                                item-key="color"
                                hide-actions
                                class="elevation-10">
                    <template slot="items" scope="props">
                      <td class="text-xs">{{ props.item.Comments  }}</td>
                      <td class="text-xs">{{ props.item.status }}</td>
                    </template>
                  </v-data-table>

                </v-card-text>
              </v-card>
            </template>
           </v-data-table>



        </template>

        <back-to-top bottom="90px" right="90px">
          <v-btn class="red darken-4" dark absolute fab small>
            <v-icon>expand_less</v-icon>
          </v-btn>
        </back-to-top>
        <!-- end Data Tabel -->



      </v-flex>
    </v-layout>
  </v-slide-y-transition>
</v-container>
</template>

<script>
import moment from "moment";
import {
  serverBus
} from "@/main";

var curday = function(sp) {
  var today = new Date();
  var dd = today.getDate();
  var mm = today.getMonth() + 1; //As January is 0.
  var yyyy = today.getFullYear();

  if (dd < 10) dd = '0' + dd;
  if (mm < 10) mm = '0' + mm;
  return (yyyy + sp + mm + sp + dd);
};

export default {
  data: () => ({
    SetBranch: [],
    branch: [{
        shortCode: 'Select All',
        text: 'All'
      }

    ],
    SetStatus: [],
    bill_status: [{
        shortCode: 'Select All',
        text: 'All'
      },
      {
        shortCode: 'Pending',
        text: '1'
      },
      {
        shortCode: 'Approved',
        text: '2'
      },
      {
        shortCode: 'Approved By Finance',
        text: '4'
      },

      {
        shortCode: 'Cancelled',
        text: '3'
      },
      {
        shortCode: 'Cancelled By Finance',
        text: '5'
      },
    ],

    fromdate: null,
    todate: null,
    minDate: "2020-06-01",
    maxDate: curday('-'),
    menu1: false,
    menu2: false,
    isLoading: false,
    fullPage: true,
    mainHeaders: [
       { text: 'Name', value: 'name' },
       { text: 'Age', value: 'age' }
     ],
     subHeaders: [
       { text: 'Color', value: 'color' },
       { text: 'Value', value: 'value' }
     ],
     mainItems: [
       { name: 'Marc	Moreno', age: '30' },
       { name: 'Wallace	Frank', age: '31' },
       { name: 'Enrique	Sanders', age: '45' },
       { name: 'Alyssa	Butler', age: '23' },
       { name: 'Domingo	Gill', age: '29' },
       { name: 'Felix	May', age: '28' },
       { name: 'Bradford	Powell', age: '15' },
       { name: 'Hattie	Coleman', age: '17' },
       { name: 'Marty	Jordan', age: '23' },
       { name: 'Bobbie	Webb', age: '28' }
     ],
     subItems: [
       { color: 'Red', value: '1' },
       { color: 'Green', value: '2' },
       { color: 'Blue', value: '3' },
       { color: 'Black', value: '4' }
     ],
  singleExpand: false,
  }),
  mounted() {
    this.loadbranch();

  },
  methods: {

    rowClick(a, b) {
      console.log(a);
      console.log(b);
      b.index
      b.expanded = !b.expanded;
    },
    loadbranch() {
      let userid = JSON.parse(sessionStorage.getItem("normal_user"));
      this.SetBranch = [];
      this.branch = [];
      var arr1 = [{
        shortCode: 'Select All',
        text: 'All'
      }];
      this.axios
        //  .get(`https://scm.dragarwal.com/api-branch/${selectObj}`).then(response =>{
        .get(`http://localhost:8888/api-schbranch/${userid.userName}`).then(response => {
          this.branch = arr1.concat(response.data);
          console.log(this.branch);
        })


    },

    apiRequestschpc(fromdate, todate, SetStatus, SetBranch) {
      this.isLoading = true;
      let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
      console.log(normalusername);
      this.$http.get(`http://localhost:8888/api-schpc/${this.fromdate}/${this.todate}/${this.SetStatus}/${this.SetBranch}/${normalusername.name}`)
        .then(response => {
          this.isLoading = false;
          console.log(response.data);
          this.processliststrchdata(response.data);

        })

    },
    processliststrchdata(data) {
      this.strch = data.result["pcbill"]
      console.log(this.strch);
      this.show = true
    }


  }
}
</script>
