<template>
  <div class="projects-view">
    <header class="page-header">
      <div>
        <p class="page-eyebrow">Overview</p>
        <h1>Your Projects</h1>
        <p class="page-subtitle">Track progress and manage your work in one place.</p>
      </div>
      <div v-if="projects.length > 0" class="project-count">
        <span class="count-number">{{ projects.length }}</span>
        <span class="count-label">{{ projects.length === 1 ? 'project' : 'projects' }}</span>
      </div>
    </header>

  <FilterNavView @filterProjects="filter = $event"  :filter="filter" />

    <div v-if="projects.length === 0" class="empty-state">
      <div class="empty-icon">
        <i class="material-icons">folder_open</i>
      </div>
      <h2>No projects yet</h2>
      <p>Create your first project to start organizing your work.</p>
    </div>

    <div v-else class="projects-list">
      <SingleProjectView
        v-for="project in filteredProjects"
        :key="project.id"
        :project="project"
        @deleteProject="deleteProject"
        @toggleCompleted="toggleCompleted"
      />
    </div>
  </div>
</template>

<script>
import SingleProjectView from './SingleProjectView.vue';
import FilterNavView from './FilterNavView.vue';
export default {
  computed: {
    filteredProjects() {
      if (this.filter === 'completed') {
        return this.projects.filter(project => project.completed);
      } else if (this.filter === 'ongoing') {
        return this.projects.filter(project => !project.completed);
      }
      return this.projects;
    }
  },
  methods: {
    deleteProject(id) {
      this.projects = this.projects.filter(project => project.id !== id);
    },
    toggleCompleted(id) {
      let findProject = this.projects.find(project => project.id === id);
      findProject.completed = !findProject.completed;
    },
  },
  data() {
    return {
      projects: [],
      filter: 'all'
    }
  },
  components: {
    SingleProjectView,
    FilterNavView
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(response => response.json())
      .then(data => {
        this.projects = data
      }).catch(error => {
        console.error('Error fetching projects:', error)
      })
  }
}
</script>

<style scoped>
.projects-view {
  width: 100%;
}

.page-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 20px;
  margin-bottom: 28px;
}

.page-eyebrow {
  margin: 0 0 6px;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-accent);
}

.page-header h1 {
  margin: 0;
  font-size: 1.75rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  color: var(--color-text);
}

.page-subtitle {
  margin: 8px 0 0;
  font-size: 0.95rem;
  color: var(--color-text-muted);
}

.project-count {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-width: 88px;
  padding: 14px 20px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
}

.count-number {
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1;
  color: var(--color-accent);
}

.count-label {
  margin-top: 4px;
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--color-text-faint);
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 56px 32px;
  text-align: center;
  background: var(--color-surface);
  border: 1px dashed var(--color-border-strong);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-sm);
}

.empty-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 72px;
  height: 72px;
  margin-bottom: 20px;
  border-radius: 50%;
  background: var(--color-accent-soft);
  color: var(--color-accent);
}

.empty-icon .material-icons {
  font-size: 36px;
}

.empty-state h2 {
  margin: 0 0 8px;
  font-size: 1.15rem;
  font-weight: 600;
  color: var(--color-text);
}

.empty-state p {
  margin: 0;
  max-width: 280px;
  font-size: 0.95rem;
  color: var(--color-text-muted);
  line-height: 1.6;
}

.projects-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
</style>
