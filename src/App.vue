<template>
  <v-app>
    <Orderlist />
  </v-app>
</template>

<script>
import Orderlist from './components/orderlist.vue';

export default {
  name: 'App',

  components: {
    Orderlist
  },

  data: () => ({
    //
  }),
};
</script>
