<template>
  <div class="app-shell">
    <header class="app-header">
      <div class="brand">
        <span class="brand-icon">
          <i class="material-icons">dashboard</i>
        </span>
        <span class="brand-name">Project Planner</span>
      </div>
      <nav class="app-nav">
        <router-link to="/" class="nav-link">
          <i class="material-icons">home</i>
          Home
        </router-link>
        <router-link to="/addProject" class="nav-link">
          <i class="material-icons">add_circle</i>
          Add Project
        </router-link>
      </nav>
    </header>
    <main class="app-main">
      <router-view />
    </main>
  </div>
</template>

<style>
.app-shell {
  margin: 0 auto;
  padding: 0 24px 48px;
  width: min(92%, 880px);
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  padding: 28px 0 32px;
  flex-wrap: wrap;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.brand-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 42px;
  height: 42px;
  border-radius: var(--radius-md);
  background: linear-gradient(135deg, var(--color-accent) 0%, #7c3aed 100%);
  color: #fff;
  box-shadow: 0 4px 14px rgba(79, 70, 229, 0.35);
}

.brand-icon .material-icons {
  font-size: 22px;
}

.brand-name {
  font-size: 1.15rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  color: var(--color-text);
}

.app-nav {
  display: flex;
  gap: 8px;
  padding: 6px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 999px;
  box-shadow: var(--shadow-sm);
}

.nav-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 10px 18px;
  border-radius: 999px;
  color: var(--color-text-muted);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  transition: background var(--transition), color var(--transition), box-shadow var(--transition);
}

.nav-link .material-icons {
  font-size: 18px;
}

.nav-link:hover {
  color: var(--color-text);
  background: var(--color-bg);
}

.nav-link.router-link-active {
  color: var(--color-accent);
  background: var(--color-accent-soft);
  box-shadow: var(--shadow-sm);
}

.app-main {
  animation: fadeIn 0.35s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(6px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
