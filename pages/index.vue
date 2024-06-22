<template>
  <div>
    <PersonsTable />
  </div>
  <div>
    <p>
    <nuxt-link to="/about">Go to About Page</nuxt-link>
    </p>
  </div>
</template>

<script>
import PersonsTable from '@/components/PersonsTable.vue';

export default {
  components: {
    PersonsTable
  }
};
</script>

