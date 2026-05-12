<script setup lang="ts">
import Loading from '@/components/Loading.vue'
import type { MovieModel } from '@/models/movie.model'
import axios from 'axios'
import { computed, onMounted, ref } from 'vue'

type MovieFilter = 'recommended' | 'now' | 'soon'

const movies = ref<MovieModel[]>([])
const isLoading = ref(true)
const errorMessage = ref<string | null>(null)
const activeFilter = ref<MovieFilter>('recommended')

const filters: { label: string; value: MovieFilter }[] = [
  {
    label: 'Recommended',
    value: 'recommended'
  },
  {
    label: 'Now In Cinema',
    value: 'now'
  },
  {
    label: 'Coming Soon',
    value: 'soon'
  }
]

const filteredMovies = computed(() => {
  const today = new Date()

  if (activeFilter.value === 'recommended') {
    return movies.value
  }

  if (activeFilter.value === 'now') {
    return movies.value.filter(movie => {
      return movie.active && new Date(movie.startDate) <= today
    })
  }

  return movies.value.filter(movie => {
    return new Date(movie.startDate) > today
  })
})

function getGenres(movie: MovieModel): string[] {
  return movie.movieGenres?.map(item => item.genre.name) ?? []
}

function formatRuntime(minutes: number): string {
  const hours = Math.floor(minutes / 60)
  const remainingMinutes = minutes % 60

  if (hours === 0) {
    return `${remainingMinutes} min`
  }

  return `${hours}h ${remainingMinutes}min`
}

async function fetchMovies() {
  try {
    isLoading.value = true
    errorMessage.value = null

    const response = await axios.get<MovieModel[]>(
      'https://movie.pequla.com/api/movie'
    )

    movies.value = response.data
  } catch (error) {
    errorMessage.value = 'Failed to load movies.'
    console.error(error)
  } finally {
    isLoading.value = false
  }
}

onMounted(fetchMovies)
</script>

<template>
  <section class="movies-page">
    <div class="movies-hero text-center mb-5">
      <span class="hero-badge">Cinema</span>

      <h1 class="display-5 fw-bold mb-3">
        Explore Movies
      </h1>

      <p class="hero-description mb-0">
        Discover movies currently playing and coming soon to cinemas.
      </p>
    </div>

    <ul class="nav nav-pills justify-content-center gap-2 mb-5">
      <li
        v-for="filter in filters"
        :key="filter.value"
        class="nav-item"
      >
        <button
          class="nav-link filter-link"
          :class="{ active: activeFilter === filter.value }"
          type="button"
          @click="activeFilter = filter.value"
        >
          {{ filter.label }}
        </button>
      </li>
    </ul>

    <Loading v-if="isLoading" />

    <div
      v-else-if="errorMessage"
      class="alert alert-danger text-center"
      role="alert"
    >
      {{ errorMessage }}
    </div>

    <div
      v-else-if="filteredMovies.length > 0"
      class="row g-4"
    >
      <div
        v-for="movie in filteredMovies"
        :key="movie.movieId"
        class="col-12 col-sm-6 col-md-4 col-lg-3"
      >
        <div class="card movie-card h-100">
          <div class="poster-wrapper">
            <img
              :src="movie.poster"
              class="card-img-top movie-poster"
              :alt="movie.title"
            />

            <div class="poster-overlay">
              <RouterLink
                :to="`/details/${movie.movieId}`"
                class="btn btn-light btn-sm rounded-pill px-3"
              >
                View Details
              </RouterLink>
            </div>
          </div>

          <div class="card-body d-flex flex-column">
            <div class="d-flex flex-wrap gap-1 mb-3">
              <span
                v-for="genre in getGenres(movie)"
                :key="genre"
                class="badge rounded-pill genre-badge"
              >
                {{ genre }}
              </span>
            </div>

            <h5 class="card-title fw-bold mb-2">
              {{ movie.title }}
            </h5>

            <p class="movie-original-title mb-3">
              {{ movie.originalTitle }}
            </p>

            <div class="movie-meta mt-auto">
              <span>{{ formatRuntime(movie.runTime) }}</span>
              <span>•</span>
              <span>{{ movie.director.name }}</span>
            </div>

            <div class="movie-actions mt-4">
              <RouterLink
                :to="`/details/${movie.movieId}`"
                class="btn details-btn w-100"
              >
                Details
              </RouterLink>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div
      v-else
      class="empty-state text-center"
    >
      <div class="empty-icon mb-3">🎬</div>

      <h4 class="fw-bold">
        No movies found
      </h4>

      <p class="empty-description mb-0">
        There are no movies available for this category.
      </p>
    </div>
  </section>
