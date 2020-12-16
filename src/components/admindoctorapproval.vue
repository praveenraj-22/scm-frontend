<template>
<v-container fluid fill-height class="grey lighten-3">
  <v-slide-y-transition mode="out-in">
    <v-layout row wrap>
      <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
        <v-toolbar flat color="grey lighten-2">

          <v-spacer></v-spacer>

          <v-select :items="currentstatus" v-model="Setstatus" label="Status Type" item-text="shortCode" item-value="text"></v-select>
          <v-spacer></v-spacer>
          <v-autocomplete clearable v-bind:items="branch" v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-autocomplete>
          <v-spacer></v-spacer>


          <v-btn rounded color="primary" dark @click="apiapproveicdoctorlist(Setstatus,SetBranch)">Generate</v-btn>

          <download-excel :data="json_data" :fields="json_fields" type="csv" :name="fileName" :fetch="downloadExcelDr">
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
            <v-toolbar-title>IC Doctor list</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-text-field v-model="search" v-if="findoctorlist" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
          </v-card-title>

          <v-data-table :headers="headers" :items="findoctorlist" v-model="selected" :search="search" class="elevation-1">
            <template slot="items" slot-scope="props">
              <tr @click="rowClick(props.item)">
                <td>{{ props.item.Active_status}}</td>
                <td class="text-xs-left">{{ props.item.Branch }}</td>
                <td class="text-xs-left">{{ props.item.Name }}</td>
                <td class="text-xs-left">{{ props.item.Infavour_of }}</td>
                <td class="text-xs-left">{{ props.item.Percentage }}</td>
                <td class="text-xs-left">{{ props.item.Pan_no }}</td>
                <td class="text-xs-left">{{ props.item.Bank_name }}</td>
                <td class="text-xs-left">{{ props.item.Account_no }}</td>
                <td class="text-xs-left">{{ props.item.Bank_ifsc }}</td>
                <td class="text-xs-left">{{ props.item.Payment_type }}</td>
                <td class="text-xs-left">{{props.item.Agreement}}</td>

                <td class="text-xs-left" v-if="!(props.item.Agreement_d==='NA')">
                  <v-btn slot="activator" small fab color="primary" @click="downloadagreement(props.item.Agreement_d)">
                    <v-icon>cloud_download</v-icon>
                  </v-btn>

                </td>
                <td class="text-xs-left" v-else="(props.item.Agreement_d==='NA')">

                </td>
                <td class="text-xs-left" v-if="!(props.item.Pan_d==='NA')">
                  <v-btn slot="activator" small fab color="primary" @click="downloadpan(props.item.Pan_d)">
                    <v-icon>cloud_download</v-icon>
                  </v-btn>

                </td>
                <td class="text-xs-left" v-else="(props.item.Pan_d==='NA')">
                </td>
                <td class="text-xs-left" v-if="!(props.item.Passbook_d==='NA')">
                  <v-btn slot="activator" small fab color="primary" @click="downloadpassbook(props.item.Passbook_d)">
                    <v-icon>cloud_download</v-icon>
                  </v-btn>

                </td>
                <td class="text-xs-left" v-else="(props.item.Passbook_d==='NA')">

                </td>
                <td class="text-xs-left" v-if="props.item.Active_status==='Pending for approval'">
                  <v-btn slot="activator" small fab color="success" @click="rowApprove(props.item)">
                    <v-icon>check</v-icon>
                  </v-btn>

                </td>
                <td class="text-xs-left" v-if="props.item.Active_status==='Pending for approval'">
                  <v-btn slot="activator" small fab color="red" @click="rowDecline(props.item)">
                    <v-icon>fas fa-times</v-icon>
                  </v-btn>

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
import {
  serverBus
} from "@/main";

