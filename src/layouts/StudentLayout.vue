<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-info bg-main no-shadow text-main">
      <q-toolbar>
        <q-btn
          flat
          v-if="title != 'Dashboard'"
          v-go-back.single
          dense
          round
          icon="arrow_back"
          aria-label="Logout"
        />
        <q-toolbar-title class="text-center">
          {{ title }}
        </q-toolbar-title>
        <q-btn
          flat
          dense
          round
          @click="logout"
          icon="login"
          aria-label="Logout"
        />

      </q-toolbar>
    </q-header>

    <q-footer elevated bordere class="justify-center bg-white text-main no-shadow">
      <q-tabs dense no-caps class="text-black">
        <q-route-tab
          v-for="nav in navs"
          :key="nav.label"
          :to="nav.to"
          :icon="nav.icon"
          :class="nav.is_center ? 'center' : ''"
          :label="nav.is_center ? '' : nav.label"
        >
        </q-route-tab>
      </q-tabs>
    </q-footer>


    <q-page-container class="bg-teal-1">
      <transition :name="transitionName">
        <keep-alive>
          <router-view class="child-slide"></router-view>
        </keep-alive>
      </transition>
    </q-page-container>
  </q-layout>
</template>

<script>


import {mapState} from "vuex";

export default {
  name: 'StudentLayout',
  data () {
    return {
      leftDrawerOpen: false,

      navs: [
        {
          label: "Profile",
          icon: "person_outline",
          is_center:0,
          // to: "/home"
          to:'/student/profile'
        },
        {
          label: "Categories",
          icon: "view_module",
          is_center:1,
          is_:false,
          to: "/student/home"
        },
        {
          label: "Notice",
          icon: "notification_important",
          is_center:0,
          to: "/student/notice"
        }
      ],

      transitionName: ''
    }
  },

  methods:{
    logout(){
      this.$router.push('/auth/login');
      // firebaseAuth.signOut().then(()=>{
      //   this.$store.commit("store/logout");
      //   this.$router.push('/login');
      // })
    }
  },

  computed:{
    // ...mapState('store', ['carts', 'currentUser']),
    title() {
      let currentPath = this.$route.fullPath;
      if (currentPath == '/') return 'Dashboard'
      else return this.$route.meta.name
    },
    // ...mapState('store', ['currentUser']),
  },

  beforeRouteUpdate (to, from, next) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'overlap-left' : 'overlap-right'
    next()
  }
}
</script>

<style lang="scss">
.center {
  .q-tab__icon{
    margin-bottom: -20px;
  }
  color: #26A69A;
  .q-tab__icon{
    font-size: 45px!important;
    background: black;
    padding: 15px;
    border-radius: 50%;
    margin-bottom: 5px;
  }
}
</style>
