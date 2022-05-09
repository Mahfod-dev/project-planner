<template>
  <div class="project" :class="{ complete: complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{ title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'editProject', params: { id: id } }"
          ><span class="material-icons"> edit </span></router-link
        >

        <span @click="deleteItems" class="material-icons"> delete </span>
        <span @click="toggleComplete" class="material-icons tick"> done </span>
      </div>
    </div>
    <div v-show="showDetails" class="details">
      <p>{{ details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['title', 'id', 'details', 'complete'],
  emits: ['delete', 'complete'],
  data() {
    return {
      showDetails: false,
      uri: `http://localhost:3000/projects/${this.id}`,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    deleteItems() {
      fetch(`${this.uri}`, { method: 'DELETE' })
        .then(() => {
          this.$emit('delete', this.id);
        })
        .catch((err) => console.log(err.message));
    },
    toggleComplete() {
      fetch(`${this.uri}`, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !this.complete }),
      })
        .then(() => this.$emit('complete', this.id))
        .catch((err) => console.log(err.message));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background-color: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid red;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;

  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
.complete {
  border-left: 4px solid #00ce89;
}
.complete .tick {
  color: #00ce89;
}
</style>
