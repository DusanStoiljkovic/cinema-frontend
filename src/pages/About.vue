<template>
  <section class="about-page">
    <div class="about-hero text-center">
      <span class="about-badge">About Cinema</span>

      <h1 class="display-5 fw-bold mb-3">
        Your movie companion
      </h1>

      <p class="about-description mx-auto mb-0">
        Cinema is a modern web application for browsing movies, exploring movie
        details, checking genres, directors, actors and discovering what is
        currently available in cinemas.
      </p>
    </div>

    <div class="row g-4 mt-4">
      <div class="col-12 col-md-4">
        <div class="about-card h-100">
          <div class="about-icon">🎬</div>

          <h5>Movies</h5>

          <p>
            Browse available movies with posters, titles, genres and useful
            movie information.
          </p>
        </div>
      </div>

      <div class="col-12 col-md-4">
        <div class="about-card h-100">
          <div class="about-icon">🍿</div>

          <h5>Cinema Experience</h5>

          <p>
            Discover movies that are now playing or coming soon in a clean and
            responsive interface.
          </p>
        </div>
      </div>

      <div class="col-12 col-md-4">
        <div class="about-card h-100">
          <div class="about-icon">⭐</div>

          <h5>Details</h5>

          <p>
            Open every movie page to see description, runtime, director, actors
            and additional movie metadata.
          </p>
        </div>
      </div>
    </div>

    <div class="project-card mt-4">
      <div class="row align-items-center g-4">
        <div class="col-12 col-lg-8">
          <h3 class="fw-bold mb-3">
            Built with Vue 3 and Bootstrap 5.3
          </h3>

          <p class="mb-0">
            This project uses Vue Router, TypeScript, Axios and Bootstrap dark
            theme to create a simple but polished cinema application.
          </p>
        </div>

        <div class="col-12 col-lg-4 text-lg-end">
          <RouterLink to="/" class="btn about-btn">
            Explore movies
          </RouterLink>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about-page {
  padding: 2rem 0 4rem;
}

.about-hero {
  padding: 3rem 1.5rem;
  border-radius: 28px;
  background:
    linear-gradient(135deg, rgba(220, 53, 69, 0.2), rgba(13, 110, 253, 0.08)),
    #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 22px 55px rgba(0, 0, 0, 0.35);
}

.about-badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: #dc3545;
  color: #ffffff;
  font-weight: 700;
  font-size: 0.85rem;
  padding: 0.45rem 1rem;
  border-radius: 999px;
  margin-bottom: 1rem;
}

.about-description {
  max-width: 760px;
  color: rgba(255, 255, 255, 0.68);
  font-size: 1.05rem;
  line-height: 1.7;
}

.about-card,
.project-card {
  border-radius: 24px;
  background: #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 16px 35px rgba(0, 0, 0, 0.28);
}

.about-card {
  padding: 1.5rem;
  transition:
    transform 0.25s ease,
    border-color 0.25s ease,
    box-shadow 0.25s ease;
}

.about-card:hover {
  transform: translateY(-6px);
  border-color: rgba(220, 53, 69, 0.45);
  box-shadow: 0 22px 45px rgba(0, 0, 0, 0.42);
}

.about-icon {
  width: 52px;
  height: 52px;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  border-radius: 16px;
  background: rgba(220, 53, 69, 0.15);
  border: 1px solid rgba(220, 53, 69, 0.28);
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.about-card h5 {
  color: #ffffff;
  font-weight: 800;
  margin-bottom: 0.75rem;
}

.about-card p,
.project-card p {
  color: rgba(255, 255, 255, 0.62);
  line-height: 1.7;
}

.project-card {
  padding: 2rem;
}

.about-btn {
  border-radius: 14px;
  background: linear-gradient(135deg, #dc3545, #a71d2a);
  color: #ffffff;
  font-weight: 700;
  padding: 0.8rem 1.3rem;
  border: 0;
  box-shadow: 0 12px 24px rgba(220, 53, 69, 0.25);
}

.about-btn:hover {
  color: #ffffff;
  background: linear-gradient(135deg, #bb2d3b, #842029);
  box-shadow: 0 16px 32px rgba(220, 53, 69, 0.35);
}

@media (max-width: 575px) {
  .about-page {
    padding-top: 1rem;
  }

  .about-hero,
  .about-card,
  .project-card {
    border-radius: 22px;
  }

  .about-hero {
    padding: 2rem 1.2rem;
  }

  .about-btn {
    width: 100%;
  }
}
</style>