export default {
  data: () => ({
    branch: [{
      shortCode: 'Select All',
      text: 'All'
    }],
    currentstatus: [{
        shortCode: "Select All",
        text: "All"
      },
      {
        shortCode: "Pending",
        text: "-1",
      },
      {
        shortCode: "Approved",
        text: "1",
      },
      {
        shortCode: "Cancelled",
        text: "-2",
      },
    ],
    headers: [{
        text: 'Status',
        align: 'left',
        sortable: false,
        value: 'Status'

      },
      {
        text: 'Branch',
        value: 'Branch'
      },
      {
        text: 'Name',
        value: 'Name'
      },
      {
        text: 'Infavour of',
        value: 'Infavour_of'
      },
      {
        text: 'Agreed %',
        value: 'Percentage'
      },
      {
        text: 'Pan no',
        value: 'Pan_no'
      },
      {
        text: 'Bank Name',
        value: 'Bank_name'
      },
      {
        text: 'Account no',
        value: 'Account_no'
      },
      {
        text: 'Bank IFSC',
        value: 'Bank_ifsc'
      },
      {
        text: 'Payment type',
        value: 'Payment_type'
      },
      {
        text: 'Agreement Available',
        value: 'Payment_type'
      },
      {
        text: 'Agreement',
        value: 'Agreement_url'
      },
      {
        text: 'Pan',
        value: 'Pan_url'
      },
      {
        text: 'Passbook',
        value: 'Passbook_url'
      },
      {
        text: 'Approve',
        value: 'Approve'
      },
      {
        text: 'Reject',
        value: 'Reject'
      }
    ],
    isLoading: false,
    fullPage: true,
    findoctorlist: null,
    fromdate: null,
    todate: null,
    search: '',
    selected: [],
    refdoctor: '',
    refinfavourdoctor: '',
    refdocbranch: '',
    refdoccontact: '',
    refdocemail: '',
    refdocpan: '',
    refdocgstin: '',
    refdocagreed: "",
    refdocacc: '',
    refdocaccifsc: '',
    refdocaccbank: '',
    agreementupload: '',
    panupload: '',
    passbookupload: '',
    json_data: null,
    json_meta: [{
      key: "charset",
      value: "utf-8"
    }],
    json_fields: {
      "Branch": "Branch",
      "Name":"Name",
      "Infavour of":"Infavour_of",
      "Agreed %":"Percentage",
      "Pan no":"Pan_no",
      "Bank Name":"Bank_name",
      "Account no":"Accnt_no",
      "Bank IFSC":"Bank_ifsc",
      "Payment type":"Payment_type",

        },
    fileName: null,
  }),
  mounted() {
    this.loadbranch();
  },

  methods: {
    loadbranch() {
      let userid = JSON.parse(sessionStorage.getItem("fin_user"));
      this.SetBranch = [];
      this.branch = [];
      var arr1 = [{
        shortCode: 'Select All',
        text: 'All'
      }];
      this.axios

        //.get(`http://localhost:8888/api-finbranch`).then(response => {
		.get(`http://localhost:8888/api-finbranch`).then(response => {
          this.branch = arr1.concat(response.data);
          console.log(this.branch);
        })
    },
    downloadagreement(Agreement_d) {

      this.axios({
        //url: `http://localhost:8888/api-download/${Agreement_d}`,
		url: `http://localhost:8888/api-download/${Agreement_d}`,
        method: 'GET',
        responseType: 'blob',
      }).then(response => {
        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement('a');

        fileLink.href = fileURL;
        fileLink.setAttribute('download', Agreement_d);
        document.body.appendChild(fileLink);

        fileLink.click();
      })

    },
    downloadpan(Pan_d) {

      this.axios({
        //url: `http://localhost:8888/api-download/${Pan_d}`,
		url: `http://localhost:8888/api-download/${Pan_d}`,
        method: 'GET',
        responseType: 'blob',
      }).then(response => {
        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement('a');

        fileLink.href = fileURL;
        fileLink.setAttribute('download', Pan_d);
        document.body.appendChild(fileLink);

        fileLink.click();
      })

    },
    downloadpassbook(Passbook_d) {

      this.axios({
        //url: `http://localhost:8888/api-download/${Passbook_d}`,
		url: `http://localhost:8888/api-download/${Passbook_d}`,

        method: 'GET',
        responseType: 'blob',
      }).then(response => {
        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement('a');

        fileLink.href = fileURL;
        fileLink.setAttribute('download', Passbook_d);
        document.body.appendChild(fileLink);

        fileLink.click();
      })

    },
    apiapproveicdoctorlist(Setstatus, SetBranch) {

      if ((this.Setstatus == null) || (this.Setstatus == '')) {
        alert("Please select the status type")
        return false;
      }
      if ((this.SetBranch == null) || (this.SetBranch == '')) {
        alert("Please select the Branch")
        return false;
      }
      this.isLoading = true;
      //this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
	  this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
        .then(response => {
          console.log(response.data);
          this.processdatalist(response.data)
          this.isLoading = false;
        })
        this.fileName=`Doctor_Report.csv`;
    },
    processdatalist(data) {
      this.findoctorlist = data.result["findata"];
      console.log(this.findoctorlist);
      this.show = true;
    },
    rowApprove(row) {
      let fin_id = '';

      this.isLoading = true;
      //this.$http.post(`http://localhost:8888/api-doctorapprove/`, {
	  this.$http.post(`http://localhost:8888/api-doctorapprove/`, {
        fin_id: row.ID,
      }).then(response => {
        this.isLoading = false;
        alert("Approved");
        console.log("this.Setstatus : " + this.Setstatus);
        console.log("this.SetBranch :" + this.SetBranch);
        //this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
		this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
          .then(response => {
            console.log(response.data);
            this.processdatalist(response.data)
            this.isLoading = false;
          })


      })

    },
    rowDecline(row) {
      let fin_id = '';
      this.isLoading = true;
      //this.$http.post(`http://localhost:8888/api-doctorreject/`, {
	  this.$http.post(` http://localhost:8888/api-doctorreject/`, {
        fin_id: row.ID,

      }).then(response => {
        this.isLoading = false;
        alert("cancelled");
        console.log("this.Setstatus : " + this.Setstatus);
        console.log("this.SetBranch :" + this.SetBranch);
        //this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
		this.$http.get(`http://localhost:8888/api-findoctorlist/${this.Setstatus}/${this.SetBranch}`)
          .then(response => {
            console.log(response.data);
            this.processdatalist(response.data)
            this.isLoading = false;
          })
      })
    }
    ,downloadExcelDr() {
      let tempDataArr = [];
      if (this.fileDate !== null) {
        tempDataArr = this.findoctorlist

        return tempDataArr;
      } else {
        return null;
      }
    },
  }

}
</script>
