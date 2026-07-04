<template>
  <div class="add-project">
    <header class="page-header">
      <p class="page-eyebrow">New entry</p>
      <h1>Create a Project</h1>
      <p class="page-subtitle">Fill in the details below to add a new project to your list.</p>
    </header>

    <div class="add-project-card">
      <form @submit.prevent="addProject">
        <div class="form-group">
          <label for="project-title">Project Title</label>
          <div class="input-wrapper">
            <i class="material-icons input-icon">title</i>
            <input id="project-title" type="text" placeholder="e.g. Website redesign" v-model="name"/>
          </div>
        </div>
        <div class="form-group">
          <label for="project-description">Project Description</label>
          <div class="input-wrapper">
            <i class="material-icons input-icon textarea-icon">notes</i>
            <textarea
              id="project-description"
              rows="4"
              placeholder="Describe the goals and scope of this project..."
              v-model="description"
            ></textarea>
          </div>
        </div>
        <button type="submit">
          <i class="material-icons">add</i>
          Add Project
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {

  data() {
    return {
      name: '',
      description: ''
    }
  },
  methods: {
    addProject() {
      fetch('http://localhost:3000/projects', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: this.name,
          description: this.description,
          completed: false
        })
      })
      .then(() => {
        this.$router.push('/')
      })
      .catch(error => {
        console.error('Error:', error)
      })  
    }
  }
}
</script>

<style>
.add-project {
  max-width: 520px;
  margin: 0 auto;
}

.page-header {
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

.add-project-card {
  padding: 28px 28px 32px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-lg);
}

.add-project-card form {
  display: flex;
  flex-direction: column;
  gap: 22px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text);
}

.input-wrapper {
  position: relative;
}

.input-icon {
  position: absolute;
  left: 14px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--color-text-faint);
  pointer-events: none;
}

.textarea-icon {
  top: 16px;
  transform: none;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 13px 14px 13px 44px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  font-size: 0.95rem;
  font-family: inherit;
  color: var(--color-text);
  background: var(--color-bg);
  box-sizing: border-box;
  transition: border-color var(--transition), background var(--transition), box-shadow var(--transition);
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: var(--color-text-faint);
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--color-accent);
  background: var(--color-surface);
  box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.12);
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
  line-height: 1.6;
}

.add-project-card button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 4px;
  padding: 14px 24px;
  border: none;
  border-radius: var(--radius-md);
  background: linear-gradient(135deg, var(--color-accent) 0%, #7c3aed 100%);
  color: #fff;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(79, 70, 229, 0.35);
  transition: transform var(--transition), box-shadow var(--transition), filter var(--transition);
}

.add-project-card button .material-icons {
  font-size: 20px;
}

.add-project-card button:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(79, 70, 229, 0.4);
  filter: brightness(1.05);
}

.add-project-card button:active {
  transform: translateY(0);
}
</style>
