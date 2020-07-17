<template >
  <v-container fluid fill-height class="grey lighten-3">
    <v-slide-y-transition mode="out-in">
      <v-layout row wrap>
        <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
          <v-toolbar flat color="grey lighten-2">



            <th width="20%">
              <v-autocomplete clearable v-bind:items="branch" v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-autocomplete>
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

            <v-btn rounded color="primary" dark @click="apichpendingdrtbill(fromdate,todate,SetStatus,SetBranch)">Generate</v-btn>

            <download-excel :data="json_data" :fields="json_fields" type="csv" :name="fileName" :fetch="downloadExcelsubDrtbill">
              <v-btn fab flat medium color="black">
                <v-tooltip bottom>
                  <v-icon slot="activator" color="green darken-4">fas fa-file-excel</v-icon>
                  <span>Export</span>
                </v-tooltip>
              </v-btn>
            </download-excel>

          </v-toolbar>
          <loading :active.sync="isLoading" :is-full-page="fullPage" color="#7f0000" loader="bars"></loading>
          <!-- Vuetify Data table -->

          <template>
            <v-card-title>
              <v-toolbar-title>IC Bills</v-toolbar-title>
              <v-spacer></v-spacer>



              <v-spacer></v-spacer>
              <v-text-field v-model="search" v-if="subill" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
            </v-card-title>

            <v-data-table :headers="headers" :items="subill" v-model="selected" :search="search" v-bind:pagination.sync="pagination" class="elevation-4">
              <template slot="items" slot-scope="props">
                <tr>
                  <td @click="rowClick(props.item.drtApproval_status)">{{ props.item.drtApproval_status }}</td>
                  <td class="text-xs-left">{{ props.item.Bill_no }}</td>
                  <td class="text-xs-left">{{ props.item.bill_date }}</td>
                  <td class="text-xs-left">{{ props.item.Mrn }}</td>
                  <td class="text-xs-left">{{ props.item.Name }}</td>
                  <td class="text-xs-left">{{props.item.Net_amount }}</td>
                  <td class="text-xs-left">{{props.item.Reference }}</td>
                  <td class="text-xs-left">{{props.item.DRTNAME}}</td>
                  <td class="text-xs-left">{{props.item.Aggreed_percentage_value}}</td>
                  <td class="text-xs-left">{{props.item.Drt_percentage_value}}</td>
                  <td class="text-xs-left">{{props.item.Drt_amount}}</td>
                  <td class="text-xs-left">{{props.item.Comments}}</td>

                </tr>
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
} from "../main";


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
    branch: [{
        shortCode: 'Select All',
        text: 'All'
      }],
      bill_status: [{
          shortCode: 'Select All',
          text: 'All'
        },
        {
          shortCode: 'Approved',
          text: '2'
        },
        {
          shortCode: 'Approved By SCH',
          text: '1'
        },
        {
          shortCode: 'Cancelled By Finance',
          text: '4'
        },
        {
          shortCode: 'Cancelled by SCH',
          text: '3'
        },
        {
          shortCode: 'Need SCH Approval',
          text: '0'
        },
      ],
      headers: [{
          text: 'Status',
          align: 'left',
          sortable: false,
          value: 'status'

        },
        {
          text: 'Bill no',
          value: 'Bill_no'
        },
        {
          text: 'Date',
          value: 'bill_date'
        },

        {
          text: 'MRN',
          value: 'Mrn'
        },
        {
          text: 'Name',
          value: 'Name'
        },
        {
          text: 'Net Amount',
          value: 'Net_amount'
        },
        {
          text: 'Reference',
          value: 'Reference'
        },
        {
          text: 'DRT Name',
          value: 'DRT Name'
        },
        {
          text: 'Agreed %',
          value: 'Aggreed_percentage_value'
        },
        {
          text: 'Comm %',
          value: 'Drt_percentage_value'
        },
        {
          text: 'Comm Amt',
          value: 'Drt_amount'
        },
        {
          text: 'Comments',
          value: 'Comments'
        }


      ],json_data: null,
      json_meta: [{
        key: "charset",
        value: "utf-8"
      }],
      json_fields: {
        "Bill No": "Bill_no",
        "Bill Date": "bill_date",
        "Mrn": "Mrn",
        "Name": "Name",
        "Total Net Amount": "Net_amount",
        "Reference": "Reference",
        "DRT Name": "DRTNAME",
        "Agreed %": "Aggreed_percentage_value",
        "Comm %":"Drt_percentage_value",
        "Comm Amt":"Drt_amount",
        "Comments":"Comments",
        "Net Amount":"Net_amount",
        "Status":"drtApproval_status",
      },
      fileName: null,
      fromdate:null,
      todate:null,
      loading: false,
      isLoading: false,
      fullPage: true,
      minDate: "2020-01-01",
      maxDate: curday('-'),
      search: '',
      menu1: false,
      menu2: false,
      SetBranch: [],
      SetStatus: [],
      subill:null

  }),
  mounted(){
    this.loadbranch();
  },
  methods: {
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
        .get(`http://localhost:8888/api-chbranch/${userid.userName}`).then(response => {
          this.branch = arr1.concat(response.data);

        })


    },
        getToday() {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },
    apichpendingdrtbill(fromdate,todate,SetStatus,SetBranch){

      var date3 = new Date();
      var date4 = date3.getMonth() + "/" + date3.getDay() + "/" + date3.getYear();
      var currentDate = new Date(date4);


      if (fromdate > todate) {
        alert("From date should be less than todate");
        return false;
      } else if (fromdate > currentDate) {
        alert("From Date should be less than current date");
        return false;
      } else if (todate > currentDate) {
        alert("To Date should be less than current date");
        return false;
      }
      else if ((!this.fromdate) || (!this.todate)) {
        alert("Please Select Date");
        return false;
      }
      else if((this.SetStatus=='')||(this.SetBranch==''))
{
  alert("Please select status and Branch");
  return false;
}
else if((this.SetStatus===null)||(this.SetBranch==null))
{
alert("Please select status and Branch");
return false;
}

      let frmdate=this.fromdate;
      let todat=this.todate;
      let Status=this.SetStatus;
      let Branch=this.SetBranch;
      let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
      console.log(normalusername);
      this.isLoading=true;
      this.axios
      .get(`http://localhost:8888/api-submittedbills/${this.fromdate}/${this.todate}/${this.SetStatus}/${this.SetBranch}/${normalusername.name}`)
        .then(response =>{
          console.log(response.data);
            this.isLoading=false;
           this.processlistsubbill(response.data);
          // this.isLoading=false;
        })
        var str = "_"
        this.fileDate = this.fromdate.concat(str, this.todate);
        this.fileName = `IC_claim_bills_${this.fileDate}.csv`;

    },
    processlistsubbill(data){
      this.subill=data.result["subbill"]
      console.log(this.subill);
      this.show=true
    },
    downloadExcelsubDrtbill(){
      let tempDataArr = [];
      if (this.fileDate !== null) {
        tempDataArr = this.subill

        return tempDataArr;
      } else {
        return null;
      }
    }
  }
}
</script>
