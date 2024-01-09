<template>
  <div class="project" :class="{ completed: project.completed }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id }}">
          <span class="material-icons">edit</span>
        </router-link>
        <span class="material-icons" @click="deleteProject">delete</span>
        <span class="material-icons tick" @click="finish">done</span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: `http://localhost:3000/projects/${this.project.id}`,
    };
  },
  methods: {
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err));
    },
    finish() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ completed: !this.project.completed }),
      })
        .then(() => this.$emit("complete", this.project.id))
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.project {
  background-color: white;
  padding: 15px;
  border-radius: 6px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  margin: 20px auto;
  text-align: left;
  position: relative;
  border-left: 5px solid #e90074;
}
.project.completed {
  border-left: 5px solid #00ce89;
}
.project.completed .tick {
  color: #00ce89;
}
.actions h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.icons span {
  margin-left: 10px;
  cursor: pointer;
  font-size: 24px;
  color: #bbb;
  transition: 0.3s;
}
.icons span:hover {
  color: #777;
}
</style>
