<template>
  <div>
    <q-input
      @keyup.enter.native="handleEnterKeyPress"
      standout="bg-secondary text-white"
      bottom-slots
      v-model="newRoute"
      label="Enter new route"
      :dense="dense"
      class="input-add"
    ></q-input>
    <!--<div class="route-display">-->
    <!--

    -->
    <Routes></Routes>
  </div>
</template>

<script>
import Routes from './Routes'
import { mapState, mapActions } from 'vuex'

export default {
  name: 'RouteDisplay',
  components: {
    Routes
  },
  computed: {
    ...mapState(['routes'])
  },
  data () {
    return {
      newRoute: ''
    }
  },
  methods: {
    ...mapActions(['addRouteToRouteMap', 'setRoutes']),
    handleEnterKeyPress () {
      this.addRouteToRouteMap(this.newRoute)
        .then(() => {
          this.newRoute = ''
        })

        .catch(err => console.log(err))
    }
  }
}
</script>

<style scoped>
.route-display {
  border: 1px solid rgba(208, 208, 208, 0.694);
  overflow: auto;
  height: 60%;
  padding: 0;
  margin: 1rem;
  padding: 0.5rem;
  border-radius: 5px;
}

i:hover {
  cursor: pointer;
}
</style>
