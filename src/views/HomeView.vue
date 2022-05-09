<template>
  <div class="home">
    <div v-if="projects.length">
      <NavFilter @update="current = $event" :current="current" />
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          v-bind="project"
          @delete="handleDelete"
          @complete="handleUpdate"
        />
      </div>
    </div>
    <div v-else>...Loading</div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '../components/SingleProject.vue';
import NavFilter from '../components/NavFilter.vue';

export default {
  name: 'Home',
  components: { SingleProject, NavFilter },
  data() {
    return {
      projects: [],
      current: 'all',
    };
  },
  mounted() {
    this.handleData();
  },

  methods: {
    async handleData() {
      const response = await fetch('http://localhost:3000/projects');
      const data = await response.json();
      this.projects = data;
    },
    handleDelete(projectId) {
      this.projects = this.projects.filter(
        (project) => project.id !== projectId
      );
    },
    handleUpdate(projectId) {
      let item = this.projects.find((project) => project.id === projectId);
      console.log(item);
      item.complete = !item.complete;
    },
  },
  computed: {
    filteredProjects() {
      console.log(this.projects);

      if (this.current === 'completed') {
        return this.projects.filter((project) => project.complete);
      }
      if (this.current === 'ongoing') {
        return this.projects.filter((project) => {
          return !project.complete;
        });
      }
      return this.projects;
    },
  },
};
</script>
