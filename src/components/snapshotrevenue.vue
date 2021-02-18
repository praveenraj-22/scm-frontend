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
			<v-select :items=branch v-model="SetBranch" label="Branch:" item-text="shortCode" item-value="text" id="SelBranch"></v-select>
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
		  <table
              border="1"
              v-if="show"
			  width="98%"
			
            >
              
                <tr>
                  <td
                    class="text-xs-left"
                    scope="col"
                    width="5.1%"                
                  ><b>Desc</b></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5.1%"
                  ><b>Year</b></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5.1%"
                  ><b>Unit</b></td>
				 
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                     width="5%"
                  ><b>Apr</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>May</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Jun</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Jul</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Aug</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  ><b>Sep</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Oct</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Nov</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Dec</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Jan</b></td>
				  <td
                    class="text-xs-center"
                   scope="col"
                    width="5%"
                  ><b>Feb</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Mar</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>YTD</b></td>
				  
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Q1</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Q2</b></td>
				  <td
                    class="text-xs-center"
                    width="5%"
                    cope="col"
                  ><b>Q3</b></td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  ><b>Q4</b></td>
                </tr>
				
				
				
               
              
		  </table>
          <div class="table-responsive" column style="height: 90vh">
            <table
              border="1"
              v-if="show"
			  width="100%"
			 
            >
              
              <tbody>
			  
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdrevtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totaltarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdrevtarget}}</td>
                </tr>
				
				
				
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdrev}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalrev}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col" 
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdrev}}</td>
                </tr>
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Sales / Turnover <br>(with Tax)</td>
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
				  {{item.mtdrevtarget}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totaltarget}}</td>
				 
				   <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdrevtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdrevtarget}}</td>
				  
                </tr>
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdrev}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalrev}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdrev}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdrev}}</td>
                </tr>
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdrevper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.mtdrevper}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdrevper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdrevper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdrevper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdrevper}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdopdtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalopdtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdopdtarget}}</td>
                </tr>
				
				<tr  class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdopd}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalopd}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdopd}}</td>
                </tr>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >OP New</td>
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
				  {{item.mtdopdtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalopdtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdopdtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdopdtarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdopd}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalopd}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdopd}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdopd}}</td>
                </tr>
				
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdopdper}}
				  </td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.mtdopdper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdopdper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdopdper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdopdper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdopdper}}</td>
				  
                </tr>	
				
								
				<br><br>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcatcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalallcatcounttarget}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcatcounttarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcatcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalallcatcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcatcount}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract Surgery nos</td>
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
				  {{item.mtdallcatcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalallcatcounttarget}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcatcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcatcounttarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcatcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalallcatcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdallcatcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcatcount}}</td>
                </tr>
				
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdallcatcountper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctallcountper}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdallcatcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdallcatcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdallcatcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdallcatcountper}}</td>
				  
                </tr>	



				<br><br>
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcattarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalallcattarget}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcattarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcat}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalallcat}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdallcat}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract Surgery value</td>
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
				  {{item.mtdallcattarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalallcattarget}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdallcattarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcattarget}}</td>
                </tr>
				
				
				
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdallcat}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalallcat}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdallcat}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdallcat}}</td>
                </tr>
				
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdallcatper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctallper}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdallcatper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdallcatper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdallcatper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdallcatper}}</td>
				  
                </tr>	
				
				
				<br><br>
				
                <tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctlowendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctlowendcounttarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctlowendcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctlowendcount}}</td>
                </tr>
				
				
				 <tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Low End nos</td>
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
				  {{item.mtdctlowendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctlowendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctlowendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctlowendcounttarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctlowendcount}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctlowendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctlowendcount}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                     width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdctlowendcountper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctlowendcountper}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdctlowendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdctlowendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdctlowendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdctlowendcountper}}</td>
				  
                </tr>
				<br><br>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctlowendtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctlowendtarget}}</td>
                </tr>
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctlowend}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctlowend}}</td>
                </tr>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Low End Value</td>
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
				  {{item.mtdctlowendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctlowendtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctlowendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctlowendtarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctlowend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctlowend}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctlowend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctlowend}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdctlowendper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctlowendper}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdctlowendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdctlowendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdctlowendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdctlowendper}}</td>
				  
                </tr>
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctmidendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctmidendcounttarget}}</td>
                </tr>
				
				
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctmidendcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctmidendcount}}</td>
                </tr>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Mid End nos</td>
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
				  {{item.mtdctmidendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctmidendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctmidendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctmidendcounttarget}}</td>
                </tr>
				
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctmidendcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctmidendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctmidendcount}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdctmidendcountper}}
				  </td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctmidendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdctmidendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdctmidendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdctmidendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdctmidendcountper}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctmidendtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctmidendtarget}}</td>
                </tr>
				
				
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalctmidend}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdctmidend}}</td>
                </tr>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX Mid End Value</td>
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
				  {{item.mtdctmidendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctmidendtarget}}</td>
				 
				  
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctmidendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctmidendtarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalctmidend}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdctmidend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdctmidend}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdctmidendper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.ctmidendper}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdctmidendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdctmidendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdctmidendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdctmidendper}}</td>
				  
                </tr>
				
				
				
				<br><br>
				
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdctmidendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalcthighendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendcounttarget}}</td>
                </tr>
			
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdcthighendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalcthighendcount}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendcount}}</td>
                </tr>
				
				
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX High End No</td>
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
				  {{item.mtdcthighendcounttarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalcthighendcounttarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdcthighendcounttarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdcthighendcounttarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdcthighendcount}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalcthighendcount}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdcthighendcount}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdcthighendcount}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdcthighendcountper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.cthighendcountper}}</td>
				
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdcthighendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdcthighendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdcthighendcountper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdcthighendcountper}}</td>
				  
                </tr>
				
				<br><br>
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdcthighendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalcthighendtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighendtarget}}</td>
                </tr>
				<tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdcthighend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueTotal.totalcthighend}}</td>
				  
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ1[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ2[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ3[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{preYearRevenueQ4[0].mtdcthighend}}</td>
                </tr>
				
				
				<tr  >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  >Cataract SX High End Value</td>
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
				  {{item.mtdcthighendtarget}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalcthighendtarget}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdcthighendtarget}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdcthighendtarget}}</td>
                </tr>
				
			    <tr class="branchesgrp1">
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
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
				  {{item.mtdcthighend}}
				  </td>
				 <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueTotal.totalcthighend}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ1[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ2[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ3[0].mtdcthighend}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{currentYearRevenueQ4[0].mtdcthighend}}</td>
                </tr>
				
				
				<tr >
                  <td
                    class="text-xs-left"
                    width="5%"
                    scope="col"                  
                  ></td>
                  <td
                    class="text-xs-center"
                    scope="col"
                   width="5%"
                  >GOLY % FY19 vs FY20</td>
                  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >%</td>
				
				  
				  <td v-for="(item,indexs) in revPer" 
				  class="text-xs-center" width="5%"
                    scope="col">
				  {{item.mtdcthighendper}}
				  </td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{totalrevper.cthighendper}}</td>
				 
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ1[0].mtdcthighendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ2[0].mtdcthighendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ3[0].mtdcthighendper}}</td>
				  <td
                    class="text-xs-center"
                    scope="col"
                    width="5%"
                  >{{revenuePerQ4[0].mtdcthighendper}}</td>
				  
                </tr>
				
				

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
    branch: [
       
		{ shortCode: 'CMH', text: 'CMH'},
		{ shortCode: 'ANN', text: 'ANN'},
		{ shortCode: 'ASN', text: 'ASN'},	
		{ shortCode: 'AVD', text: 'AVD'},	
		{ shortCode: 'NLR', text: 'NLR'},        
		{ shortCode: 'PMB', text: 'PMB'},	
		{ shortCode: 'PRR', text: 'PRR'}  
      ],
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
	this.SetBranch='CMH';
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
	    this.currentYearRevenue = data.monthWiseRevenue['currentyearmonth'];
		
		
	   
	    this.currentYearRevenueQ1 =data.QWiseRevenue['currentyearQ']['Q1'];	
		this.currentYearRevenueQ2 =data.QWiseRevenue['currentyearQ']['Q2'];
		this.currentYearRevenueQ3 =data.QWiseRevenue['currentyearQ']['Q3'];
		this.currentYearRevenueQ4 =data.QWiseRevenue['currentyearQ']['Q4'];
		
		
		this.preYearRevenueQ1 =data.QWiseRevenue['lastyearQ']['Q1'];	
		this.preYearRevenueQ2 =data.QWiseRevenue['lastyearQ']['Q2'];
		this.preYearRevenueQ3 =data.QWiseRevenue['lastyearQ']['Q3'];
		this.preYearRevenueQ4 =data.QWiseRevenue['lastyearQ']['Q4'];
		
		
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



</style>
