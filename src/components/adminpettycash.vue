<template>
<v-container fluid fill-height class="grey lighten-3">
  <v-slide-y-transition mode="out-in">
    <v-layout row wrap>
      <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
        <v-toolbar flat color="grey lighten-2">
  <v-spacer></v-spacer>
          <th width="20%">
            <v-autocomplete :items="branch" v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-autocomplete>
          </th>

          <v-spacer></v-spacer>
          <v-select :items="bill_status" v-model="SetStatus" label="Bill Status" id="SelEntity" item-text="shortCode" item-value="text"></v-select>

          <!-- <v-spacer></v-spacer>
          <v-menu absolute ref="menu1" :close-on-content-click="false" v-model="menu1" :nudge-right="40" :return-value.sync="fromdate" lazy transition="scale-transition" offset-y full-width min-width="150px">
            <v-text-field slot="activator" v-model="fromdate" placeholder="Select From Date" prepend-inner-icon="event" readonly></v-text-field>
            <v-date-picker color="primary" v-model="fromdate" no-title scrollable :min="minDate" :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="menu1 = false" style="outline:none">Cancel</v-btn>
              <v-btn flat color="primary" @click="$refs.menu1.save(fromdate)" style="outline:none">Ok</v-btn>
            </v-date-picker>
          </v-menu>


          <v-menu absolute ref="menu2" :close-on-content-click="false" v-model="menu2" :nudge-right="40" :return-value.sync="todate" lazy transition="scale-transition" offset-y full-width min-width="150px">
            <v-text-field slot="activator" v-model="todate" placeholder="Select To Date" prepend-inner-icon="event" readonly></v-text-field>
            <v-date-picker color="primary" v-model="todate" no-title scrollable :min="minDate" :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="menu2 = false" style="outline:none">Cancel</v-btn>
              <v-btn flat color="primary" @click="$refs.menu2.save(todate)" style="outline:none">Ok</v-btn>
            </v-date-picker>
          </v-menu> -->

          <v-btn rounded color="primary" dark @click="apiRequestfinpc(SetStatus,SetBranch)">Generate</v-btn>


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

          <v-data-table :headers="headers" :items="strch" v-model="selected" :search="search" class="elevation-4">
            <template slot="items" slot-scope="props">
              <tr>
                <td @click="rowClick(props.item.STATUS)">{{ props.item.STATUS }}</td>
                <td class="text-xs-left">{{ props.item.branch }}</td>
                <td class="text-xs-right">{{ props.item.credit }}</td>
                <td class="text-xs-right">{{ props.item.approved }}</td>

                <td class="text-xs-right" style="color:red">{{ props.item.cancelled }}</td>
                <td class="text-xs-right" style="color:blue" >{{ props.item.pending }}</td>
                  <td class="text-xs-right" style="color:green" >{{ props.item.balance }}</td>
                  <td class="text-xs-left">{{ props.item.Submitted_date }}</td>
                <td class="text-xs-left">
                  <v-btn slot="activator" small color="primary" @click="rowClick(props.item)">
                    View
                  </v-btn>
                </td>
                <td class="text-xs-left" v-if="props.item.STATUS==='Pending'">
                  <!-- <v-btn slot="activator" small fab color="success" @click="rowApproveAll(props.item)">
                    <v-icon>check</v-icon>
                  </v-btn> -->
                  <v-btn slot="activator" small fab @click.stop="$set(dialogapprove, props.item.test, true)"  color="green">
                    <v-icon>check</v-icon>
                  </v-btn>


                  <v-dialog v-model="dialogapprove[props.item.test]" persistent max-width="800px" lazy absolute :key="props.item.test">
                    <v-card>

                      <v-card-title class="headline">Branch : {{props.item.branch }}</v-card-title>

                      <!-- <v-card-subtitle>Total amount : {{props.item.credit}}</v-card-subtitle> -->

                      <v-card-title>

                        <span>Total amount: {{props.item.credit}}{{"------"}} Used amount : {{props.item.pending}} {{"------"}} Balance : {{props.item.balance}}</span>

                      </v-card-title>

                      <v-card-title>

                        <span>Approving amount: {{props.item.pending}}</span>

                        <!-- <span>Cancelled amount :{{props.item.Cancelled_amount}}</span> -->

                      </v-card-title>

                      <v-card-title>
                        <!--
                        <span>Approving amount: {{props.item.totalamount}}</span> -->

                        <span style="color:red">Cancelled amount :{{props.item.cancelled}}</span>

                      </v-card-title>

                      <v-card-text>
                        <v-container grid-list-md>
                          <v-layout wrap>

                            <v-flex xs12 sm6>
                              <!-- <v-textarea clearable clear-icon="cancel" label="Comments" v-model='schcomments'></v-textarea> -->
                              <v-text-field type="number" clearable v-model="refillamount" label="Amount" required></v-text-field>
                            </v-flex>
                          </v-layout>
                        </v-container>
                      </v-card-text>


                      <v-card-actions>

                        <v-btn color="primary" flat @click.stop="$set(dialogapprove, props.item.test, false)">Close</v-btn>
                        <v-btn color="blue darken-1" flat @click="rowApproveAll(props.item,refillamount)" @click.stop="$set(dialogapprove, props.item.test, false)">Refill&Approve</v-btn>

                      </v-card-actions>
                    </v-card>
                  </v-dialog>


                </td>

                <v-dialog v-model="dialog" persistent max-width="2000px" lazy absolute>
                  <v-card>
                    <v-card-title>
                      <span class="headline">Information</span>
                    </v-card-title>

                    <v-card-text>

                      <table align="center" v-if="showgroup" class="inlineTable table-hover table-bordered" border="2">
                        <tr>
                          <th class="text-left">Category</th>
                          <th class="text-left">Total amount</th>
                          <th class="text-left">Decline</th>
                        </tr>
                        <tr v-for="(grpdata) in groupdata" :key="grpdata.category_name" @click="">
                          <td @click="groupclick(grpdata)" class="text-xs-left" style="color:blue">
                            <u>{{grpdata.category_name}}</u>
                          </td>
                          <td class="text-xs-right">{{grpdata.totalamount}}</td>
                          <td class="text-xs-right" v-if="grpdata.status==='Pending'">
                            <!-- <v-btn slot="activator" small fab color="success" @click="rowApprove(grpdata)">
                              <v-icon>check</v-icon>
                            </v-btn> -->
                            <!-- <v-btn slot="activator" small color="primary" @click="groupclick(grpdata)">
                              View
                            </v-btn> -->
                            {{grpdata.status}}
                          </td>
                          <td class="text-xs-right" v-else>
                            {{grpdata.status}}
                          </td>

                        </tr>
                      </table>
                      <table align="center" v-if="showgroupdetail" class="inlineTable table-hover table-bordered" border="1">
                        <tr>

                          <th class="text-left">Bill date</th>

                          <th class="text-left">Category name</th>
                          <th class="text-left">Bill No</th>
                          <th class="text-left">Vendor Name </th>
                          <th class="text-left">Voucher No </th>
                          <th class="text-left">Total Amount</th>
                          <th class="text-left">Comments</th>
                          <th class="text-left">Decline</th>
                          <th class="text-left">Bill Download</th>
                          <th class="text-left">Voucher Download</th>
                        </tr>
                        <tr v-for="item in groupdatadetail" :key="item.name">
                          <td class="text-left">{{ item.Bill_date }}</td>
                          <td class="text-left">{{ item.category_name }}</td>
                          <td class="text-left">{{ item.bill_no }}</td>
                          <td class="text-left">{{ item.vendorname }}</td>
                          <td class="text-left">{{ item.voucher_no }}</td>
                          <td class="text-right">{{ item.totalamount }}</td>
                          <td class="text-left"> {{item.Comments}}</td>
                          <td class="text-xs-right" v-if="item.Active_status==='Pending'">
                            <v-btn slot="activator" small fab @click.stop="$set(dialogcancel, item.bill_no, true)" color="red">
                              <v-icon>fas fa-times</v-icon>
                            </v-btn>


                            <v-dialog v-model="dialogcancel[item.bill_no]" persistent max-width="800px" lazy absolute :key="props.item.bill_no">
                              <v-card>
                                <v-card-title>
                                  <span>{{'branch : '}}{{ item.branch }}{{" -- Bill no : "}}{{item.bill_no}} {{'-- Vendorname : '}}{{item.vendorname}}{{' -- Amount : '}}{{item.Debit}}</span><br />
                                  <span>Cancel Remark</span>
                                </v-card-title>

                                <v-card-text>
                                  <v-container grid-list-md>
                                    <v-layout wrap>

                                      <v-flex xs12 sm6>
                                        <v-textarea clearable clear-icon="cancel" label="Comments" v-model='schcomments'></v-textarea>
                                      </v-flex>
                                    </v-layout>
                                  </v-container>
                                </v-card-text>


                                <v-card-actions>

                                  <v-btn color="primary" flat @click.stop="$set(dialogcancel, item.bill_no, false)">Close</v-btn>
                                  <v-btn color="blue darken-1" flat @click="rowDecline(item,schcomments)" @click.stop="$set(dialogcancel, item.bill_no, false)">Decline</v-btn>

                                </v-card-actions>
                              </v-card>
                            </v-dialog>


                          </td>
                          <td class="text-xs-right" v-else="!(item.Active_status==='Pending')">
                            {{item.Active_status}}
                          </td>
                          <td class="text-xs-right" v-if="!(item.voucher_attach==='NA')">
                            <v-btn slot="activator" small fab color="primary" @click="downloadvouchher(item.voucher_attach)">
                              <v-icon>cloud_download</v-icon>
                            </v-btn>

                          </td>
                          <td class="text-xs-right" v-else="(item.voucher_attach==='NA')">
                            {{ NA }}
                          </td>
                          <td class="text-xs-right" v-if="!(item.bill_attach==='NA')">
                            <v-btn slot="activator" small fab color="primary" @click="downloadbill(item.bill_attach)">
                              <v-icon>cloud_download</v-icon>
                            </v-btn>

                          </td>
                          <td class="text-xs-right" v-else="(item.bill_attach==='NA')">
                            {{NA }}
                          </td>

                        </tr>
                      </table>

                    </v-card-text>


                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="blue darken-1" flat @click="dialogclose()">Close</v-btn>
                    </v-card-actions>



                  </v-card>
                </v-dialog>

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
    dialogcancel: {},
    dialogapprove: {},
    refillamount: '',
    schcomments: '',
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
        text: '2'
      },
      {
        shortCode: 'Approved',
        text: '4'
      },
      {
        shortCode: 'Need SCH Approval',
        text: '1'
      },

      {
        shortCode: 'Cancelled By SCH',
        text: '3'
      },
      {
        shortCode: 'Cancelled',
        text: '5'
      },
    ],

    fromdate: null,
    todate: null,
    minDate: "2020-08-01",
    maxDate: curday('-'),
    menu1: false,
    menu2: false,
    isLoading: false,
    fullPage: true,
    headers: [
      {
        text: 'Status',
        align: 'left',
        sortable: false,
        value: 'Status'
      },
      {
        text: 'Branch',
        value: 'branch',
        sortable: false
      },
      {
        text: 'Allocated',
        value: 'Allocated',
        sortable: false
      },

      {
        text: 'Approved',
        value: 'Approved',
        sortable: false
      },
      {
        text: 'Cancelled',
        value: 'Cancelled',
        sortable: false
      },
      {
        text: 'Pending',
        value: 'Pending'


      },

      {
        text: 'Balance',
        value: 'Balance',
        sortable: false
      },
      {
        text: 'Submitted',
        value: 'Submitted',
        sortable: false
      },

      {
        text: 'View',
        value: 'View',
        sortable: false
      },
      {
        text: 'Approve',
        value: 'Approve',
        sortable: false

      },

      // {
      //   text: 'Decline',
      //   value: 'Decline'
      // }
    ],
    selected: '',
    search: '',
    strch: null,
    dialog: false,
    show: false,
    showgroup: false,

    showgroupdetail: false,

    selecteddata: [],


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
        //  .get(`https://scm.dragarwal.com/api-branch/${selectObj}`).then(response =>{
        .get(`http://localhost:8888/api-finbranch`).then(response => {
          this.branch = arr1.concat(response.data);
          console.log(this.branch);
        })


    },

    apiRequestfinpc( SetStatus, SetBranch) {
      // if ((this.fromdate == '') || (this.fromdate == '')) {
      //   alert("Please select Month");
      //   return false;
      // }
     if ((this.SetBranch == '') || (this.SetBranch == null)) {
        alert("Please select branch")
        return false;
      }
      else  if ((this.SetStatus === null) || (this.SetStatus == '')) {
       alert("Please select status");
       return false;
     }


      this.isLoading = true;
      //    let userid = JSON.parse(sessionStorage.getItem("fin_user"));
      let normalusername = JSON.parse(sessionStorage.getItem("fin_user"));
      console.log(normalusername);
      this.$http.get(`http://localhost:8888/api-finpc/${this.SetBranch}/${this.SetStatus}/${normalusername.name}`)
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
    },
    rowClick(id) {
      console.log(id);
      //return false;
      this.dialog = true;
      this.showgroup = true;
      this.isLoading = true;
      this.showgroupdetail = false;
      this.$http.get(`http://localhost:8888/api-finpcbranchgroupbill/${id.branch}/${id.statusno}/${id.bill_submission}`)
        .then(response => {
          this.isLoading = false;
          this.groupdata = response.data;
          console.log(response.data);
        })

    },

    dialogclose() {
      this.dialog = false;
      this.allSelected = false;
      this.selecteddata = false;
      this.isLoading = true;
      let normalusername = JSON.parse(sessionStorage.getItem("fin_user"));
      console.log(normalusername);
    this.$http.get(`http://localhost:8888/api-finpc/${this.SetBranch}/${this.SetStatus}/${normalusername.name}`)
        .then(response => {
          this.isLoading = false;
          console.log(response.data);
          this.processliststrchdata(response.data);

        })


    },
    groupclick(item) {

      console.log(item);
    //  return false;
      this.isLoading = true;
      this.$http.get(`http://localhost:8888/api-finpcbranchgroupbilldetail/${item.branch}/${item.category_id}/${item.bill_submission}`).
      then(response => {
        console.log(response);
        this.isLoading = false;
        this.showgroupdetail = true;
        this.groupdatadetail = response.data;
      })


    },
    rowApproveAll(item, refillamount) {



      if (item.pending < refillamount) {

        if (confirm("Entering amount is greater than refilled amount")) {

        } else {
          alert('Cancel')
          return false
        }
        //      alert("Entering amount is greater than refilled amount")

      }
        //  return false;
      console.log(item);
      console.log(refillamount);
      let normalusername = JSON.parse(sessionStorage.getItem("fin_user"));
      console.log(normalusername);
      // this.isLoading = true;
      this.$http.post(`http://localhost:8888/api-finptycshbillgroupapproveall`, {
        strch_id: normalusername.name,
        strch_branch: item.branch,
      strch_date:item.bill_submission,
        status: item.statusno,
        finrefilledamount: refillamount
      }).then(response => {
        this.isLoading = false;
        if (response.data.dataupdated == true) {
          alert("approved");
          this.isLoading = true;
          let normalusername = JSON.parse(sessionStorage.getItem("fin_user"));
          console.log(normalusername);
          this.$http.get(`http://localhost:8888/api-finpc/${this.SetBranch}/${this.SetStatus}/${normalusername.name}`)
            .then(response => {
              this.refillamount='';
              this.isLoading = false;
              console.log(response.data);
              this.processliststrchdata(response.data);

            })
        } else {
          alert("Error in approving date")
        }
      })

    },
    // rowApprove(item) {
    //   console.log(item);
    //   let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));
    //   console.log(normalusername);
    //   console.log(this.fromdate + " " + this.todate);
    //   this.isLoading = true;
    //   this.$http.post(`http://localhost:8888/api-strchbillgroupapprove`, {
    //     strch_id: normalusername.name,
    //     strch_groupcategory: item.category_name,
    //     strch_branch: item.branch,
    //     strch_fdate: this.fromdate,
    //     strch_tdate: this.todate,
    //   }).then(response => {
    //     this.isLoading = false;
    //     if (response.data.dataupdated == true) {
    //       alert("approved")
    //
    //       this.dialog = true;
    //       this.showgroup = true;
    //       this.isLoading = true;
    //       this.showgroupdetail = false;
    //       //http://localhost:8888/api-strchbranchgroupbill/${id.branch}/${this.fromdate}/${this.todate}/${id.status1}
    //       this.$http.get(`http://localhost:8888/api-strchbranchgroupbill/${item.branch}/${this.fromdate}/${this.todate}/${item.status1}`)
    //         .then(response => {
    //           this.groupdata = response.data;
    //           console.log(response.data);
    //           this.isLoading = false;
    //         })
    //
    //
    //     } else {
    //       alert("Error in updating data ");
    //       this.dialog = true;
    //       this.showgroup = true;
    //       this.isLoading = true;
    //       this.showgroupdetail = false;
    //       this.$http.get(`http://localhost:8888/api-strchbranchgroupbill/${item.branch}/${this.fromdate}/${this.todate}/${item.status1}`)
    //         .then(response => {
    //           this.isLoading = false;
    //           this.groupdata = response.data;
    //           console.log(response.data);
    //         });
    //
    //     }
    //
    //   })
    // },

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

    rowDecline(item, schcomments) {
      console.log(item);
      console.log("jhit");
      //  this.dialogcancel=false;
      let normalusername = JSON.parse(sessionStorage.getItem("fin_user"));

      this.isLoading = true;

      this.$http.post(`http://localhost:8888/api-finpcbillgroupdecline`, {
        strch_id: normalusername.name,
        strch_groupcategory: item.category_name,
        strch_branch: item.branch,
        strch_date: item.created_date,
        amount: item.totalamount,
        comments: schcomments,
        strch_billno: item.bill_no,
        strch_vouchername: item.vendorname,
        strch_voucherno: item.voucher_no,
        bill_attach: item.bill_attach,
        voucher_attach: item.voucher_attach


      }).then(response => {
        this.isLoading = false;
        if (response.data.dataupdated == true) {
          alert('Cancelled');
          this.schcomments = '';
          console.log(item);
          this.isLoading = true;
          this.$http.get(`http://localhost:8888/api-finpcbranchgroupbilldetail/${item.branch}/${item.category_id}/${item.bill_submission}`).
          then(response => {
            console.log(response);

            this.showgroupdetail = true;
            this.groupdatadetail = response.data;
          });
            this.$http.get(`http://localhost:8888/api-finpcbranchgroupbill/${item.branch}/${item.status}/${item.bill_submission}`)
            .then(response => {
              this.isLoading = false;
              this.groupdata = response.data;
              console.log(response.data);
              this.showgroup = true;
            });


        } else {
          alert('error in cancelling data');
        }
      })

    },
    declineamount(item) {
      console.log(item);
      return false;
      this.isLoading = true;
      this.$http.get(`http://localhost:8888/api-declineamount/${item.branch}/${this.fromdate}/${this.todate}`)
        .then(response => {
          console.log(response);
          this.isLoading = false;
          this.declineamount = response.data;

        })
    }

  }
}
</script>

<style >
.inlineTable {
  display: inline-block;
}
</style>
