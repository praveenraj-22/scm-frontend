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
            <v-toolbar-title>RevenueBudget </v-toolbar-title>
            <v-divider
              class="mx-2 black"
              inset
              vertical
            ></v-divider>
            <v-spacer></v-spacer>
			
			
			<v-select :items=branch v-model="SetBranch" label="Branch:" item-text="text" item-value="shortCode" id="SelBranch"></v-select>
			<v-spacer></v-spacer>
			<v-btn rounded color="primary" dark @click="apiRequestCogsdata(SetBranch)">GET</v-btn>
		    
            <!--<v-menu
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
                min="2018-04-01"
                :max="today"
                backgroundRevenue-color="grey"
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
                  @click="$refs.menu.save(date);apiRequestOPDSuper(date)"
                  style="outline:none"
                >Generate</v-btn>
              </v-date-picker>
			  
			  
			  
			  
			  
            </v-menu>-->
            <!--<download-excel
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
            </download-excel>-->
          </v-toolbar>
          <loading
            :active.sync="isLoading"
            :is-full-page="fullPage"
            color="#7f0000"
            loader="bars"
          ></loading>
          <!-- Vuetify Data table -->
		  <div class="table-responsive" column >
		   <table border="1" width="100%" class="opr" style="table-layout: fixed"><tr><th class="text-xs-center" width="5%"><b>Desc</b></th> <th class="text-xs-center" width="5%"><b>Year</b></th> <th class="text-xs-center" width="5%"><b>Unit</b></th> <th class="text-xs-center" width="5%"><b>Apr</b></th> <th class="text-xs-center" width="5%"><b>May</b></th> <th class="text-xs-center" width="5%"><b>Jun</b></th> <th class="text-xs-center" width="5%"><b>Jul</b></th> <th class="text-xs-center" width="5%"><b>Aug</b></th> <th class="text-xs-center" width="5%"><b>Sep</b></th> <th class="text-xs-center" width="5%"><b>Oct</b></th> <th class="text-xs-center" width="5%"><b>Nov</b></th> <th class="text-xs-center" width="4.9%"><b>Dec</b></th> <th class="text-xs-center" width="4.9%"><b>Jan</b></th> <th class="text-xs-center" width="4.9%"><b>Feb</b></th> <th class="text-xs-center" width="4.9%"><b>Mar</b></th> <th class="text-xs-center" width="4.9%"><b>YTD</b></th> <th class="text-xs-center" width="4.9%"><b>Q1</b></th> <th class="text-xs-center" width="4.9%"><b>Q2</b></th> <th class="text-xs-center" width="4.9%"><b>Q3</b></th> <th class="text-xs-center" ><b>Q4</b></th></tr></table>
		   </div>
		 
          <div class="table-responsive" column style="height: 90vh">
            <table
              border="1"
              v-if="show"
			  width="100%"
			  class="opr"
			  style="table-layout: fixed"
            >
              
              <tbody>
			  
			  
			  
				<tr  >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Sales / Turnover <br>(with Tax)</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdrevtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaltarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdrevtarget)}}</td>
                </tr>
				
				
				
				
				<tr class="branchesgrp1">
                 
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdrev)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalrev)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col" 
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdrev)}}</td>
                </tr>
				<tr >
                  <td
                    class="text-xs-center"
                    scope="col"
                     width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdrevtarget)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaltarget)}}</td>
				 
				   <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdrevtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdrevtarget)}}</td>
				  
                </tr>
				<tr class="branchesgrp1">
                 <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdrev)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalrev)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdrev)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdrev)}}</td>
                </tr>
				<tr >
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdrevper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.mtdrevper)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdrevper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdrevper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdrevper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdrevper)}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				<tr  >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >OP New</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopdtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopdtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopdtarget)}}</td>
                </tr>
				
				<tr  class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopd)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopd)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopd)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopdtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopdtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopdtarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopd)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopd)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopd)}}</td>
                </tr>
				
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdopdper)}}
				  </td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.mtdopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdopdper)}}</td>
				  
                </tr>
				
				<br><br>
				
				
				<tr  >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >OP Review Paid</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdpopdtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalpopdtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdpopdtarget)}}</td>
                </tr>
				
				<tr  class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdpopd)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalpopd)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdpopd)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdpopdtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalpopdtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdpopdtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdpopdtarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdpopd)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalpopd)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdpopd)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdpopd)}}</td>
                </tr>
				
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdpopdper)}}
				  </td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.popdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdpopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdpopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdpopdper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdpopdper)}}</td>
				  
                </tr>



				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Total Consultation charges</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdtconsutarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totaltconsutarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdtconsutarget}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdtconsu}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totaltconsu}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdtconsu}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdtconsutarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totaltconsutarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdtconsutarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdtconsutarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdtconsu}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totaltconsu}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdtconsu}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdtconsu}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdtconsuper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.tconsuper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdtconsuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdtconsuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdtconsuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdtconsuper)}}</td>
				  
                </tr>

				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >YPP (New OP)</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopdypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopdypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopdypptarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopdypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopdypp)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopdypp)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopdypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopdypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopdypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopdypptarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopdypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopdypp)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopdypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopdypp)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdopdyppper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.opdyppper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdopdyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdopdyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdopdyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdopdyppper)}}</td>
				  
                </tr>
								
				<br><br>
				
				
				<tr >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract Surgery nos</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  > {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcatcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalallcatcounttarget)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcatcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalallcatcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcatcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  > {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcatcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalallcatcounttarget)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcatcounttarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcatcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalallcatcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdallcatcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcatcount)}}</td>
                </tr>
				
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdallcatcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctallcountper)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdallcatcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdallcatcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdallcatcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdallcatcountper)}}</td>
				  
                </tr>	



				<br><br>
				
				<tr >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract Surgery value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  > FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcattarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalallcattarget)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcattarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcat)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalallcat)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdallcat)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  > {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcattarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalallcattarget)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdallcattarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcattarget)}}</td>
                </tr>
				
				
				
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdallcat)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalallcat)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdallcat)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdallcat)}}</td>
                </tr>
				
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdallcatper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctallper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdallcatper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdallcatper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdallcatper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdallcatper)}}</td>
				  
                </tr>	
				
				
				<br><br>
				
                <tr  >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Low End nos</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctlowendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctlowendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctlowendcounttarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctlowendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctlowendcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctlowendcount)}}</td>
                </tr>
				
				
				 <tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctlowendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctlowendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctlowendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctlowendcounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctlowendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctlowendcount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctlowendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctlowendcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                     width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdctlowendcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctlowendcountper)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdctlowendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdctlowendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdctlowendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdctlowendcountper)}}</td>
				  
                </tr>
				<br><br>
				
				
				<tr  >
                  <td rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Low End Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctlowendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctlowendtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctlowendtarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctlowend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctlowend)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctlowend)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctlowendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctlowendtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctlowendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctlowendtarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctlowend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctlowend)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctlowend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctlowend)}}</td>
                </tr>
				
				
				<tr >
                 
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdctlowendper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctlowendper)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdctlowendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdctlowendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdctlowendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdctlowendper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Mid End nos</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctmidendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctmidendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctmidendcounttarget)}}</td>
                </tr>
				
				
				
				<tr class="branchesgrp1">
                 
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctmidendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctmidendcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctmidendcount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctmidendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctmidendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctmidendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctmidendcounttarget)}}</td>
                </tr>
				
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctmidendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctmidendcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctmidendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctmidendcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdctmidendcountper)}}
				  </td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctmidendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdctmidendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdctmidendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdctmidendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdctmidendcountper)}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Mid End Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctmidendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctmidendtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctmidendtarget)}}</td>
                </tr>
				
				
				<tr class="branchesgrp1">
                 
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctmidend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalctmidend)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdctmidend)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdctmidendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctmidendtarget)}}</td>
				 
				  
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctmidendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctmidendtarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdctmidend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalctmidend)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdctmidend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdctmidend)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdctmidendper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.ctmidendper)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdctmidendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdctmidendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdctmidendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdctmidendper)}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX High End No</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcthighendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcthighendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendcounttarget)}}</td>
                </tr>
			
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcthighendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcthighendcount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendcount)}}</td>
                </tr>
				
				
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcthighendcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcthighendcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcthighendcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcthighendcounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcthighendcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcthighendcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcthighendcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcthighendcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdcthighendcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.cthighendcountper)}}</td>
				
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdcthighendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdcthighendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdcthighendcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdcthighendcountper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX High End Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcthighendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcthighendtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighendtarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcthighend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcthighend)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcthighend)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcthighendtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcthighendtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcthighendtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcthighendtarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcthighend)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcthighend)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcthighend)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcthighend)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdcthighendper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.cthighendper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdcthighendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdcthighendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdcthighendper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdcthighendper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Corneal  No</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcorcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcorcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcorcounttarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcorcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcorcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcorcount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcorcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcorcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcorcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcorcounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcorcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcorcount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcorcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcorcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdcorcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.corcountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdcorcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdcorcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdcorcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdcorcountper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Corneal  Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcortarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcortarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcortarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcor)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalcor)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdcor)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdcortarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcortarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcortarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcortarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdcor)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalcor)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdcor)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdcor)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdcorper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.corper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdcorper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdcorper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdcorper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdcorper)}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Refractive Nos</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdrefcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalrefcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdrefcounttarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdrefcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalrefcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdrefcount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdrefcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalrefcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdrefcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdrefcounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdrefcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalrefcount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdrefcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdrefcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdrefcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.refcountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdrefcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdrefcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdrefcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdrefcountper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Refractive Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdreftarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalreftarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdreftarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdref)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalref)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdref)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdreftarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalreftarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdreftarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdreftarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdref)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalref)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdref)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdref)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdrefper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.refper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdrefper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdrefper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdrefper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdrefper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >VR Surgery No</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdvrsugcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalvrsugcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdvrsugcounttarget}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdvrsugcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalvrsugcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdvrsugcount}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdvrsugcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalvrsugcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdvrsugcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdvrsugcounttarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdvrsugcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalvrsugcount}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdvrsugcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdvrsugcount}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdvrsugcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.vrsugcountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdvrsugcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdvrsugcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdvrsugcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdvrsugcountper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >VR Surgery Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdvrsugtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalvrsugtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdvrsugtarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdvrsug)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalvrsug)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdvrsug)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdvrsugtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalvrsugtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdvrsugtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdvrsugtarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdvrsug)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalvrsug)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdvrsug)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdvrsug)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdvrsugper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.vrsugper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdvrsugper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdvrsugper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdvrsugper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdvrsugper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >VR Injections No</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdvrincounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalvrincounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdvrincounttarget}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdvrincount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalvrincount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdvrincount}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{item.mtdvrincounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalvrincounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdvrincounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdvrincounttarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdvrincount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalvrincount}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdvrincount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdvrincount}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdvrincountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.vrincountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdvrincountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdvrincountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdvrincountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdvrincountper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >VR Injections Value</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdvrintarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalvrintarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdvrintarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdvrin)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalvrin)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdvrin)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdvrintarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalvrintarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdvrintarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdvrintarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdvrin)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalvrin)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdvrin)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdvrin)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdvrinper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.vrinper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdvrinper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdvrinper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdvrinper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdvrinper)}}</td>
				  
                </tr>
				
				
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Opticals No. of Orders </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdoptcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaloptcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdoptcounttarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdoptcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaloptcount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdoptcount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdoptcounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaloptcounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdoptcounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdoptcounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdoptcount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaloptcount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdoptcount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdoptcount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdoptcountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.optcountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdoptcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdoptcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdoptcountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdoptcountper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Opticals Value (Delivered)</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopttarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopt)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalopt)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdopt)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdopttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdopt)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalopt)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdopt)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdopt)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdoptper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.optper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdoptper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdoptper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdoptper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdoptper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Opticals YPP</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdoptypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaloptypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdoptypptarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdoptypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaloptypp)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdoptypp)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdoptypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaloptypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdoptypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdoptypptarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdoptypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaloptypp)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdoptypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdoptypp)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdoptyppper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.optper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdoptyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdoptyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdoptyppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdoptyppper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Pharmacy No. of Bills </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphacounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalphacounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdphacounttarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdphacount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalphacount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdphacount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphacounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalphacounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdphacounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdphacounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdphacount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalphacount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdphacount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdphacount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdphacountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.phacountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdphacountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdphacountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdphacountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdphacountper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Pharmacy Value </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphatarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalphatarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdphatarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdpha)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalpha)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdpha)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphatarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalphatarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdphatarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdphatarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdpha)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalpha)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdpha)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdpha)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdphaper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.phaper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdphaper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdphaper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdphaper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdphaper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Pharmacy YPP</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphaypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalphaypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdphaypptarget)}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdphaypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalphaypp)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdphaypp)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdphaypptarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalphaypptarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdphaypptarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdphaypptarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Rs.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdphaypp)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalphaypp)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdphaypp)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdphaypp)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdphayppper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.phaper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdphayppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdphayppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdphayppper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdphayppper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Contact Lens</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdconlenstarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalconlenstarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdconlenstarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdconlens)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalconlens)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdconlens)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdconlenstarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalconlenstarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdconlenstarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdconlenstarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdconlens)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalconlens)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdconlens)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdconlens)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdconlensper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.conlensper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdconlensper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdconlensper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdconlensper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdconlensper)}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Other surgery Nos </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdosucounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalosucounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdosucounttarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdosucount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalosucount)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdosucount)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdosucounttarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalosucounttarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdosucounttarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdosucounttarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >Nos.</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdosucount)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalosucount)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdosucount)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdosucount)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdosucountper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.osucountper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdosucountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdosucountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdosucountper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdosucountper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Other surgery Value </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdosutarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalosutarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdosutarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdosu)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totalosu)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdosu)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdosutarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalosutarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdosutarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdosutarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdosu)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totalosu)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdosu)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdosu)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdosuper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.osuper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdosuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdosuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdosuper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdosuper)}}</td>
				  
                </tr>
				
				
				<br><br>
				
				<tr  >
                  <td
				    rowspan="5"
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Invs, Treat Value </td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.preYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdtreatinvtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaltreatinvtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdtreatinvtarget)}}</td>
                </tr>
				<tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.preYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in preYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdtreatinv)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueTotal.totaltreatinv)}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ1[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ2[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ3[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(preYearRevenueQ4[0].mtdtreatinv)}}</td>
                </tr>
				
				
				<tr  >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >FY {{this.curYeartext}} Target</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center"
                    scope="col"
					width="5%">
				  {{numFormat(item.mtdtreatinvtarget)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaltreatinvtarget)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdtreatinvtarget)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdtreatinvtarget)}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >FY {{this.curYeartext}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >RIL</td>
				
				  
				  <td v-for="(item,indexs) in currentYearRevenue" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numFormat(item.mtdtreatinv)}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueTotal.totaltreatinv)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ1[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ2[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ3[0].mtdtreatinv)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numFormat(currentYearRevenueQ4[0].mtdtreatinv)}}</td>
                </tr>
				
				
				<tr >
                  
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >{{goalText}}</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{numPosNeg(item.mtdtreatinvper)}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(totalrevper.treatinvper)}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ1[0].mtdtreatinvper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ2[0].mtdtreatinvper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ3[0].mtdtreatinvper)}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{numPosNeg(revenuePerQ4[0].mtdtreatinvper)}}</td>
				  
                </tr>
				
				
				

              </tbody>
				
				

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
    branch: [],
	  SetBranch: [],
	  currentYearRevenue :null,
	  currentYearRevenueTar :null,
	  currentYearRevenueTotal :null,
	  currentYearRevenueTarTotal :null,
	  currentYearRevenueQ1 :null,
	  currentYearRevenueQ2 :null,
	  currentYearRevenueQ3 :null,
	  currentYearRevenueQ4 :null,
	  revenuePerQ1 :null,
	  revenuePerQ2 :null,
	  revenuePerQ3 :null,
	  revenuePerQ4 :null,
	  preYearRevenueQ1 :null,
	  preYearRevenueQ2 :null,
	  preYearRevenueQ3 :null,
	  preYearRevenueQ4 :null,
	  preYearRevenueTotal :null,
	  preYearRevenueTarTotal :null,
	  preYearRevenue :null,
	  preYearRevenueTar :null,
	  preYearRevenueQ :null,
	  revPer :null,
	  totalrevper:null,
	  curYeartext:null,
	  preYeartext:null,
	  goalText:null,
	  
    rowColor: null,
    isActive: false,
    save: "save",
    show: false,
    fileDate: null,
    loading: false,
    date: null,
    menu: false,
    modal: false,
    menu2: false,
    today: "",
	group: null,
	state: null,
    branchwise: null,
    json_data: null,
    json_meta: [
      {
        key: "charset",
        value: "utf-8"
      }
    ],
    json_fields: {
      "Group/Branch": "branch",
      "FTD": "ftdopdrev",
      "MTD": "mtdopdrev",
	  "LMTD": "mtdopdrevlastyear",
	  "MTD%": "mtdopdpercentage",
	  
      
    },
    fileName: null
  }),
  created () {
    this.getToday();
	this.loadbranch();
	this.SetBranch='ADY';
	this.apiRequestCogsdata(this.SetBranch);
  },
  methods: {
    getToday () {
      this.today = moment()
        .subtract(1, "days")
        .format("YYYY-MM-DD");
    },
    apiRequestCogsdata(SetBranch){

  //alert(this.SetBranch);

	this.isLoading = true;
    
	
	this.$http.get(`https://mis.dragarwal.com/api-snapshotrevenue/${this.SetBranch}`).then(response => {

      //console.log(response.data);
      this.processDatacogs(response.data);
		this.isLoading = false;
		this.show = true;
     
    })




},
    processDatacogs (data) {

        this.curYeartext = data.curYear;
		this.preYeartext = data.preYear;
		this.goalText = data.goalText;
	    this.currentYearRevenue = data.monthWiseRevenue['currentyearmonth'];
		
		
	   
	    this.currentYearRevenueQ1 = data.QWiseRevenue['currentyearQ']['Q1'];	
		this.currentYearRevenueQ2 = data.QWiseRevenue['currentyearQ']['Q2'];
		this.currentYearRevenueQ3 = data.QWiseRevenue['currentyearQ']['Q3'];
		this.currentYearRevenueQ4 = data.QWiseRevenue['currentyearQ']['Q4'];
		
		
		this.preYearRevenueQ1 = data.QWiseRevenue['lastyearQ']['Q1'];	
		this.preYearRevenueQ2 = data.QWiseRevenue['lastyearQ']['Q2'];
		this.preYearRevenueQ3 = data.QWiseRevenue['lastyearQ']['Q3'];
		this.preYearRevenueQ4 = data.QWiseRevenue['lastyearQ']['Q4'];
		
		
		this.revenuePerQ1 =data.QWiseRevenue['revenueQper']['Q1'];	
		this.revenuePerQ2 =data.QWiseRevenue['revenueQper']['Q2'];
		this.revenuePerQ3 =data.QWiseRevenue['revenueQper']['Q3'];
		this.revenuePerQ4 =data.QWiseRevenue['revenueQper']['Q4'];
		this.totalrevper = data.monthWiseRevenue['totalper']
		
	    this.preYearRevenue =data.monthWiseRevenue['lastyearmonth'];
		this.revPer =data.monthWiseRevenue['monthper'];
		
	    
	    this.preYearRevenueQ =null;
		this.currentYearRevenueTotal = data.monthWiseRevenue['currentyeartotal'];
		this.preYearRevenueTotal =data.monthWiseRevenue['lastyeartotal'];
	    //this.preYearRevenueTarTotal =data.lastyeartotal.totalrev;
    },
	numFormat(number){
		let numObj=new Intl.NumberFormat("en-IN");
		return numObj.format(number);
	},
	numPosNeg(num){
		if(num>0 && ( num != "Infinity" && num!='NaN')){
		    return num;
		}else{
			if(num != "Infinity" && num!='NaN'){
				return "("+Math.abs(num)+")";
			}else{
			
			  
				return "-";
			}
		}
	},
	loadbranch() {
			var arr2 = [{text: 'Select',shortCode: '' },{text: 'Chennai',shortCode: 'Chennai' },{text: 'Tamil Nadu',shortCode: 'Tamil Nadu' },{text: 'ROTN',shortCode: 'ROTN' },{text: 'Karnataka',shortCode: 'Karnataka' },{text: 'Andhra Pradesh',shortCode: 'Andhra Pradesh' },{text: 'Telangana',shortCode: 'Telangana' },{text: 'Maharashtra',shortCode: 'Maharashtra' },{text: 'Madhya Pradesh',shortCode: 'Madhya Pradesh' },{text: 'Odisha',shortCode: 'Odisha' },{text: 'ROI',shortCode: 'ROI' },{text: 'Kerala',shortCode: 'Kerala' },{text: 'Kolkata',shortCode: 'Kolkata' }];		
			this.$http.get(`https://mis.dragarwal.com/api-opr-branches`).then(response => {
			         //console.log(response.data);
					 this.branch = arr2.concat(response.data);		
     
			})
	},
    downloadExcelRevenueSuper () {
	
		
    },
	exportXL(){
		
	
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

.branchesgrp1{
background-color : #d0dff7!important
}



.opr  tbody > tr > td:nth-last-child(5) {background-color : #fcd6ec!important}
.opr  tbody > tr > td:nth-last-child(1) {background-color : #a8f3f7!important}
.opr  tbody > tr > td:nth-last-child(2) {background-color : #a8f3f7!important}
.opr   tbody > tr > td:nth-last-child(3) {background-color : #a8f3f7!important}
.opr   tbody > tr > td:nth-last-child(4) {background-color : #a8f3f7!important}
</style>
