<template>
  <div>
    <h1>Person Form</h1>
    <form @submit.prevent="submitForm">
      <div>
        <label for="full_name">Full Name</label>
        <input type="text" v-model="form.full_name" id="full_name" required />
      </div>
      <div>
        <label for="email">Email</label>
        <input type="email" v-model="form.email" id="email" required />
      </div>
      <div>
        <label for="job_title">Job Title</label>
        <input type="text" v-model="form.job_title" id="job_title" required />
      </div>
      <button type="submit">Submit</button>
    </form>
    <p>
    <nuxt-link to="/">Go to Home Page</nuxt-link>
    </p>
    <p>
    <nuxt-link to="/about">Go to About Page</nuxt-link>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        full_name: '',
        email: '',
        job_title: ''
      }
    };
  },
  methods: {
    async submitForm() {
      try {
        const response = await fetch('http://localhost:8000/persons/', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.form)
        });

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const data = await response.json();
        console.log('Response:', data);
        this.$router.push('/');
      } catch (error) {
        console.error('Error submitting form:', error);
        alert('There was an error submitting the form.');
      }
    }
  }
};
</script>

<style scoped>
form {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}
div {
  margin-bottom: 1em;
}
label {
  margin-bottom: 0.5em;
  font-weight: bold;
}
input {
  padding: 0.5em;
  font-size: 1em;
}
button {
  padding: 0.5em;
  font-size: 1em;
  cursor: pointer;
}
</style>

