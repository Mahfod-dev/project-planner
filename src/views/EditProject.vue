<template>
  <h1>Edit project</h1>
  <form @submit.prevent="handleUpdate">
    <label>title:</label>
    <input type="text" v-model="title" required />
    <label>Details: </label>
    <textarea required v-model="details"></textarea>
    <button type="submit">Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: `http://localhost:3000/projects/${this.id}`,
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const res = await fetch(this.uri);
        const { title, details } = await res.json();
        this.title = title;
        this.details = details;
      } catch (error) {
        console.log(error);
      }
    },
    handleUpdate() {
      let projectUpdate = {
        title: this.title,
        details: this.details,
      };
      this.fetchDataUpdate(projectUpdate);
      this.$router.push({ name: 'home' });
    },
    async fetchDataUpdate(projectUp) {
      try {
        const res = await fetch(this.uri, {
          method: 'PATCH',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(projectUp),
        });

        console.log(title, details);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
