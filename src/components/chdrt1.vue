<template>
<v-container fluid fill-height class="grey lighten-3">
  <v-slide-y-transition mode="out-in">
    <v-layout row wrap>
      <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
        <v-toolbar flat color="grey lighten-2">

          <v-spacer></v-spacer>
          <v-select :items=entities v-model="SetVisit" label="Visit Type" id="SelEntity" item-text="shortCode" item-value="text"></v-select>


          <v-spacer></v-spacer>
          <th width="20%">
            <v-select :items=branch v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-select>
          </th>
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

          <v-btn rounded color="primary" dark @click="apiRequestdrtbill(fromdate,todate,SetVisit,SetBranch)">Generate</v-btn>

        </v-toolbar>
        <loading :active.sync="isLoading" :is-full-page="fullPage" color="#7f0000" loader="bars"></loading>
        <!-- Vuetify Data table -->

        <template>
          <v-card-title>
            <v-toolbar-title>DRT </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-text-field v-model="search" v-if="billdata" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
          </v-card-title>

          <v-data-table :headers="headers" :items="billdata" v-model="selected" :search="search" class="elevation-1">
            <template slot="items" slot-scope="props">
              <tr @click="rowClick(props.item)">
                <td>{{ props.item.BILLED }}</td>
                <td class="text-xs-left">{{ props.item.TRANSACTION_DATE }}</td>
                <td class="text-xs-left">{{ props.item.BILLNO }}</td>
                <td class="text-xs-left">{{ props.item.MRN }}</td>
                <td class="text-xs-left">{{ props.item.PATIENT_NAME }}</td>
                <td class="text-xs-left">{{ numberformat(props.item.TOTAL_AMOUNT) }}</td>
                <td class="text-xs-left">{{props.item.status}}</td>
                <td class="text-xs-right">

                  <v-layout row justify-center>
                    <v-dialog v-model="dialog" persistent max-width="800px" lazy absolute>
                      <v-btn slot="activator" small fab color="success" @click="rowClick1(props.item)">
                        <v-icon>edit</v-icon>
                      </v-btn>

                      <v-card>
                        <v-card-title>
                          <span class="headline">Information</span>

                        </v-card-title>
                        <v-card-text>
                          <v-container grid-list-md>
                            <v-layout wrap>
                              <v-flex xs12 sm6 md4>
                <td class="text-xs-left">Mrn : {{ Mrn }}</td>
      </v-flex>
      <v-flex xs12 sm6 md4>
        <td class="text-xs-left">Name : {{ Name }}</td>
      </v-flex>
      <v-flex xs12 sm6 md4>
        <td class="text-xs-left">Visited date : {{ vdate }}</td>
      </v-flex>
      <v-flex xs12 sm6 md4>
        <td class="text-xs-left">Ref Type : {{ reftype }}</td>
      </v-flex>
      <v-flex xs12 sm6 md4>
        <td class="text-xs-left">Ref Sub : {{ refsub }}</td>
      </v-flex>
      <v-flex xs12 sm6 md4>
        <td class="text-xs-left">Ref By : {{ refby }}</td>
      </v-flex>
      <v-flex xs12 sm6>
        <td class="text-xs-left">Bill No : {{ Billno }}</td>

      </v-flex>
      <v-flex xs12 sm6>
        <td class="text-xs-left">Bill Date : {{ vdate }}</td>
      </v-flex>
      <v-flex xs12 sm6>
        <td class="text-xs-left">Bill Amount : {{ billamount}}</td>
      </v-flex>
      <v-flex xs12 sm6>
        <td class="text-xs-left">Net Amount : {{ netamount }}</td>
      </v-flex>
      <v-flex xs12>
        <div class="table-responsive">
          <table align="center" class="table table-hover table-bordered" v-if="show">
            <thead>
              <tr>
                <th class="text-left">Service/Item</th>
                <th class="text-left">QTY</th>
                <th class="text-left">Amount</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in drtbilldetail" :key="item.name">
                <td>{{ item.ITEMNAME }}</td>
                <td>{{ item.QUANTITY }}</td>
                <td>{{ item.NET_AMOUNT }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </v-flex>
      <v-flex xs12 sm6>
        <v-autocomplete clearable :items="drtall" v-model="drt1" label="Select DRT" required item-text="Name" item-value="ID" v-on:change='getDRTdetail'></v-autocomplete>
      </v-flex>
      <v-flex xs12 sm6>
        <v-autocomplete clearable :items=category label="Category" required item-text="shortCode" item-value="text" v-on:change='getDRTcategory'></v-autocomplete>
      </v-flex>
      <v-flex xs12 sm6>
        <v-text-field v-model="panno" clearable label="Pan No" disabled></v-text-field>
      </v-flex>
      <v-flex xs12 sm6>
        <v-text-field type='number' disabled v-model="aggcommission" label="Agreed %" required></v-text-field>
      </v-flex>
      <v-flex xs12 sm6>
        <v-text-field type="text" clearable v-model="drtcommission" label="DRT %" @change="drtcommissionvalue" required></v-text-field>
      </v-flex>
      <v-flex xs12 sm6>
        <v-text-field type="text" clearable v-model="drtamount" label="Amount" @change="drtamountvalue" required></v-text-field>
      </v-flex>
      <v-flex xs12 sm6>
        <v-textarea clearable clear-icon="cancel" label="Comments" v-model='drtcomments'></v-textarea>
      </v-flex>

    </v-layout>
</v-container>
<small>*indicates required field</small>
</v-card-text>
<v-card-actions>
  <v-spacer></v-spacer>
  <v-btn color="blue darken-1" flat @click="dialog = false">Close</v-btn>
  <v-btn color="blue darken-1" v-model='buttonstatus' v-if="approval" flat @click="apiinsertbill(billid,netamount,aggcommission,drtcommission,drtamount,drtid,drtcategory,drtcomments,buttonstatus)">Submit</v-btn>

</v-card-actions>
</v-card>



</v-dialog>
</v-layout>

</td>
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
  var dd = today.getDate() - 1;
  var mm = today.getMonth() + 1; //As January is 0.
  var yyyy = today.getFullYear();

  if (dd < 10) dd = '0' + dd;
  if (mm < 10) mm = '0' + mm;
  return (yyyy + sp + mm + sp + dd);
};


