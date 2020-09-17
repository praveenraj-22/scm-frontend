<template>
  <v-container
    fluid
    fill-height
    class="grey lighten-3"
  >
    <v-slide-y-transition mode="out-in">
      <v-layout
        row
        wrap
      >
        <v-flex
          xs12
          sm10
          offset-sm1
          md10
          offest-md1
          lg10
          offset-lg1
        >
          <v-toolbar
            flat
            color="grey lighten-2"
          >
            <v-toolbar-title>Petty Cash Details</v-toolbar-title>
            <v-divider
              class="mx-2 black"
              inset
              vertical
            ></v-divider>
            <v-spacer></v-spacer>

			<v-select
				v-model="selbranch"
				:items="branch"
				label="Branch"
				 item-text="shortCode"
				item-value="TEXT"

			></v-select>
				<v-spacer></v-spacer>
			<v-select
				  :items="status"
				  v-model="Selstatus"
				  label="Status:"
				   item-text="shortCode"
				   item-value="text"

				></v-select>
				<v-spacer></v-spacer>
            <v-menu
              absolute
              ref="menu"
              :close-on-content-click="false"
              v-model="menu"
              :nudge-right="40"
              :return-value.sync="date"
              lazy
              transition="scale-transition"
              offset-y
              full-width
              min-width="200px"
            >
              <v-text-field
                slot="activator"
                v-model="date"
                placeholder="Select Date"
                prepend-inner-icon="event"
                readonly
              ></v-text-field>
              <v-date-picker
                color="primary"
                v-model="date"
                no-title
                scrollable
                min="2020-04-01"
                :max="maxDate"
				        type=month
                background-color="grey"
                style="box-shadow:none"
              >
                <v-spacer></v-spacer>
                <v-btn
                  flat
                  color="primary"
                  @click="menu = false"
                  style="outline:none"
                >Cancel</v-btn>
                <v-btn
                  flat
                  color="primary"
                  @click="$refs.menu.save(date);"
                  style="outline:none"
                >Ok</v-btn>
              </v-date-picker>
            </v-menu>
			<v-btn rounded color="primary" dark @click="apiGetPettyCashDetails(date,selbranch,Selstatus)">Generate</v-btn>
            <!--<download-excel
              :data="json_data"
              :fields="json_fields"
              type="csv"
              :name="fileName"
              :fetch="downloadExcelSuper"
            >
              <v-btn
                fab
                flat
                medium
                color="black"
              >
                <v-tooltip bottom>
                  <v-icon
                    slot="activator"
                    color="green darken-4"
                  >fas fa-file-excel</v-icon>
                  <span>Export</span>
                </v-tooltip>
              </v-btn>
            </download-excel>-->
          </v-toolbar>
          <loading
            :active.sync="isLoading"
            :is-full-page="fullPage"
            color="#7f0000"
            loader="bars"
          ></loading>
		  <br><br><br>

		  <v-card-title>
            <v-toolbar-title></v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn rounded color="primary" dark @click="newBill()"  >New Bill</v-btn></td>


            <v-spacer></v-spacer>
            <v-text-field v-model="search" v-if="billdata" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
          </v-card-title>





          <!-- Vuetify Data table -->
             <v-data-table :headers="headers" :items="billdata" v-model="selected" :search="search" class="elevation-1">
            <template slot="items" slot-scope="props">
              <tr @click="rowClick(props.item)">
                <td>{{ props.item.branch }}</td>
                <td class="text-xs-left">{{ props.item.voucher_no }}</td>
                <td class="text-xs-left">{{ props.item.category_name }}</td>
                <td class="text-xs-left">{{ props.item.bill_date }}</td>
                <td class="text-xs-left">{{ props.item.bill_submission }}</td>
                <td class="text-xs-left">{{ props.item.debit }}</td>
                <td class="text-xs-left">{{props.item.status | statusVal}}</td>
				<td class="text-xs-left">
					<v-btn slot="activator" small fab color="primary" @click="downloadvouchher(props.item.voucher_attach)">
                    <v-icon>cloud_download</v-icon>
                  </v-btn>

				<td class="text-xs-left" v-if="!(props.item.bill_attch==='NA')">

				  <v-btn slot="activator" small fab color="primary" @click="downloadbill(props.item.bill_attch)">
                    <v-icon>cloud_download</v-icon>
                  </v-btn>
				</td>
				<td class="text-xs-left" v-else="(props.item.bill_attch==='NA')">
					NA
				</td>



			</tr>
			</template>
			</v-data-table>


          <back-to-top
            bottom="90px"
            right="90px"
          >
            <v-btn
              class="red darken-4"
              dark
              absolute
              fab
              small
            >
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
import { serverBus } from "../main";
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

    branch :[],
	status: [
        { shortCode: 'Select All', text: ''},
		{  shortCode: 'SCHPending', text: 'SCHPending' },
        { shortCode: 'SCHApproved', text: 'SCHApproved' },
		{ shortCode: 'SCHReject', text: 'SCHReject' },
		{ shortCode: 'FinancePending', text: 'FinancePending' },
		{ shortCode: 'FinanceApproved', text: 'FinanceApproved' },
		{ shortCode: 'FinanceReject', text: 'FinanceReject' }

      ],
	headers: [{
        text: 'Branch',
        align: 'left',
        sortable: false,
        value: 'branch'

      },
      {
        text: 'Voucher No',
        value: 'voucher_no'
      },
      {
        text: 'Category',
        value: 'category_name'
      },
      {
        text: 'Bill/ Expense Date',
        value: 'bill_date'
      },
      {
        text: 'Submission Date',
        value: 'bill_submission'
      },
      {
        text: 'Amount',
        value: 'debit'
      },
      {
        text: 'Status',
        value: 'status'
      },
	  {
        text: 'voucher',
        value: ''
      },
	  {
        text: 'Bill',
        value: ''
      }
    ],
	search: '',
	billdata: null,
    userName: null,
    isLoading: false,
    fullPage: true,
    dialog: false,
    dialogdata: null,
    title: null,
    show: false,
    loading: false,
    menu: false,
    today: "",
    fileName: null,
  maxDate: curday('-'),
  }),
  created () {
    this.getToday();
	this.loadbranch();
  },
  computed: {

  },
  methods: {
    getToday () {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },
	loadbranch() {
		  let userid = JSON.parse(sessionStorage.getItem("normal_user"));
          var arr1 = [{TEXT: '',shortCode: 'Select All', }];
		  this.axios
			.get(`http://localhost:8888/api-chbranch/${userid.userName}`).then(response => {
			  console.log(response.data);
			  this.branch = arr1.concat(response.data);
			  //alert(JSON.stringify(this.branch));
			  console.log(this.branch);
			})
	},
    apiGetPettyCashDetails (date,selbranch,Selstatus) {
      let userid = JSON.parse(sessionStorage.getItem("normal_user"));
		  if (!this.date) {
			alert("Please Select Date");
			return false;
		  }
		  var statusvalue='';
		  var branchvalue='';
		  if (this.Selstatus=='') {
			statusvalue = 'undefined';
		  }else{
			statusvalue = this.Selstatus;
		  }
		  if (this.selbranch=='') {
			branchvalue = 'undefined';
		  }else{

			branchvalue = this.selbranch;
		  }


		  /*alert(selbranch);
		  alert(Selstatus);
		  alert(userid.name);*/

		  //alert(branchvalue);
		  //alert(statusvalue);

        //this.loading = true;
        //this.isLoading = true;
        this.$http
          .get(`http://localhost:8888/api-petty-cash-details/${userid.name}/${branchvalue}/${statusvalue}/${date}`) // https need for server
          //.get(`https://mis.dragarwal.com/api-petty-cash-details/${userid.name}/${branchvalue}/${statusvalue}/${date}`)
          .then(response => {
            this.isLoading = false;

			//alert("ZZZZZZZZZZZz");
			//alert(response.data.ResponseMsg);
			this.billdata = response.data.ResponseMsg;
          });
    },
	downloadvouchher(filename) {
      this.axios({
        url: `http://localhost:8888/api-voucher-download/${filename}`,
		//url: `https://mis.dragarwal.com/api-voucher-download/${filename}`,
        method: 'GET',
        responseType: 'blob',
      }).then(response => {


        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement('a');

        fileLink.href = fileURL;
        fileLink.setAttribute('download', filename);
        document.body.appendChild(fileLink);
         //con
        fileLink.click();
      })

    },
	downloadbill(filename) {
      this.axios({
        url: `http://localhost:8888/api-bill-download/${filename}`,
		//url: `https://mis.dragarwal.com/api-bill-download/${filename}`,
        method: 'GET',
        responseType: 'blob',
      }).then(response => {
        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement('a');
        fileLink.href = fileURL;
        fileLink.setAttribute('download', filename);
        document.body.appendChild(fileLink);
         //con
        fileLink.click();
      })

    },
	newBill() {
		serverBus.$emit('changeComponent', 'perrycashform')
	}
   },
   filters: {
    statusVal (num) {
      if(num==0 || num==1){
			return 'SchPending';
	  }else if(num==2){
			return 'SchApproved / FinPending';
	  }else if(num==3){
			return 'SchReject';
	  }else if(num==4){
			return 'Finance Approved';
	  }else if(num==5){
			return 'Finance Reject';
	  }
    }
  }

};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
button.v-btn.v-btn--active.v-btn--icon.v-btn--floating.theme--light.primary::after {
  outline: none;
  box-shadow: none;
}
.btn:focus,
.btn.focus {
  outline: none;
  box-shadow: none;
}

h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.customTable {
  table-layout: fixed;
  width: 100px;
  height: 100px;
  border-collapse: collapse;
}
table#stickyHeader thead {
  border-top: none;
  border-bottom: none;
  background-color: #000;
}
.table-striped > tbody > tr:nth-child(2n + 2) > td,
.table-striped > tbody > tr:nth-child(2n + 2) > th {
  background-color: #e5e5f2;
}
.allindiagroup {
background-color : #f0ae19!important
}
.branchesgrp{
background-color : #7083a9!important
}
.rotgrp{
background-color : #264e99!important
}
.whitefont{
font-weight: 900!important;
color : #ffffff!important
}
.ochfont{
background-color : #f9e699!important;
font-weight: 900!important;
color : #1d1d1d!important
}
</style>
