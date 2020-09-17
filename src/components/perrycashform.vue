<template>
  <v-container

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
            <v-toolbar-title>Petty Cash</v-toolbar-title>



          </v-toolbar>

          <loading
            :active.sync="isLoading"
            :is-full-page="fullPage"
            color="#7f0000"
            loader="bars"
          ></loading>



		     <v-card-text>
			         <table  class="table table-hover table-bordered"
              v-if="show">
              <thead>
                <tr class="grey lighten-2">
                  <th
                    class="text-xs-left"
                    scope="col"
                  >Branch</th>
                  <th
                    class="text-xs-center"
                    scope="col"
                  >Credit</th>
                  <th
                    class="text-xs-center"
                    scope="col"
                  >Debit</th>
				  <th
                    class="text-xs-center"
                    scope="col"
                  >Balance</th>
                </tr>
              </thead>
			  <tbody>
                <tr
                  scope="row"
                  :class="'font-weight-black indigo lighten-2'"
                >
                  <td
                    scope="row"
                    :class="'text-xs-left'"
                  >{{pettycash_branch}}</td>
                  <td
                    scope="row"
                    class="text-xs-center"
                  >{{pettycash_credit}}</td>
                  <td
                    scope="row"
                    class="text-xs-center"
                  >{{pettycash_debit}}</td>
                  <td
                    scope="row"
                    class="text-xs-center"
                  >{{pettycash_balance}}</td>

                </tr>
              </tbody>
			  </table>

                    <v-container grid-list-md>
                      <v-layout wrap>
					        <v-flex xs12 sm6 md4>
								<v-select
								v-model="selbranch"
								:items="branch"
								label="Branch"
								 item-text="shortCode"
								item-value="TEXT"
								v-on:change="branchamount"
								></v-select>
							</v-flex>

							<v-flex xs12 sm6 md4>
								 <v-text-field
								  v-model="voucherno"
								  label="Voucher No"
								></v-text-field>
							</v-flex>

							<v-flex xs12 sm6 md4>
								<v-select
								v-model="category"
								:items="items"
								label="Category"
								item-text="shortCode"
								item-value="text"

								></v-select>
							</v-flex>

							<v-flex xs12 sm6 md4>
								 <v-text-field
								  v-model="vendorname"
								  label="Vendor Name"
								></v-text-field>
							</v-flex>

							<v-flex xs12 sm6 md4>
								 <v-text-field
								  v-model="billno"
								  label="Bill No"
								 ></v-text-field>
							</v-flex>
							<v-flex xs12 sm6 md4>


								 <!-- <v-text-field
								  v-model="expensedate"
								  label="Bill/ Expense Date"
								  type="date"
								  :max="today"


								></v-text-field> -->
                <v-menu absolute ref="menu1" :close-on-content-click="false" v-model="menu1" :nudge-right="40" :return-value.sync="expensedate" lazy transition="scale-transition" offset-y full-width min-width="150px">
                  <v-text-field slot="activator" v-model="expensedate" placeholder="Select From Date" prepend-inner-icon="event" readonly></v-text-field>
                  <v-date-picker color="primary" v-model="expensedate" no-title scrollable  :max="maxDate" backgroundRevenue-color="red" style="box-shadow:none">
                    <v-spacer></v-spacer>
                    <v-btn flat color="primary" @click="menu1 = false" style="outline:none">Cancel</v-btn>
                    <v-btn flat color="primary" @click="$refs.menu1.save(expensedate)" style="outline:none">Ok</v-btn>
                  </v-date-picker>
                </v-menu>
							</v-flex>

							<v-flex xs12 sm6 md4>

								 <v-text-field
								  v-model="remarks"
								  label="Remarks"
								></v-text-field>
							</v-flex>

							<v-flex xs12 sm6 md4>
								 <v-text-field
								  v-model="amount"
								  label="Amount"
								  type="number"

								></v-text-field>
							</v-flex>

							<v-flex xs12 sm6 md4>
								 <v-text-field
								  v-model="sumbissiondate"
								  label="Submission Date"
								  readonly
								></v-text-field>
							</v-flex>
							<v-flex xs12 sm6 md4>

							</v-flex>
							<v-flex xs12 sm6 md4>
								 <b>Attachment</b>
							</v-flex>
							<v-flex xs12 sm6 md4>

							</v-flex>
							<v-flex xs12 sm6 md4>
								 <label>Voucher</label>
									<!--<v-file-input  label="File input"></v-file-input>-->
								<input type="file" 	ref="voucherupload" id="voucherupload"	accept="image/x-png, image/gif, image/jpeg,application/pdf" v-on:change="handleFileUploadVoucher()" /><span id="voucherUploaderr" ref="voucherUploaderr" style='color:red'></span>
							</v-flex>
							<v-flex xs12 sm6 md4>
								 <label>Bill</label>
								  <input type="file" ref="billupload" id="billupload" accept="image/x-png, image/gif, image/jpeg,application/pdf"   v-on:change="handleFileUploadBill()" /><span id="billUploaderr" ref="billUploaderr" style='color:red'></span>

							</v-flex>
							<v-flex xs12 sm6 md4>

							</v-flex>
							<v-flex xs12 sm6 md4>

							</v-flex>

							 <v-btn class="mr-4" @click="submit">submit</v-btn>
							 <v-btn @click="clear">clear</v-btn>

                      </v-layout>
                    </v-container>
                  </v-card-text>
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
	  isLoading: false,
      fullPage: true,
	  selbranch:[],
      voucherno: '',
      category: [],
      branch :[],
      items: [],
	  expensedate:'',
	  amount:'',
	  sumbissiondate:'',
	  remarks:'',
	  vendorname:'',
	  billno:'',
	  show: false,
	  pettycash_branch : null,
	  pettycash_credit : null,
	  pettycash_debit : null,
	  pettycash_balance : null,
	  fileVoucher:null,
	  fileBill:null,
      today: "",
      maxDate: curday('-')
    }),


	mounted() {
		this.loadbranch();
		this.loadcategoty();
		this.getToday();
		this.sumbissiondate=this.today;

    },

    methods: {
	  getToday () {
		this.today = moment().format("YYYY-MM-DD");
	  },
      submit () {

		  if(this.selbranch==''){
			alert("Branch is required");
			return false;
		  }
		  if(this.voucherno==''){
			alert("Voucher No is required");
			return false;
		  }
		  if(this.category==''){
			alert("Category is required");
			return false;
		  }
		  if(this.expensedate==''){
			alert("Bill/ Expense Date is required");
			return false;
		  }
		  if(this.amount==''){
			alert("Amount is required");
			return false;
		  }
		if(!this.$refs.voucherupload.files[0]){
		    alert("Voucher is required");
			return false;
		}
		if ((this.fileVoucher.size  > 500000)) {
		    alert("Voucher file is greater than 500KB");
			return false;
		}
		if (this.fileBill!=null) {
		    if ((this.fileBill.size  > 500000)){
				alert("Bill file is greater than 500KB");
				return false;
			}
		}
    if(this.amount<=0 ){
      alert("Please check the amount ")
      return false;
    }
        var formData = new FormData()
		let normalusername = JSON.parse(sessionStorage.getItem("normal_user"));


		formData.append("branch", this.selbranch);
		formData.append("voucherno", this.voucherno);
		formData.append("category", this.category);
		formData.append("remarks", this.remarks);
		formData.append("expensedate", this.expensedate);
		formData.append("amount", this.amount);
		formData.append("vendorname", this.vendorname);
		formData.append("billno", this.billno);
		formData.append("sumbissiondate", this.sumbissiondate);
		formData.append("fileVoucher", this.fileVoucher);
		formData.append("fileBill", this.fileBill);
		formData.append("chid", normalusername.name);

		this.$http.post('http://localhost:8888/api-petty-cash-bill-submit', formData, {}).then(res => {
				this.isLoading = true;
				alert(res.data.ResponseMsg);
				this.isLoading = false;
				this.clear();
				this.show = false;
        })



      },
	  handleFileUploadVoucher(){
		this.fileVoucher = this.$refs.voucherupload.files[0];
		if ((this.fileVoucher.size > 500000)) {
			 //alert("Voucher file is greater than 500KB");
			 return false;

		}
	  },
	  handleFileUploadBill(){
		this.fileBill = this.$refs.billupload.files[0];
		if ((this.fileBill.size > 500000)) {
			 //alert("Bill file is greater than 500KB");
			 return false;

		}
	  },
      clear () {
        this.voucherno = ''
        this.selbranch = [];
		this.category = [];
		this.remarks='';
		this.expensedate='';
		this.amount='';
		this.vendorname='';
		this.billno='';
		//this.sumbissiondate='';
		this.$refs.voucherupload.value = null;
		this.$refs.billupload.value = null;
		this.$refs.voucherUploaderr.innerText = '';
		this.$refs.billUploaderr.innerText = '';
		this.fileVoucher = null;
		this.fileBill = null;


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

	branchamount(selectObj) {
		  this.axios
			.get(`http://localhost:8888/api-bramch-allocated-amount/${selectObj}`).then(response => {
				//alert(response.data[0]['credit']);
				this.show = true;
				this.pettycash_branch=response.data[0]['branch'];
				this.pettycash_credit=response.data[0]['credit'];
				this.pettycash_debit=response.data[0]['debit'];
				this.pettycash_balance=response.data[0]['balance'];

			})
	},

	loadcategoty() {
          var arr2 = [{text: '',shortCode: 'Select All', }];
		  this.axios
			.get(`http://localhost:8888/api-pettycashcategory`).then(response => {
			  this.items = arr2.concat(response.data);

			})
	},

    },
  }
</script>

<style>
input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    margin: 0;
}
</style>
