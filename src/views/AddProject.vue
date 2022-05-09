<template>
  <form @submit.prevent="handleSubmit">
    <label>title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea required v-model="details"></textarea>
    <button type="submit">Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      details: '',
    };
  },
  methods: {
    handleSubmit() {
      let projectData = {
        title: this.title,
        details: this.details,
        complete: false,
      };
      console.log(projectData);
      this.fetchData(projectData);
    },

    async fetchData(project) {
      try {
        await fetch('http://localhost:3000/projects', {
          method: 'POST',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(project),
        });
        this.$router.push({ name: 'home' });
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style lang="scss">
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
  }
  button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
}
</style>
