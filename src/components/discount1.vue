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
            <v-toolbar-title>Discount </v-toolbar-title>
            <v-divider
              class="mx-2 black"
              inset
              vertical
            ></v-divider>
            <v-spacer></v-spacer>
            <v-menu
              absolute
              ref="menu1"
              :close-on-content-click="false"
              v-model="menu1"
              :nudge-right="40"
              :return-value.sync="fdate"
              lazy
              transition="scale-transition"
              offset-y
              full-width
              min-width="200px"
            >
              <v-text-field
                slot="activator"
                v-model="fdate"
                placeholder="Select From Date"
                prepend-inner-icon="event"
                readonly
              ></v-text-field>
              <v-date-picker
                color="primary"
                v-model="fdate"
                no-title
                scrollable
                min="2018-04-01"
                backgroundRevenue-color="red"
                style="box-shadow:none"
              >
                <v-spacer></v-spacer>
                <v-btn
                  flat
                  color="primary"
                  @click="menu1 = false"
                  style="outline:none"
                >Cancel</v-btn>
                <v-btn
                  flat
                  color="primary"
                  @click="$refs.menu1.save(fdate)"
                  style="outline:none"
                >Ok</v-btn>
              </v-date-picker>
            </v-menu>

            <v-spacer></v-spacer>
            <v-menu
              absolute
              ref="menu2"
              :close-on-content-click="false"
              v-model="menu2"
              :nudge-right="40"
              :return-value.sync="tdate"
              lazy
              transition="scale-transition"
              offset-y
              full-width
              min-width="200px"
            >
              <v-text-field
                slot="activator"
                v-model="tdate"
                placeholder="Select To Date"
                prepend-inner-icon="event"
                readonly
              ></v-text-field>
              <v-date-picker
                color="primary"
                v-model="tdate"
                no-title
                scrollable
                min="2018-04-01"
                max=
                backgroundRevenue-color="red"
                style="box-shadow:none"
              >
              <v-spacer></v-spacer>
              <v-btn
                flat
                color="primary"
                @click="menu2 = false"
                style="outline:none"
              >Cancel</v-btn>
              <v-btn
                flat
                color="primary"
                @click="$refs.menu2.save(tdate)"
                style="outline:none"
              >Ok</v-btn>
              </v-date-picker>
            </v-menu>

             <v-btn rounded color="primary" dark @click="apiRequestDiscount(fdate,tdate)">Generate</v-btn>



            <download-excel
              :data="json_data"
              :fields="json_fields"
              type="csv"
              :name="fileName"
              :fetch="downloadExcelRevenueSuper"
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
            </download-excel>
          </v-toolbar>
          <loading
            :active.sync="isLoading"
            :is-full-page="fullPage"
            color="#7f0000"
            loader="bars"
          ></loading>
          <!-- Vuetify Data table -->
          <div class="table-responsive">
            <table
              class="table table-hover table-bordered"
              v-if="show"
            >
              <thead>

              </thead>
              <tbody>


              </tbody>

            </table>
          </div>

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
export default {
  data: () => ({


    // ftdotcount: null,
    // mtdotcount: null,
    // cogsftdotcount: null,
    // cogsmtdotcount: null,
    userName: null,
    isLoading: false,
    fullPage: true,

    title: null,
	user_role : null,

    rowColor: null,
    isActive: false,
    save: "save",
    show: false,
    fileDate: null,
    loading: false,
    fdate: null,
    tdate: null,
    menu: false,
    menu1: false,
    modal: false,
    menu2: false,
    today: "",
    alin: null,
	 Ap: null,
   Chennai:null,
   ROTN:null,
   Karnataka:null,
   Hyderabad:null,
   Kolkata:null,
   Odisha:null,
   ROI:null,
Chennaibranches:null,
ROTNbranches:null,
Karnatakabranches:null,
Hyderabadbranches:null,
APbranches:null,
Kolkatabranches:null,
Odishabranches:null,
ROIbranches:null,
    json_data: null,
    json_meta: [
      {
        key: "charset",
        value: "utf-8"
      }
    ],
    json_fields: {
"Region":"groupwise",
      "Code": "branchcode",
      "Location":"branch",
      "MTD": "mtdoptrev",
      "LMTD": "lstoptrev",
	  "MTD%": "mtdoptperc",
	  "Target Month": "targetmtdrev",
"Target Achieved":"mtdoptpercachieved"
    },
    fileName: null
  }),
  created () {
    this.getToday();
  },
  methods: {
    getToday () {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },

    numberformat(number){
      return number.toLocaleString('en');
    },

    apiRequestDiscount(fdate,tdate) {
      // let superUserName = sessionStorage.getItem("super_user");
      console.log("hit");
      alert("fdate : "+fdate);
      alert("tdate : "+tdate);
      if (date !== null) {
        console.log(fdate);
                console.log(tdate);
        this.fileDate = date;
        this.fileName = `NewOPD_Report_${this.fileDate}.csv`;
        this.loading = true;
        this.isLoading = true;
        this.$http
      //    .get(`https://scm.dragarwal.com/api-opticals-super/${date}`)
          .get(`http://localhost:7777/api-discount-super/${fdate}/${tdate}`)
                    .then(response => {
            this.processDataOPDSuper(response.data);
            this.isLoading = false;
          });
      } else {
        return null;
      }
    },
    processDataOPDSuper (data) {
	  if (sessionStorage.getItem('super_user')){
	     this.user_role = 'super_user';
	  }else if(sessionStorage.getItem('overseas_user')){
	    this.user_role = 'overseas_user';
	  }else if(sessionStorage.getItem('indian_user')){
		this.user_role = 'indian_user';
  }else if(sessionStorage.getItem('optical_user')){
		this.user_role = 'optical_user';
	  }
this.Chennai = [data.group['Chennai']];
	  this.AP = [data.group['AP']];
this.ROTN=[data.group['ROTN']];
this.Karnataka=[data.group['Karnataka']];
this.Maharashtra=[data.group['Maharashtra']];
this.Hyderabad=[data.group['Hyderabad']];
this.Kolkata=[data.group['Kolkata']];
this.Odisha=[data.group['Odisha']];
this.ROI=[data.group['ROI']];
   this.alin = [data.alin];
   this.Chennaibranches=data.branch['Chennai'];
   this.ROTNbranches=data.branch['ROTN'];
   this.Karnatakabranches=data.branch['Karnataka'];
    this.Maharashtrabranches=data.branch['Maharashtra'];
   this.Hyderabadbranches=data.branch['Hyderabad'];
   this.APbranches=data.branch['AP'];
   this.Kolkatabranches=data.branch['Kolkata'];
   this.Odishabranches=data.branch['Odisha'];
   this.ROIbranches=data.branch['ROI'];

    //   this.aeh = [data.aeh];
    //   this.ahc = [data.ahc];
	  // this.ohc = [data.ohc];
    //   // this.cmh = [data.aehgroup["Chennai Main Hospital"]];
    //   this.cmh = data.branchwise["Chennai Main Hospital"];
    //   this.aeh_chennai = [data.aehgroup["Chennai Branches"]];
    //   this.aeh_chennai_branches = data.branchwise["Chennai Branches"];
    //   this.kanchi_vel = [data.aehgroup["Kanchi + Vellore"]];
    //   this.kanchi_vel_branches = data.branchwise["Kanchi + Vellore"];
    //   this.kum_ney_vil = [data.aehgroup["Kum + Ney + Vil"]];
    //   this.kum_ney_vil_branches = data.branchwise["Kum + Ney + Vil"];
    //   this.dha_salem_krish = [data.aehgroup["Dha + Salem + Krish"]];
    //   this.dha_salem_krish_branches = data.branchwise["Dha + Salem + Krish"];
    //   this.erod_hosure = [data.aehgroup["Erode + Hosur"]];
    //   this.erod_hosure_branches = data.branchwise["Erode + Hosur"];
    //   // this.jaipur = [data.aehgroup["Jaipur"]];
    //   this.jaipur = data.branchwise["Jaipur"];
    //   this.madurai = [data.aehgroup["Madurai KK Nagar"]];
    //   this.ahc_chennai = [data.ahcgroup["Chennai branches"]];
    //   this.ahc_chennai_branches = data.branchwise["Chennai branches"];
    //   // this.tirunelveli = [data.ahcgroup["Tirunelveli"]];
    //   this.tirunelveli = data.branchwise["Tirunelveli"];
	  // this.coimbatore = data.branchwise["Coimbatore"];
    //
    //   this.tuti_madurai = [data.ahcgroup["Tuticorin + Madurai"]];
    //   this.tuti_madurai_branches = data.branchwise[
    //     "Tuticorin + Madurai"
    //   ].concat(data.branchwise["Madurai KK Nagar"]);
    //   // this.trichy = [data.ahcgroup["Trichy"]];
    //   this.trichy = data.branchwise["Trichy"];
    //   // this.thanjavur = [data.ahcgroup["Thanjavur"]];
    //   this.thanjavur = data.branchwise["Thanjavur"];
    //   this.andaman = [data.ahcgroup["Andaman"]];
    //   this.karnataka = [data.ahcgroup["Karnataka"]];
    //   this.banglore = [data.ahcgroup["Banglore"]];
    //   this.banglore_branches = data.branchwise["Banglore"];
    //   this.hub_mys = [data.ahcgroup["Hubli + Mysore"]];
    //   this.hub_mys_branches = data.branchwise["Hubli + Mysore"];
    //   this.telangana = [data.ahcgroup["Telangana"]];
    //   this.hyderabad = [data.ahcgroup["Hyderabad"]];
    //   this.hyderabad_branches = data.branchwise["Hyderabad"];
    //   this.andhra = [data.ahcgroup["Andhra Pradesh"]];
    //   this.andhra_branches = data.branchwise["Andhra Pradesh"];
    //   this.roi = [data.ahcgroup["Rest of India(incl. Jaipur)"]];
    //   // this.triv = [data.ahcgroup["Trivandrum"]];
    //   this.triv = data.branchwise["Trivandrum"];
    //   this.kolk = [data.ahcgroup["Kolkata"]];
    //   this.kolk_branches = data.branchwise["Kolkata"];
    //   // this.pune = [data.ahcgroup["Pune"]];
    //   this.pune = data.branchwise["Pune"];
    //   this.ahmedabad = data.branchwise["Ahmedabad"];
    //   this.indore = data.branchwise["Indore"];
    //   this.odisha = [data.ahcgroup["Odisha"]];
    //   this.odisha_branches = data.branchwise["Odisha"];
    //
    //
	  // this.madagascar = data.branchwise["Madagascar"];
	  // this.mozambique = [data.ohcgroup["Mozambique"]];
    //   this.mozambique_branches = data.branchwise["Mozambique"];
	  // this.nigeria = data.branchwise["Nigeria"];
	  // this.rwanda = data.branchwise["Rwanda"];
	  // this.mauritius = [data.ohcgroup["Mauritius"]];
    //   this.mauritius_branches = data.branchwise["Mauritius"];
	  // this.zambia = data.branchwise["Zambia"];
	  // this.ghana = data.branchwise["Ghana"];
	  // this.nairobi = data.branchwise["Nairobi"];
	  // this.uganda = data.branchwise["Uganda"];
	  // this.tanzania = data.branchwise["Tanzania"];





      // this.amb = [data.ahcgroup["Ambattur"]];
      this.show = true;
    },
    downloadExcelRevenueSuper () {
let tempDataArr = [];

if (this.fileDate !== null) {
tempDataArr = [this.alin].concat(
  this.alin,
    this.Chennai ,
      this.AP ,
  this.ROTN,
  this.Karnataka,
  this.Hyderabad,
  this.Kolkata,
  this.Odisha,
  this.ROI,
    // this.alin ,
     this.Chennaibranches,
     this.ROTNbranches,
     this.Karnatakabranches,
     this.Hyderabadbranches,
     this.APbranches,
     this.Kolkatabranches,
     this.Odishabranches,
     this.ROIbranches
);
return tempDataArr;
} else {
return null;
}




    },

    changeColorOPDSuper (data) {
      if (data.code === undefined) {
        this.rowColor = "text-xs-center";
        return true;
      } else {
        if (
          [
            "CMH",
            "JPR",
            "TVL",
            "TRI",
            "TNJ",
            "TVM",
            "PUN",
            "AHM",
            "IND"
          ].includes(data.code)
        ) {
          this.rowColor = "text-xs-center";
          return true;
        } else {
          this.rowColor = "text-xs-center grey lighten-1";
          return false;
        }
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
  touch-action: none;
}
table#stickyHeader thead {
  border-top: none;
  border-bottom: none;
  background-color: #000;
  touch-action: none;
}
.table-striped > tbody > tr:nth-child(2n + 2) > td,
.table-striped > tbody > tr:nth-child(2n + 2) > th {
  background-color: #e5e5f2;
  touch-action: none;
}
.allindiagroup {
background-color : #f0ae19!important
}
.targetcolor {
  background-color : #FCD12A!important
}
.targetachicolor {
  background-color : #87CEFA!important
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
