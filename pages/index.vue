<template>
  <div>
    <p>
    <nuxt-link to="/about">Go to About Page</nuxt-link>
    </p>
    <p>
    <nuxt-link to="/personForm">Go to Add a Person</nuxt-link>
    </p>
  </div>
  <div>
    <PersonsTable />
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

