<template>
<v-container fluid fill-height class="grey lighten-3">
  <v-slide-y-transition mode="out-in">
    <v-layout row wrap>
      <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
        <v-toolbar flat color="grey lighten-2">
          <v-toolbar-title>Stock Ledger </v-toolbar-title> <v-spacer></v-spacer>
          <v-divider class="mx-2 black" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-select :items=entities v-model="SetEntity" label="Entity:" id="SelEntity" item-text="shortCode" item-value="text" v-on:change='getBranches'></v-select>


          <v-spacer></v-spacer>
          <th width="20%">
            <v-select :items=branch v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-select>
          </th>
          <v-spacer></v-spacer>

          <!--<v-select :items="department" v-model="Setdepartment" label="Department type" item-text="shortCode" item-value="text"></v-select>-->

          <v-spacer></v-spacer>
          <v-menu absolute ref="menu1" :close-on-content-click="false" v-model="menu1" :nudge-right="40" :return-value.sync="fromdate" lazy transition="scale-transition" offset-y full-width min-width="150px">
            <v-text-field slot="activator" v-model="fromdate" placeholder="Select From Date" prepend-inner-icon="event" readonly></v-text-field>
            <v-date-picker color="primary" v-model="fromdate" type="month" no-title scrollable :min="minDate" :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="menu1 = false" style="outline:none">Cancel</v-btn>
              <v-btn flat color="primary" @click="$refs.menu1.save(fromdate)" style="outline:none">Ok</v-btn>
            </v-date-picker>
          </v-menu>


          <v-btn rounded color="primary" dark @click="apiRequestCogsdata(fromdate,SetEntity,SetBranch)">Generate</v-btn>



          <download-excel :data="json_data" :fields="json_fields" type="csv" :name="fileName" :fetch="downloadExcelCogsdata">
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
import { serverBus } from "../main";

var curday = function(sp) {
  var today = new Date();
  var dd = today.getDate() - 1;
  var mm = today.getMonth() + 1; //As January is 0.
  var yyyy = today.getFullYear();

  if (dd < 10) dd = '0' + dd;
  if (mm < 10) mm = '0' + mm;
  return (yyyy + sp + mm + sp + dd);
};

export default{
  data:()=>({
    minDate: "2020-04-01",
    maxDate: curday('-'),
    entities: [{
        shortCode: 'Select All',
        text: 'All'
      },
      {
        shortCode: 'AEH',
        text: 'AEH'
      },
      {
        shortCode: 'AHC',
        text: 'AHC'
      },
      {
        shortCode: 'OHC',
        text: 'OHC'
      },
    ],
    branch: [{
        shortCode: 'Select All',
        text: 'All'
      }],
      
      SetBranch: [],
      SetEntity: [],
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

      fromdate: null,
      todate: null,
      menu: false,
      menu1: false,

      Cogs: null,
      Cogslist: null,

      json_data: null,
      json_meta: [{
        key: "charset",
        value: "utf-8"
      }],
      json_fields: {
        "ENTITY":"DESCRIPTION",
		"BRANCH":"ORGANIZATIONNAME",
		"DATE":"PREVDATE",
		"ITEMID":"ITEMID",
		"ITEMNAME":"ITEMNAME",
		"ITEMCODE":"ITEMCODE",
		"REGION":"REGION",
		"SECONDCATEGORY":"SECONDCATEGORY",
		"ITEMCATEGORY":"ITEMCATEGORY",
		"SUBCATEGORY":"SUBCATEGORY",
		"UNITS":"UNITS",
		"MANUFACTURER":"MANUFACTURER",
		"COST_PRICE":"COST_PRICE",
		"UNITPRICE":"UNITPRICE",
		"DEPTID":"DEPTID",
		"DEPTNAME":"DEPTNAME",
		"OpeningStock":"OpeningStock",
		"OpeningValue":"OpeningValue",
		"Purchase":"Purchase",
		"PurchaseValue":"PurchaseValue",
		"PurchaseReturn":"PurchaseReturn",
		"PurchaseReturnValue":"PurchaseReturnValue",
		"Issues":"Issues",
		"IssuesValue":"IssuesValue",
		"SalesReturn":"SalesReturn",
		"SalesReturnValue":"SalesReturnValue",
		"StockAdjustment(T)":"StockAdjustment(T)",
		"StockAdjustmentValue(T)":"StockAdjustmentValue(T)",
		"StockAdjustment(O)":"StockAdjustment(O)",
		"StockAdjustmentValue(O)":"StockAdjustmentValue(O)",
		"StockTransferIn":"StockTransferIn",
		"StockTransferInValue":"StockTransferInValue",
		"StockTransferOut":"StockTransferOut",
		"StockTransferOutValue":"StockTransferOutValue",
		"ClosingStock":"ClosingStock",
		"ClosingStockValue":"ClosingStockValue",
		"TransitStock":"TransitStock",
		"TransitStockValue":"TransitStockValue",



      },
      fileName: null

  }),
  created() {
    this.getToday();
  },
  methods:{
    getBranches(selectObj) {
      this.SetBranch = [];
      this.branch = [];
      var arr1 = [{
        shortCode: 'Select All',
        text: 'All'
      }];
      //if(this.SetEntity!='' && selectObj!='')
      this.axios
        .get(`https://mis.dragarwal.com/api-branch/${selectObj}`).then(response => {
          this.branch = arr1.concat(response.data);
          console.log(arr1);

          console.log(this.branch);

        })

    },

    getToday() {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },
apiRequestCogsdata(fromdate,SetEntity,SetBranch,Setdepartment){
  let entity = '';
  let branch = '';
    let type = '';

if((fromdate==null)||fromdate==''){
  alert("please select date")
  return false;
}
else if((SetEntity==null)||(SetEntity=='')){
  alert("please select any entity")
  return false;
}
else if((SetBranch==null)||(SetBranch=='')){
  alert("Please select any branch")
  return false;
}

else {

  
    type = 'All';
  

  console.log(fromdate+SetEntity+SetBranch+type);
  this.isLoading = true;
  this.axios
  .get(`https://mis.dragarwal.com/api-stockledger/${this.fromdate}/${this.SetEntity}/${this.SetBranch}/${type}`)
  //.get(`http://localhost:8888/api-stockledger/${this.fromdate}/${this.SetEntity}/${this.SetBranch}/${type}`)
    .then(response =>{

      console.log(response.data);
      this.processDatacogs(response.data);
    this.isLoading = false;
      if(response.data!=''){
        alert("Data loaded... please download in excel");
        branch=this.SetBranch;
        entity=this.SetEntity;

          var str="_"
          this.fileDate = this.fromdate.concat(str);
          this.file=entity.concat(str,branch,str,this.fileDate)
          console.log(this.fileDate);

          this.fileName = `Stockledger_Report_${this.file}.csv`;

      }
    })


}


},
downloadExcelCogsdata(){
  let tempDataArr = [];
  if (this.fileDate !== null) {
  tempDataArr =this.Cogslist

  return tempDataArr;
  } else {
  return null;
  }
},
processDatacogs(data){

  console.log("hit");
  console.log(data);
 this.Cogslist=data.result['stockledger'];
}

},



}
</script>
