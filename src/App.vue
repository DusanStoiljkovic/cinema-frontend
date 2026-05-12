<script setup lang="ts">
const year = new Date().getFullYear()

function handleSearch() {
  // Kasnije ovde možeš dodati search logiku
}
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-dark app-navbar sticky-top">
    <div class="container">
      <RouterLink class="navbar-brand d-flex align-items-center gap-2" to="/">
        <span class="brand-icon">🎬</span>
        <span class="brand-text">Cinema</span>
      </RouterLink>

      <button
        class="navbar-toggler shadow-none border-0"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#mainNavbar"
        aria-controls="mainNavbar"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="mainNavbar">
        <ul class="navbar-nav mx-auto mb-3 mb-lg-0 gap-lg-2">
          <li class="nav-item">
            <RouterLink
              class="nav-link"
              exact-active-class="nav-link-active"
              to="/"
            >
              Movies
            </RouterLink>
          </li>

          <li class="nav-item">
            <RouterLink
              class="nav-link"
              active-class="nav-link-active"
              to="/recommended"
            >
              <i class="fa-solid fa-film"></i>
              Cinemas
            </RouterLink>
          </li>

          <li class="nav-item">
            <RouterLink
              class="nav-link"
              active-class="nav-link-active"
              to="/about"
            >
              <i class="fa-solid fa-circle-info"></i>
              About
            </RouterLink>
          </li>

          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="#"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              <i class="fa-solid fa-lock"></i>
              Auth
            </a>

            <ul class="dropdown-menu dropdown-menu-dark app-dropdown">
              <li>
                <RouterLink class="dropdown-item" to="/register">
                  Register
                </RouterLink>
              </li>

              <li>
                <RouterLink class="dropdown-item" to="/login">
                  Login
                </RouterLink>
              </li>
            </ul>
          </li>

          <li class="nav-item">
            <span class="nav-link disabled-link">
              <i class="fa-solid fa-receipt"></i>
              Coupons
            </span>
          </li>
        </ul>

        <form class="d-flex search-form" role="search" @submit.prevent="handleSearch">
          <input
            class="form-control search-input"
            type="search"
            placeholder="Search movies..."
            aria-label="Search"
          />

          <button class="btn search-btn" type="submit">
            Search
          </button>
        </form>
      </div>
    </div>
  </nav>

  <main class="container py-4">
    <RouterView />
  </main>

  <footer class="app-footer">
    <div class="container d-flex flex-column flex-md-row justify-content-between align-items-center gap-2">
      <span>&copy; {{ year }} Univerzitet Singidunum</span>
      <span class="footer-text">Cinema web application</span>
    </div>
  </footer>
</template>

<style scoped>
.app-navbar {
  background: linear-gradient(135deg, #111827, #1f2937, #0f172a);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  padding: 0.9rem 0;
  z-index: 1000;
}

.navbar-brand {
  font-weight: 800;
  letter-spacing: 0.3px;
  color: #ffffff;
}

.brand-icon {
  width: 42px;
  height: 42px;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  background: rgba(255, 255, 255, 0.12);
  border-radius: 14px;
  font-size: 1.3rem;
}

.brand-text {
  font-size: 1.35rem;
}

.nav-link {
  color: rgba(255, 255, 255, 0.78);
  font-weight: 500;
  padding: 0.6rem 0.95rem;
  border-radius: 999px;
  transition: all 0.2s ease;
}

.nav-link:hover {
  color: #ffffff;
  background: rgba(255, 255, 255, 0.1);
}

.nav-link-active {
  color: #ffffff !important;
  background: #dc3545;
}

.disabled-link {
  color: rgba(255, 255, 255, 0.35) !important;
  cursor: not-allowed;
}

.app-dropdown {
  border: 0;
  border-radius: 16px;
  padding: 0.5rem;
  margin-top: 0.8rem;
  background: #111827;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.35);
}

.dropdown-item {
  border-radius: 10px;
  padding: 0.65rem 0.9rem;
  font-weight: 500;
}

.dropdown-item:hover {
  background: #dc3545;
  color: #ffffff;
}

.search-form {
  background: rgba(255, 255, 255, 0.1);
  padding: 0.35rem;
  border-radius: 999px;
  gap: 0.4rem;
}

.search-input {
  min-width: 220px;
  border: 0;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.95);
  padding-left: 1rem;
}

.search-input:focus {
  box-shadow: none;
}

.search-btn {
  border-radius: 999px;
  background: #dc3545;
  color: #ffffff;
  font-weight: 600;
  padding-inline: 1.2rem;
  border: 0;
}

.search-btn:hover {
  background: #bb2d3b;
  color: #ffffff;
}

.app-footer {
  margin-top: 3rem;
  padding: 3rem 0;
  background: #111827;
  color: rgba(255, 255, 255, 0.75);
}

.footer-text {
  color: rgba(255, 255, 255, 0.45);
}

@media (max-width: 991px) {
  .navbar-collapse {
    margin-top: 1rem;
  }

  .nav-link {
    border-radius: 12px;
  }

  .search-form {
    border-radius: 16px;
    flex-direction: column;
    align-items: stretch;
  }

  .search-input {
    min-width: 100%;
  }

  .search-btn {
    width: 100%;
  }
}
</style>