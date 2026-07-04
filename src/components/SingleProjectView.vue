<template>
  <article class="single-project" :class="{ completed: project.completed }">
    <div class="single-project-header">
      <div class="project-info">
        <div class="project-title-row">
          <span class="status-badge" :class="project.completed ? 'status-done' : 'status-active'">
            {{ project.completed ? 'Completed' : 'Active' }}
          </span>
          <h2
            class="project-title"
            :class="{ expanded: showDescription }"
            @click="showDescription = !showDescription"
          >
            {{ project.name }}
            <i class="material-icons expand-icon">{{ showDescription ? 'expand_less' : 'expand_more' }}</i>
          </h2>
        </div>
        <transition name="slide">
          <p v-if="showDescription" class="project-description">{{ project.description }}</p>
        </transition>
      </div>
      <div class="project-actions">
        <button
          type="button"
          class="action-btn"
          title="Delete"
          @click="deleteProject(project.id)"
        >
          <i class="material-icons">delete</i>
        </button>
        <router-link :to="`/projects/${project.id}`">
          <button type="button" class="action-btn" title="Edit">
            <i class="material-icons">edit</i>
          </button>
        </router-link>
        <button
          type="button"
          class="action-btn"
          :class="{ 'action-btn-done': project.completed }"
          title="Mark complete"
          @click="toggleCompleted(project.id)"
        >
          <i class="material-icons">{{ project.completed ? 'check_circle' : 'done' }}</i>
        </button>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  props: ['project'],
  methods: {
    deleteProject(id) {
      fetch(`http://localhost:3000/projects/${id}`, {
        method: 'DELETE'
      })
      .then(data => {
        this.$emit('deleteProject', id);
      }).catch(error => {
        console.error('Error deleting project:', error);
      })
    },
    toggleCompleted(id) {
      fetch(`http://localhost:3000/projects/${id}`, {
        method: 'PATCH',
        body: JSON.stringify({ completed: !this.project.completed })
      })
      .then(data => {
        this.$emit('toggleCompleted', id);
      }).catch(error => {
        console.error('Error toggling completed:', error);
      })
    }
  },
  data() {
    return {
      showDescription: false
    }
  }
}
</script>

<style scoped>
.single-project {
  padding: 20px 22px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  transition: box-shadow var(--transition), border-color var(--transition), transform var(--transition);
}

.single-project:hover {
  border-color: var(--color-border-strong);
  box-shadow: var(--shadow-md);
  transform: translateY(-1px);
}

.single-project-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}

.project-info {
  flex: 1;
  min-width: 0;
}

.project-title-row {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.status-badge {
  align-self: flex-start;
  padding: 3px 10px;
  border-radius: 999px;
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.03em;
  text-transform: uppercase;
}

.status-active {
  background: var(--color-accent-soft);
  color: var(--color-accent);
}

.status-done {
  background: var(--color-success-soft);
  color: var(--color-success);
}

.project-title {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  margin: 0;
  font-size: 1.05rem;
  font-weight: 600;
  letter-spacing: -0.01em;
  cursor: pointer;
  word-break: break-word;
  transition: color var(--transition);
}

.project-title:hover {
  color: var(--color-accent);
}

.expand-icon {
  font-size: 20px !important;
  color: var(--color-text-faint);
  transition: transform var(--transition), color var(--transition);
  flex-shrink: 0;
}

.project-title.expanded .expand-icon {
  color: var(--color-accent);
}

.project-description {
  margin: 12px 0 0;
  padding: 14px 16px;
  color: var(--color-text-muted);
  line-height: 1.6;
  word-break: break-word;
  background: var(--color-bg);
  border-radius: var(--radius-sm);
  border-left: 3px solid var(--color-accent);
}

.project-actions {
  display: flex;
  align-items: center;
  flex-shrink: 0;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-surface);
  overflow: hidden;
}

.action-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  padding: 0;
  border: none;
  border-right: 1px solid var(--color-border);
  background: transparent;
  color: var(--color-text-muted);
  cursor: pointer;
  transition: background var(--transition), color var(--transition);
}

.action-btn:last-child {
  border-right: none;
}

.action-btn .material-icons {
  font-size: 20px;
}

.action-btn:hover {
  background: var(--color-bg);
  color: var(--color-text);
}

.action-btn:first-child:hover {
  background: var(--color-danger-soft);
  color: var(--color-danger);
}

.action-btn-done {
  color: var(--color-success);
}

.action-btn-done:hover {
  background: var(--color-success-soft);
  color: var(--color-success);
}

.completed {
  background: linear-gradient(to right, var(--color-success-soft) 0%, var(--color-surface) 12%);
}

.completed .project-title {
  text-decoration: line-through;
  color: var(--color-text-faint);
}

.completed .project-title:hover {
  color: var(--color-text-muted);
}

.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>
