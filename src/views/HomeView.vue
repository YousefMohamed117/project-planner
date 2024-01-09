<template>
  <div class="home">
    <FilterNav @updateFilter="updateFilter" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";

export default {
  name: "HomeView",
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data));
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((i) => {
        return i.id != id;
      });
    },
    handleComplete(id) {
      let x = this.projects.find((i) => {
        return i.id == id;
      });
      x.completed = !x.completed;
    },
    updateFilter(by) {
      this.current = by;
    },
  },
  computed: {
    filteredProjects() {
      if (this.current == "completed") {
        return this.projects.filter((i) => i.completed);
      } else if (this.current == "ongoing") {
        return this.projects.filter((i) => !i.completed);
      }return this.projects
    },
  },
};
</script>