export default {


  data: () => ({

    approval: true,
    Mrn: '',
    search: '',
    Name: '',
    vdate: '',
    reftype: '',
    refby: '',
    Billno: '',
    vdate: '',
    billamount: '',
    netamount: '',
    refsub: '',
    fNumber: '',
    servicename: '',
    branchcode: '',
    commissions: '',
    billid: '',
    drtid: '',
    drtcusid:'',
    drtcomments: '',
    aggcommission: '',
    drtcomments: '',
    drtcommission: '',
    drtbilldate:'',
    drttable:'',
    drtamount: '',
    drt: '',
    drtall:'',
    buttonstatus: '',
    drtcategory: '',
    selected: [],
    gstin: [],
    panno: [],
    billedbranch:[],
    drtpertcent: [],
    aggcommission: [],
    drtcusid:[],
    commission: [],
    total: [],
    dialog: false,
    test: true,
    GSTIN: '',
    headers: [{
        text: 'Branch',
        align: 'left',
        sortable: false,
        value: 'BILLED'

      },
      {
        text: 'Date',
        value: 'TRANSACTION_DATE'
      },
      {
        text: 'Bill No',
        value: 'BILLNO'
      },
      {
        text: 'MRN',
        value: 'MRN'
      },
      {
        text: 'Name',
        value: 'PATIENT_NAME'
      },
      {
        text: 'Amount',
        value: 'NET_AMOUNT'
      },
      {
        text: 'Status',
        value: 'Status'
      },
      {
        text: 'Apply',
        value: ''
      }
    ],
    category: [{
        shortCode: 'GPC',
        text: 'GPC'
      },
      {
        shortCode: 'IPC',
        text: 'IPC'
      },
      {
        shortCode: 'DPC',
        text: 'DPC'
      },
      {
        shortCode: 'YPP',
        text: 'YPP'
      },
      {
        shortCode: 'IC',
        text: 'IC'
      }
    ],
    message1: '',
    minDate: "2020-01-01",
    maxDate: curday('-'),
    entities: [{
        shortCode: 'Select All',
        text: 'All'
      },
      {
        shortCode: 'OP',
        text: 'OP'
      },
      {
        shortCode: 'IP',
        text: 'IP'
      },

    ],
    branch: [{
        shortCode: 'Select All',
        text: ''
      }

    ],

    SetBranch: [],
    SetVisit: [],
    userName: null,
    isLoading: false,
    fullPage: true,

    title: null,
    user_role: null,

    rowColor: null,
    isActive: false,
    save: "save",
    show: false,
    fileDate: null,
    loading: false,
    fromdate: null,
    todate: null,
    menu: false,
    menu1: false,
    modal: false,
    menu2: false,
    today: "",
    collection: null,
    billdata: null,
    billstatus: null,
    json_data: null,
    json_meta: [{
      key: "charset",
      value: "utf-8"
    }],
    json_fields: {
      "PARENT BRANCH": "PARENT_BRANCH",
      "BRANCH": "BRANCH",
      "PATIENT MRN": "PATIENT_MRN",
      "PATIENT NAME": "PATIENT_NAME",
      "PAYMENT OR REFUND_DATE": "PAYMENT_OR_REFUND_DATE",
      "BILL NO": "BILL_NO",
      "RECEIPT NO": "RECEIPT_NO",
      "CASH AMOUNT": "CASH_AMOUNT",
      "CARD AMOUNT": "CARD_AMOUNT",
      "CARD SERVICE CHARGE_AMOUNT": "CARD_SERVICE_CHARGE_AMOUNT",
      "CHEQUE AMOUNT": "CHEQUE_AMOUNT",
      "FUND TRANSFER AMOUNT": "FUND_TRANSFER_AMOUNT",
      "PAYTM AMOUNT": "PAYTM_AMOUNT",
      "DD AMOUNT": "DD_AMOUNT",
      "REFUND CASH AMOUNT": "REFUND_CASH_AMOUNT",
      "REFUND CARD AMOUNT": "REFUND_CARD_AMOUNT",
      "REFUND CHEQUE AMOUNT": "REFUND_CHEQUE_AMOUNT",
      "CREDIT CHEQUEAMOUNT": "CREDIT_CHEQUE_AMOUNT",
      "CREDIT CASHAMOUNT": "CREDIT_CASH_AMOUNT",
      "PAYTM CASHAMOUNT": "PAYTM_CASH_AMOUNT",
      "PAYTM FUNDAMOUNT": "PAYTM_FUND_AMOUNT",
      "DEPARTMENT": "DEPARTMENT",
      "PAYMENT NATURE": "PAYMENT_NATURE",
      "PAYMENT MODE": "PAYMENT_MODE",
      "PAID AMOUNT": "PAID_AMOUNT",
      "CREATEDBY": "CREATEDBY",
      "PAYMENT REFERENCE": "PAYMENT_REFERENCE",
      "PAYMENT DETAIL": "PAYMENT_DETAIL"


    },
    fileName: null
  }),
  created() {
    this.getToday();

  },

  mounted() {
    this.loadbranch();
    //  this.loaddrt();
  },

  methods: {
    drtamountvalue(a) {
      this.drtamount = a;
      this.drtcommission = Math.round((this.drtamount / this.netamount) * 100);
      console.log("drtcommission : " + this.drtcommission);
    },
    drtcommissionvalue(b) {
      this.drtcommission = b;
      this.drtamount = ((this.drtcommission * this.netamount) / 100);
      console.log("drt amount---- : " + this.drtamount);
    },


    rowClick1(b) {
      this.drt=[];
      this.drt1=[];
      this.drtall = [];
      this.drtspname=[];
      this.billid = b.EXTERNAL_ID;
      console.log(this.billid);
      this.approval = true;
      this.isLoading = true;
      this.gstin = '';
        this.panno = '';

        this.commission = '';
      this.drtdetail = '';
      this.drtid = '';


      this.drtspname=null;
      this.drtcomments = '';
      this.aggcommission = '';
      this.drtamount = '';
      this.Billno='';
      this.billedbranch='';
      this.axios
        .get(`http://localhost:8888/api-billdrt/${this.billid}`).then(response => {
          this.drtbilldetail = response.data;
          console.log(this.drtbilldetail);
          this.Name = this.drtbilldetail[0]["PATIENT_NAME"]
          this.Mrn = this.drtbilldetail[0]["MRN"];
          this.vdate = this.drtbilldetail[0]["TRANSACTION_DATE"];
          this.reftype = this.drtbilldetail[0]["REFERRAL_TYPE"];
          this.refby = this.drtbilldetail[0]["REFERRAL_VALUE"];
          this.refsub = this.drtbilldetail[0]["REFERRAL_BY"];
          this.Billno = this.drtbilldetail[0]["BILLNO"];
          this.billamount = this.drtbilldetail[0]["TOTAL_AMOUNT"];
          this.billid = this.drtbilldetail[0]["EXTERNAL_ID"];
          this.netamount = this.drtbilldetail[0]["NET_AMOUNT"];
          this.buttonstatus = this.drtbilldetail[0]["status"];
          this.drtcomments = this.drtbilldetail[0]["Comments"];
          this.aggcommission = this.drtbilldetail[0]["Aggreed_percentage_value"];
          this.drtcommission = this.drtbilldetail[0]["Drt_percentage_value"];
          this.panno = this.drtbilldetail[0]["DRT_Pan"];
          this.billedbranch=this.drtbilldetail[0]['BILLED']
          this.drtamount = this.drtbilldetail[0]["Drt_amount"];
          this.drtbilldate=this.drtbilldetail[0]["TRANSACTION_DATE"];
          this.drtcusid=this.drtbilldetail[0]["DRT_Id"];

          console.log(this.buttonstatus);
          console.log(this.drtcusid);

          this.drtspname=[{'ID':this.drtcusid,'Name':this.drtbilldetail[0]["DRT_Name"]}];

          console.log(this.drtspname);

          this.drt1=this.drtspname.concat(this.drtall)


                    console.log("-------");
                    console.log(this.drt1);
                    console.log("-------");

          if((this.buttonstatus=='Submitted')||(this.buttonstatus=='0')||(this.buttonstatus=='Sch Cancelled')||(this.buttonstatus=='Admin Cancelled')){
          this.approval = true;
          }
          else {
            this.approval =false;
          }
          this.isLoading = false;

        });
      this.axios
        .get(`http://localhost:8888/api-drt`).then(response => {
          this.drtall = response.data;
          console.log(this.drtall);
          this.isLoading = true;
        });
      this.isLoading = false;

    },


    getDRTcategory(selectObj1) {
      this.drtcategory = selectObj1;
    },
    getDRTdetail(selectObj) {
      this.drtdetail = [];
      this.drtid = selectObj;
      this.axios
        .get(`http://localhost:8888/api-drtdetail/${selectObj}`).then(response => {
          this.drtdetail = response.data;
          console.log(this.drtdetail[0]["GSTIN"]);
          this.gstin = this.drtdetail[0]["GSTIN"]
          this.panno = this.drtdetail[0]["Pan_no"]
          this.aggcommission = this.drtdetail[0]["Percentage"]
          this.commission = this.drtdetail[0]["Percentage"]
          console.log(this.commissions = this.drtdetail[0]["Percentage"]);
          this.drtname = this.drtdetail[0]["Name"]
        })

    },

    loadbranch() {
      let userid = JSON.parse(sessionStorage.getItem("normal_user"));
      this.SetBranch = [];
      this.branch = [];
      var arr1 = [{
        shortCode: 'Select All',
        text: ''
      }];
      this.axios
        //  .get(`https://scm.dragarwal.com/api-branch/${selectObj}`).then(response =>{
        .get(`http://localhost:8888/api-chbranch/${userid.userName}`).then(response => {
          this.branch = arr1.concat(response.data);
          console.log(this.branch);
        })


    },
    getToday() {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },


    numberformat(number) {

      var fNumber = number;
      var sNumber = parseFloat(fNumber.toFixed(2)).toLocaleString('en-IN');
      return sNumber;
    },
    apiinsertbill(billid, netamount, aggcommission, drtcommission, drtamount, drtid, drtcategory, drtcomments, buttonstatus) {
      let bill_id = '';
      let net_amount = '';
      let drt_commission = '';
      let drt_amount = '';
      let drt_id = '';
      let drt_category = '';
      let drt_user = null;
      let drt_comments = '';
      let drt_aggcommission = '';
      let drt_billstatus = '';
      let drt_billed=''
      let drt_billno='';
      let drt_billdate='';
      let drt_name='';
      let drt_mrn='';
      let drt_ref="";
      console.log("billid : " + this.billid);
      console.log(" netamount : " + this.netamount);
      console.log("commission : " + this.drtcommission);
      console.log("drtamount : " + this.drtamount);
      console.log("drtid : " + this.drtid);
      console.log("drtcategory : " + this.drtcategory);
      console.log("comments : " + this.drtcomments);
      console.log("agg comments : " + this.aggcommission);
      console.log("status ; " + this.buttonstatus);
      console.log("billed branch : "+this.billedbranch);
      console.log("bill no : "+this.Billno);
      console.log("bill date : "+this.drtbilldate);
      console.log("Mrn : "+this.Mrn);
      console.log("name : "+this.Name);
      console.log("reference : "+this.refby);
      if (this.billid == '') {
        alert("please select DRT name");
        return false;
      } else if (this.drtcategory == '') {
        alert("please select Category")
        return false;
      } else if (this.drtcommission > 100) {
        alert("please enter valid drt percentage")
        return false;
      } else if (!(this.drtamount <= this.netamount)) {
        alert("Enter amount is greater than Net amount")
        return false;
      } else if(this.drtid =='')
      {
        this.drtid=null;
        alert('Please select the DRT name')
        return false;
      }
      else {
        let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));

        this.loading = true;
        this.isLoading = true;
        this.$http
          .post(`http://localhost:8888/api-drtbills`, {
            bill_id: billid,
            net_amount: netamount,
            drt_aggcommission: aggcommission,
            drt_commission: drtcommission,
            drt_amount: drtamount,
            drt_id: drtid,
            drt_category: drtcategory,
            drt_user: normalusername.name,
            drt_comments: drtcomments,
            drt_billed:this.billedbranch,
            drt_billno:this.Billno,
            drt_billdate:this.drtbilldate,
            drt_name:this.Name,
            drt_mrn:this.Mrn,
            drt_ref:this.refby,
          }).then(response => {
            console.log("response : " + response);
            this.isLoading = false;
            if (response.data.Datainserted === true) {
              alert("Drt amount sent for approval")
              this.approval = false;
              // this.gstin = '',
              //   this.panno = '';
              // this.aggcommission = '',
              //   this.commission = '';
              // this.drtdetail = '';
              // this.drtid = '';
              // this.drt = null;
              // this.drtcomments = '';
              // this.aggcommission = '';
              // this.drtamount = '';
              // this.datatable='inserted';
              // this.Billno='';
              // this.billedbranch='';
              console.log("this.SetVisit : "+this.SetVisit);
              console.log("this.SetBranch  ; "+this.SetBranch );
              console.log("this.fromdate : "+this.fromdate);
              console.log("ttodat : "+this.todate);

              let visit = '';
              let branch = '';
              let fromdate = '';
              let todate = '';
              if ((this.SetVisit == '') && (this.SetBranch == '')) {
                alert("if");
                visit = 'All';
                branch = 'All'
                let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                // alert(normalusername.name);

                this.loading = true;
                this.isLoading = true;
                this.$http

                  //    .get(`https://scm.dragarwal.com/api-opticals-super/${date}`)
                  //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                  .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                  .then(response => {
                    this.processDatabill(response.data);
                    this.isLoading = false;

                  });
                } else if ((this.SetVisit == '') || (this.SetBranch == '')) {
                  // alert('else if');

                  if (this.SetVisit == '') {
                    this.SetVisit = 'All'
                  } else if (this.SetBranch == '') {
                    this.SetBranch = 'All'
                  }

                  let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                  alert(normalusername.name);
                  branch = this.SetBranch;
                  visit = this.SetVisit;
                  this.loading = true;
                  this.isLoading = true;
                  this.$http
                    //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                    .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                    .then(response => {
                      this.processDatabill(response.data);
                      this.isLoading = false;


                    });


                } else {
                  // alert("else");
                  let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                  alert(normalusername.name);
                  branch = this.SetBranch;
                  visit = this.SetVisit;
                  this.loading = true;
                  this.isLoading = true;
                  this.$http

                    //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                    .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                    .then(response => {
                      this.processDatabill(response.data);
                      this.isLoading = false;

                    });



                }

              return true
            } else if (response.data.Datainserted === "Updated") {
              alert("the bill is updated successfully")
              this.approval = false;

              let visit = '';
              let branch = '';
              let fromdate = '';
              let todate = '';
              if ((this.SetVisit == '') && (this.SetBranch == '')) {
                alert("if");
                visit = 'All';
                branch = 'All'
                let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                // alert(normalusername.name);

                this.loading = true;
                this.isLoading = true;
                this.$http

                  //    .get(`https://scm.dragarwal.com/api-opticals-super/${date}`)
                  //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                  .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                  .then(response => {
                    this.processDatabill(response.data);
                    this.isLoading = false;

                  });
                } else if ((this.SetVisit == '') || (this.SetBranch == '')) {
                  // alert('else if');

                  if (this.SetVisit == '') {
                    this.SetVisit = 'All'
                  } else if (this.SetBranch == '') {
                    this.SetBranch = 'All'
                  }

                  let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                  alert(normalusername.name);
                  branch = this.SetBranch;
                  visit = this.SetVisit;
                  this.loading = true;
                  this.isLoading = true;
                  this.$http
                    //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                    .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                    .then(response => {
                      this.processDatabill(response.data);
                      this.isLoading = false;


                    });


                } else {
                  // alert("else");
                  let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
                  alert(normalusername.name);
                  branch = this.SetBranch;
                  visit = this.SetVisit;
                  this.loading = true;
                  this.isLoading = true;
                  this.$http

                    //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
                    .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
                    .then(response => {
                      this.processDatabill(response.data);
                      this.isLoading = false;

                    });



                }

              return true;
            } else {
              alert("please verify the entered data")
              return false
            }
          })
      }

    },
    apiRequestdrtbill(fromdate, todate, SetVisit, SetBranch) {
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


      if ((!this.fromdate) || (!this.todate)) {
        alert("Please Select Date");
        return false;
      } else {

        let visit = '';
        let branch = '';
        let fromdate = '';
        let todate = '';
        if ((this.SetVisit == '') && (this.SetBranch == '')) {
          alert("if");
          visit = 'All';
          branch = 'All'
          let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
          alert(normalusername.name);

          this.loading = true;
          this.isLoading = true;
          this.$http

            //    .get(`https://scm.dragarwal.com/api-opticals-super/${date}`)
            //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
            .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
            .then(response => {
              this.processDatabill(response.data);
              this.isLoading = false;

            });

        } else if ((this.SetVisit == '') || (this.SetBranch == '')) {
          alert('else if');

          if (this.SetVisit == '') {
            this.SetVisit = 'All'
          } else if (this.SetBranch == '') {
            this.SetBranch = 'All'
          }

          let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
          alert(normalusername.name);
          branch = this.SetBranch;
          visit = this.SetVisit;
          this.loading = true;
          this.isLoading = true;
          this.$http
            //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
            .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
            .then(response => {
              this.processDatabill(response.data);
              this.isLoading = false;


            });


        } else {
          alert("else");
          let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
          alert(normalusername.name);
          branch = this.SetBranch;
          visit = this.SetVisit;
          this.loading = true;
          this.isLoading = true;
          this.$http

            //.get(`https://scm.dragarwal.com/api-collection-super/${this.fromdate}/${this.todate}/${visit}/${branch}`)
            .get(`http://localhost:8888/api-chbills/${this.fromdate}/${this.todate}/${visit}/${branch}/${normalusername.name}`)
            .then(response => {
              this.processDatabill(response.data);
              this.isLoading = false;

            });



        }



        branch = this.SetBranch;
        visit = this.SetVisit;
        var str = "_"
        this.fileDate = this.fromdate.concat(str, this.todate);
        this.file = visit.concat(str, branch, str, this.fileDate)
        console.log(this.fileDate);

        this.fileName = `Collection_Report_${this.file}.csv`;


      }

    },

    processDatabill(data) {
      if (sessionStorage.getItem('super_user')) {
        this.user_role = 'super_user';
      } else if (sessionStorage.getItem('overseas_user')) {
        this.user_role = 'overseas_user';
      } else if (sessionStorage.getItem('indian_user')) {
        this.user_role = 'indian_user';
      } else if (sessionStorage.getItem('optical_user')) {
        this.user_role = 'optical_user';
      } else if (sessionStorage.getItem('coll_user')) {
        this.user_role = 'coll_user';
      }

      this.billdata = data.result['bill'];
      console.log(this.billdata);
      this.show = true;
    },

    downloadExcelCollectionSuper() {
      let tempDataArr = [];
      if (this.fileDate !== null) {
        tempDataArr = this.billdata

        return tempDataArr;
      } else {
        return null;
      }
    },

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
  touch-action: none;
}

table#stickyHeader thead {
  border-top: none;
  border-bottom: none;
  background-color: #000;
  touch-action: none;
}

.table-striped>tbody>tr:nth-child(2n + 2)>td,
.table-striped>tbody>tr:nth-child(2n + 2)>th {
  background-color: #e5e5f2;
  touch-action: none;
}

.allindiagroup {
  background-color: #f0ae19 !important
}

.targetcolor {
  background-color: #FCD12A !important
}

.targetachicolor {
  background-color: #87CEFA !important
}

.branchesgrp {
  background-color: #7083a9 !important
}

.rotgrp {
  background-color: #264e99 !important
}

.whitefont {
  font-weight: 900 !important;
  color: #ffffff !important
}

.ochfont {
  background-color: #f9e699 !important;
  font-weight: 900 !important;
  color: #1d1d1d !important
}

.inputPrice input[type='number'] {
  -moz-appearance: textfield;
}

.inputPrice input::-webkit-outer-spin-button,
.inputPrice input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
</style>