</template>

<style scoped>
.movies-page {
  padding-bottom: 3rem;
}

.movies-hero {
  padding: 2rem 1rem;
  border-radius: 28px;
  background:
    linear-gradient(135deg, rgba(220, 53, 69, 0.2), rgba(13, 110, 253, 0.08)),
    #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

.hero-badge {
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

.hero-description {
  color: rgba(255, 255, 255, 0.65);
}

.filter-link {
  border-radius: 999px;
  color: rgba(255, 255, 255, 0.75);
  font-weight: 600;
  padding: 0.65rem 1.2rem;
  background: #1f2937;
  border: 1px solid rgba(255, 255, 255, 0.08);
  transition: all 0.2s ease;
}

.filter-link:hover {
  color: #ffffff;
  background: rgba(220, 53, 69, 0.18);
  border-color: rgba(220, 53, 69, 0.45);
}

.filter-link.active {
  background: #dc3545;
  color: #ffffff;
  border-color: #dc3545;
  box-shadow: 0 8px 22px rgba(220, 53, 69, 0.35);
}

.movie-card {
  border-radius: 22px;
  overflow: hidden;
  background: #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 14px 30px rgba(0, 0, 0, 0.28);
  transition:
    transform 0.25s ease,
    box-shadow 0.25s ease,
    border-color 0.25s ease;
}

.movie-card:hover {
  transform: translateY(-8px);
  border-color: rgba(220, 53, 69, 0.45);
  box-shadow: 0 22px 45px rgba(0, 0, 0, 0.45);
}

.poster-wrapper {
  position: relative;
  overflow: hidden;
  background: #0b1120;
}

.movie-poster {
  height: 360px;
  object-fit: cover;
  transition: transform 0.35s ease;
}

.movie-card:hover .movie-poster {
  transform: scale(1.06);
}

.poster-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(3, 7, 18, 0.65);
  opacity: 0;
  transition: opacity 0.25s ease;
}

.movie-card:hover .poster-overlay {
  opacity: 1;
}

.card-body {
  background: #111827;
}

.genre-badge {
  background: rgba(220, 53, 69, 0.16);
  color: #ffb3bd;
  border: 1px solid rgba(220, 53, 69, 0.28);
  font-weight: 600;
}

.card-title {
  min-height: 48px;
  line-height: 1.25;
  color: #ffffff;
}

.movie-original-title {
  font-size: 0.9rem;
  line-height: 1.35;
  min-height: 38px;
  color: rgba(255, 255, 255, 0.55);
}

.movie-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
  color: rgba(255, 255, 255, 0.6);
  font-size: 0.9rem;
}

.movie-actions {
  padding-top: 0.75rem;
}

.details-btn {
  border-radius: 14px;
  background: linear-gradient(135deg, #dc3545, #a71d2a);
  color: #ffffff;
  font-weight: 700;
  padding: 0.75rem 1rem;
  border: 0;
  box-shadow: 0 12px 24px rgba(220, 53, 69, 0.28);
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease;
}

.details-btn:hover {
  color: #ffffff;
  transform: translateY(-2px);
  box-shadow: 0 16px 32px rgba(220, 53, 69, 0.38);
  background: linear-gradient(135deg, #bb2d3b, #842029);
}

.details-btn:active {
  transform: translateY(0);
}

.empty-state {
  padding: 4rem 1rem;
  border-radius: 24px;
  background: #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

.empty-description {
  color: rgba(255, 255, 255, 0.6);
}

.empty-icon {
  font-size: 3rem;
}

@media (max-width: 575px) {
  .movie-poster {
    height: 430px;
  }

  .movies-hero {
    padding: 1.5rem 1rem;
  }

  .filter-link {
    width: 100%;
  }

  .nav-item {
    width: 100%;
  }
}
</style>