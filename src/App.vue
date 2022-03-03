<template>
  <v-app>
    <Navbar />
    <v-main class="mb-5">
    <PurchaseRequestForm />
    </v-main>
  </v-app>
</template>

<script>
import Navbar from '@/components/Navbar';
import PurchaseRequestForm from '@/components/PurchaseRequestForm';

  export default {
  name: 'App',
  components: { 
    Navbar,
    PurchaseRequestForm,
     },
  data() {
    return {
      //
    }
  }
  }
</script>

i