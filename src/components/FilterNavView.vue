<template>
  <div class="filter-nav">
    <nav class="filter-nav-pill">
      <button
        type="button"
        class="filter-btn"
        :class="{ active: filter === 'all' }"
        @click="filterProjects('all')"
      >
        <i class="material-icons">view_list</i>
        All
      </button>
      <button
        type="button"
        class="filter-btn"
        :class="{ active: filter === 'ongoing' }"
        @click="filterProjects('ongoing')"
      >
        <i class="material-icons">pending</i>
        Ongoing
      </button>
      <button
        type="button"
        class="filter-btn"
        :class="{ active: filter === 'completed' }"
        @click="filterProjects('completed')"
      >
        <i class="material-icons">check_circle</i>
        Completed
      </button>
    </nav>
  </div>
</template>

<script>
export default {
  props: ['filter'],
  methods: {
    filterProjects(status) {
      this.$emit('filterProjects', status);
    }
  }
}
</script>

<style scoped>
.filter-nav {
  margin-bottom: 24px;
}

.filter-nav-pill {
  display: inline-flex;
  gap: 6px;
  padding: 6px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 999px;
  box-shadow: var(--shadow-sm);
}

.filter-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 10px 18px;
  border: none;
  border-radius: 999px;
  background: transparent;
  color: var(--color-text-muted);
  font-size: 0.9rem;
  font-weight: 500;
  font-family: inherit;
  cursor: pointer;
  transition: background var(--transition), color var(--transition), box-shadow var(--transition);
}

.filter-btn .material-icons {
  font-size: 18px;
}

.filter-btn:hover {
  color: var(--color-text);
  background: var(--color-bg);
}

.filter-btn.active {
  color: var(--color-accent);
  background: var(--color-accent-soft);
  box-shadow: var(--shadow-sm);
}
</style>
