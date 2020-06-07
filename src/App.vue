<template>
<v-app>
  <v-navigation-drawer fixed clipped app v-model="drawer" temporary v-if="show" mini-variant mini-variant-width=150 class="mt-5">
    <v-list dense>
      <v-list-tile v-for="item in tabItems" :key="item" @click="changeCategory(item)" @click.stop="drawer = !drawer">
        <v-list-tile-action class="subheading text-xs-center font-weight-bold" style="text-decoration: none">
          {{ item }}
        </v-list-tile-action>
      </v-list-tile>
    </v-list>
  </v-navigation-drawer>
  <v-toolbar dense color="indigo darken-4">
    <v-toolbar-side-icon @click.stop="drawer = !drawer" class="white--text" v-if="show"></v-toolbar-side-icon>
    <v-toolbar-title v-text="title" class="white--text cursor" style="{ cursor: pointer;cursor: hand;}">
    </v-toolbar-title>
    <v-spacer></v-spacer>
    <v-menu bottom offset-y v-if="show" class="ml-2">
      <!-- <v-toolbar-title slot="activator">
          <span class="white--text">Settings</span>
          <v-icon dark>arrow_drop_down</v-icon>
        </v-toolbar-title> -->
      <template slot="activator">
        <span class="white--text body-2 mr-2">{{userName | nameFilter}}</span>
        <v-icon dark>fa fa-user-circle</v-icon>
      </template>
      <v-list>
        <v-list-tile v-for="(item,index) in items" :key="index" @click="changeCategory(item.title)">
          <v-list-tile-title>{{item.title}}</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-menu>
  </v-toolbar>
  <v-content>
    <transition name="fade" mode="out-in">
      <component :is="dynamicComponent"></component>
    </transition>
  </v-content>
</v-app>
</template>

<script>
// import cogsSuper from "./components/cogs-content";
import cogsSuper from './components/cogs-content-tabs';
import cogs from "./components/cogs-with-tabs";
import revenueSuper from "./components/revenue-content";
import revenue from "./components/revenue";
import chart from "./components/chart";
import login from "./components/login";
import ot from "./components/ot-content";
import revenuereport from "./components/revenuereport";
import newpod from "./components/newpod";
import newopdnormal from "./components/newopdnormal";
import changepassword from "./components/changepassword";
import Optical from "./components/optical";
import Discount from "./components/discount";
import Collection from "./components/collection";
import avamagic from "./components/avamagic";
import Newconsultation from "./components/consultation";
import DRT from "./components/chdrt";
import DRTApproval from "./components/schdrt";
import AdminApproval from "./components/admindrt.vue";
import DRTreport from "./components/admindrtreports.vue";

import {
  serverBus
} from "./main";
import away from "away";
const timer = away(1.8e6);

export default {
  data() {
    return {
      title: "DashBoard",
      dynamicComponent: "login",
      items: [{
        title: "Change Password"
      }, {
        title: "Logout"
      }],
      show: false,
      greet: "",
      drawer: false,
      tabItems: null,
      userName: null,
      userId: null,
      userType: null,
    };
  },
  name: "App",
  components: {
    chart,
    cogs,
    login,
    cogsSuper,
    revenueSuper,
    revenue,
    ot,
    revenuereport,
    newpod,
    newopdnormal,
    changepassword,
    Optical,
    Discount,
    Collection,
    avamagic,
    Newconsultation,
    DRT,
    DRTApproval,
    AdminApproval,
    DRTreport
  },
  created() {
    serverBus.$on("changeComponent", component => {
      this.dynamicComponent = component;
      this.show = true;
      // serverBus.$emit("changeAgain");

      if (sessionStorage.getItem('normal_user'))
      {

        let normSess = JSON.parse(sessionStorage.getItem('normal_user'))
        console.log(normSess.role);
        if (normSess.role == 'ch_user') {
          this.userName = normSess.userName
          this.userId = normSess.name
          this.userType = 'centerhead'
        } else if (normSess.role == 'sch_user') {
          this.userName = normSess.userName
          this.userId = normSess.name
          this.userType = 'strcenterhead'
        }  else {
          this.userName = normSess.userName
          this.userId = normSess.name
          this.userType = 'normal'

        }
      } else if (sessionStorage.getItem('super_user')) {
        let superSess = JSON.parse(sessionStorage.getItem('super_user'))
        this.userName = superSess.userName
        this.userId = superSess.name
        this.userType = 'super'
      } else if (sessionStorage.getItem('overseas_user')) {
        let superSess = JSON.parse(sessionStorage.getItem('overseas_user'))
        this.userName = superSess.userName
        this.userId = superSess.name
        this.userType = 'overcease'
      } else if (sessionStorage.getItem('optical_user')) {
        let superSess = JSON.parse(sessionStorage.getItem('optical_user'))
        this.userName = superSess.userName
        this.userId = superSess.name
        this.userType = 'optical'
      } else if (sessionStorage.getItem('coll_user')) {
        let superSess = JSON.parse(sessionStorage.getItem('coll_user'))
        this.userName = superSess.userName
        this.userId = superSess.name
        this.userType = 'collection'
      }
      else if (sessionStorage.getItem('fin_user')) {
        let superSess = JSON.parse(sessionStorage.getItem('fin_user'))
        this.userName = superSess.userName
        console.log(this.userName);
        this.userId = superSess.name
        console.log(this.userId);
        this.userType = 'financeuser'
      }
      else {
        let superSess = JSON.parse(sessionStorage.getItem('indian_user'))
        this.userName = superSess.userName
        this.userId = superSess.name
        this.userType = 'super'
      }

      if (this.userType == 'super' && this.userId != 103292) {

        if (this.userId == 103390) {
          this.tabItems = ['NewOPD']
        } else if (this.userId == 104860) {
          this.tabItems = ['Revenue', 'Cogs Vs Revn', 'Dashboard', 'NewOPD', 'Collection', 'Newconsultation']
        } else if (this.userType == 'super' && (this.userId == 104038 || this.userId == 102170 || this.userId == 103108 || this.userId == 'anosh')) {
          this.tabItems = ['Revenue', 'Cogs Vs Revn', 'Dashboard', 'NewOPD', 'Optical', 'Discount', 'Collection', 'AVA-Magic50', 'Newconsultation']
        } else {
          this.tabItems = ['Revenue', 'Cogs Vs Revn', 'Dashboard', 'NewOPD', 'Optical', 'Collection', 'Newconsultation']
        }
      }
      else if (this.userType == 'super' && this.userId == 103292) {
        this.tabItems = ['Revenue', 'Cogs Vs Revn', 'Dashboard', 'Surgery', 'NewOPD', 'Optical', 'Discount', 'Collection', 'AVA-Magic50', 'Newconsultation']
      }
      else if (this.userType == 'overcease') {
        this.tabItems = ['Cogs Vs Revn', 'NewOPD']
      }
      else if (this.userType == 'optical') {
        this.tabItems = ['Optical']
      }
      else if (this.userType == 'collection') {
        this.tabItems = ['Collection']
      }
      else if (this.userType == 'centerhead') {

        this.tabItems = ['Revenue', 'Cogs Vs Revn', 'NewOPD', 'DRT']
      }
      else if (this.userType == 'strcenterhead') {

        this.tabItems = ['Revenue', 'Cogs Vs Revn', 'NewOPD', 'DRTApproval']
      }
      else if (this.userType == 'financeuser') {

        this.tabItems = ['AdminApproval','DRTreport']
      } else {
        this.tabItems = ['Revenue', 'Cogs Vs Revn', 'NewOPD']
      }


    });
    timer.on("idle", () => {
      this.logout();
    });
  },
  methods: {
    logout() {
      //  this.$http.get(`https://scm.dragarwal.com/logout`).then(response => {
      this.$http.get(`http://localhost:8888/logout`).then(response => {
        this.errors = [];
        this.dynamicComponent = "login";
        this.show = false;
        sessionStorage.clear();
      });
    },
    changeCategory(item) {
      if ((sessionStorage.getItem('super_user')) || (sessionStorage.getItem('overseas_user')) || (sessionStorage.getItem('indian_user')))

      {

        if (item === 'Revenue') serverBus.$emit('changeComponent', 'revenueSuper')
        if (item === 'Cogs Vs Revn') serverBus.$emit('changeComponent', 'cogsSuper')

      } else {
        if (item === 'Revenue') serverBus.$emit('changeComponent', 'revenue')
        if (item === 'Cogs Vs Revn') serverBus.$emit('changeComponent', 'cogs')
      }
      if ((sessionStorage.getItem('super_user')) || (sessionStorage.getItem('overseas_user')) || (sessionStorage.getItem('indian_user'))) {
        if (item === 'NewOPD') serverBus.$emit('changeComponent', 'newpod')

      } else {
        if (item === 'NewOPD') serverBus.$emit('changeComponent', 'newopdnormal')


      }
      if (item === 'Dashboard') {
        serverBus.$emit('changeComponent', 'chart')
      }
      if (item === 'Surgery') {
        serverBus.$emit('changeComponent', 'ot')
      }
      if (item === 'RevenueReport') {
        serverBus.$emit('changeComponent', 'revenuereport')
      }
      if (item === 'Logout') {
        this.logout();
      }
      if (item === 'Change Password') {
        serverBus.$emit('changeComponent', 'changepassword')
      }
      if (item === 'Discount') {
        serverBus.$emit('changeComponent', 'Discount')
      }


      if ((sessionStorage.getItem('super_user')) || (sessionStorage.getItem('optical_user')) || (sessionStorage.getItem('indian_user')) || (sessionStorage.getItem('collection'))) {
        if (item === 'Optical') serverBus.$emit('changeComponent', 'Optical')
        if (item === 'Collection') serverBus.$emit('changeComponent', 'Collection')

      }


      if (item === 'AVA-Magic50') {
        serverBus.$emit('changeComponent', 'avamagic')
      }
      if (item === 'Newconsultation') {
        serverBus.$emit('changeComponent', 'Newconsultation')
      }


      if (item === 'DRT') {
        serverBus.$emit('changeComponent', 'DRT')
      }

      if (item === 'DRTApproval') {
        serverBus.$emit('changeComponent', 'DRTApproval')
      }
      if (item === 'AdminApproval') {
        serverBus.$emit('changeComponent', 'AdminApproval')
      }
      if (item === 'DRTreport') {
        serverBus.$emit('changeComponent', 'DRTreport')
      }


    }
    // ,
    // home () {
    //   serverBus.$emit('changeComponent', 'chart')
    // }
  },
  filters: {
    nameFilter(name) {
      return name.split(' ')[0]
    }
  }
};
</script>

<style>
[v-cloak] {
  display: none;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.2s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave {
  opacity: 0;
}

v-list-tile-action:hover {
  text-decoration: none;
}

.cursor {
  cursor: pointer;
  cursor: hand;
  /* touch-action: none */
}
</style>
