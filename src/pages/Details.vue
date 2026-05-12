<script lang="ts" setup>
import type { MovieModel } from '@/models/movie.model'
import { useRoute } from 'vue-router'
import { computed, ref, watch } from 'vue'
import axios from 'axios'
import Loading from '@/components/Loading.vue'

const route = useRoute()

const movie = ref<MovieModel | null>(null)
const isLoading = ref(true)
const errorMessage = ref<string | null>(null)

const movieId = computed(() => {
  const id = route.params.id
  return Array.isArray(id) ? id[0] : id
})

const descriptionParagraphs = computed(() => {
  if (!movie.value?.description) return []

  return movie.value.description
    .split('\n')
    .map(paragraph => paragraph.trim())
    .filter(Boolean)
})

const genreNames = computed(() => {
  return movie.value?.movieGenres.map(item => item.genre.name) ?? []
})

const actorNames = computed(() => {
  return movie.value?.movieActors.map(item => item.actor.name) ?? []
})

function formatDate(date: string): string {
  return new Intl.DateTimeFormat('sr-RS', {
    day: '2-digit',
    month: 'long',
    year: 'numeric'
  }).format(new Date(date))
}

function formatRuntime(minutes: number): string {
  const hours = Math.floor(minutes / 60)
  const remainingMinutes = minutes % 60

  if (hours === 0) {
    return `${remainingMinutes} min`
  }

  return `${hours}h ${remainingMinutes}min`
}

async function fetchMovie() {
  try {
    isLoading.value = true
    errorMessage.value = null
    movie.value = null

    const response = await axios.get<MovieModel>(
      `https://movie.pequla.com/api/movie/${movieId.value}`
    )

    movie.value = response.data
  } catch (error) {
    errorMessage.value = 'Failed to load movie details.'
    console.error(error)
  } finally {
    isLoading.value = false
  }
}

watch(movieId, fetchMovie, { immediate: true })
</script>

<template>
  <section class="details-page">
    <Loading v-if="isLoading" />

    <div
      v-else-if="errorMessage"
      class="alert alert-danger text-center"
      role="alert"
    >
      {{ errorMessage }}
    </div>

    <div v-else-if="movie">
      <div class="details-card">
        <div class="row g-0">
          <div class="col-12 col-lg-4">
            <div class="poster-box">
              <img
                :src="movie.poster"
                :alt="movie.title"
                class="movie-poster"
              />

              <div class="poster-gradient"></div>
            </div>
          </div>

          <div class="col-12 col-lg-8">
            <div class="details-content">
              <div class="d-flex flex-wrap gap-2 mb-3">
                <span
                  v-for="genre in genreNames"
                  :key="genre"
                  class="badge genre-badge"
                >
                  {{ genre }}
                </span>

                <span
                  v-if="movie.active"
                  class="badge status-badge active-status"
                >
                  Active
                </span>

                <span
                  v-else
                  class="badge status-badge inactive-status"
                >
                  Not active
                </span>
              </div>

              <h1 class="movie-title">
                {{ movie.title }}
              </h1>

              <h5 class="movie-original-title">
                {{ movie.originalTitle }}
              </h5>

              <p class="movie-short-description">
                {{ movie.shortDescription }}
              </p>

              <div class="row g-3 my-4">
                <div class="col-12 col-md-4">
                  <div class="info-box">
                    <span class="info-label">Director</span>
                    <strong>{{ movie.director.name }}</strong>
                  </div>
                </div>

                <div class="col-12 col-md-4">
                  <div class="info-box">
                    <span class="info-label">Runtime</span>
                    <strong>{{ formatRuntime(movie.runTime) }}</strong>
                  </div>
                </div>

                <div class="col-12 col-md-4">
                  <div class="info-box">
                    <span class="info-label">Start date</span>
                    <strong>{{ formatDate(movie.startDate) }}</strong>
                  </div>
                </div>
              </div>

              <div class="section-block">
                <h5 class="section-title">Actors</h5>

                <div class="d-flex flex-wrap gap-2">
                  <span
                    v-for="actor in actorNames"
                    :key="actor"
                    class="actor-chip"
                  >
                    {{ actor }}
                  </span>
                </div>
              </div>

              <div class="section-block">
                <h5 class="section-title">Description</h5>

                <p
                  v-for="(paragraph, index) in descriptionParagraphs"
                  :key="index"
                  class="description-text"
                >
                  {{ paragraph }}
                </p>
              </div>

              <div class="d-flex flex-wrap gap-2 pt-2">
                <RouterLink
                  to="/"
                  class="btn btn-back"
                >
                  <i class="fa-solid fa-arrow-left"></i>
                  Back to movies
                </RouterLink>

                <RouterLink
                  :to="`/booking/${movie.movieId}`"
                  class="btn btn-book"
                >
                  <i class="fa-solid fa-ticket"></i>
                  Book tickets
                </RouterLink>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="movie-info-card mt-4">
        <div class="card-body p-4">
          <h5 class="section-title mb-3">
            <i class="fa-solid fa-circle-info"></i>
            Movie info
        </h5>

          <div class="table-responsive">
            <table class="table movie-info-table align-middle mb-0">
              <tbody>
                <tr>
                  <th scope="row">Movie ID</th>
                  <td>{{ movie.movieId }}</td>
                </tr>

                <tr>
                  <th scope="row">Internal ID</th>
                  <td>{{ movie.internalId }}</td>
                </tr>

                <tr>
                  <th scope="row">Corporate ID</th>
                  <td>{{ movie.corporateId }}</td>
                </tr>

                <tr>
                  <th scope="row">Short URL</th>
                  <td>{{ movie.shortUrl }}</td>
                </tr>

                <tr>
                  <th scope="row">Created at</th>
                  <td>{{ formatDate(movie.createdAt) }}</td>
                </tr>

                <tr>
                  <th scope="row">Updated at</th>
                  <td>
                    {{ movie.updatedAt ? formatDate(movie.updatedAt) : 'Not updated' }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.details-page {
  padding: 2rem 0 4rem;
}

.details-card,
.movie-info-card {
  overflow: hidden;
  border-radius: 28px;
  background:
    linear-gradient(135deg, rgba(220, 53, 69, 0.08), rgba(13, 110, 253, 0.04)),
    #111827;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 22px 55px rgba(0, 0, 0, 0.35);
}

.poster-box {
  position: relative;
  height: 100%;
  min-height: 520px;
  background: #0b1120;
  overflow: hidden;
}

.movie-poster {
  width: 100%;
  height: 100%;
  min-height: 520px;
  object-fit: cover;
  display: block;
}

.poster-gradient {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(to top, rgba(3, 7, 18, 0.85), transparent 45%),
    linear-gradient(to right, transparent, rgba(17, 24, 39, 0.25));
}

.details-content {
  padding: 2.5rem;
}

.genre-badge {
  background: rgba(220, 53, 69, 0.16);
  color: #ffb3bd;
  border: 1px solid rgba(220, 53, 69, 0.3);
  font-weight: 700;
  border-radius: 999px;
  padding: 0.55rem 0.8rem;
}

.status-badge {
  border-radius: 999px;
  padding: 0.55rem 0.8rem;
  font-weight: 700;
}

.active-status {
  color: #b7f7cf;
  background: rgba(25, 135, 84, 0.18);
  border: 1px solid rgba(25, 135, 84, 0.35);
}

.inactive-status {
  color: rgba(255, 255, 255, 0.65);
  background: rgba(108, 117, 125, 0.18);
  border: 1px solid rgba(108, 117, 125, 0.35);
}

.movie-title {
  color: #ffffff;
  font-weight: 800;
  line-height: 1.1;
  margin-bottom: 0.75rem;
}

.movie-original-title {
  color: rgba(255, 255, 255, 0.55);
  margin-bottom: 1.5rem;
}

.movie-short-description {
  color: rgba(255, 255, 255, 0.78);
  font-size: 1.1rem;
  line-height: 1.7;
  margin-bottom: 0;
}

.info-box {
  height: 100%;
  padding: 1rem;
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.045);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

.info-label {
  display: block;
  color: rgba(255, 255, 255, 0.48);
  font-size: 0.85rem;
  margin-bottom: 0.25rem;
}

.info-box strong {
  color: #ffffff;
}

.section-block {
  margin-top: 2rem;
}

.section-title {
  color: #ffffff;
  font-weight: 800;
}

.actor-chip {
  display: inline-flex;
  align-items: center;
  border-radius: 999px;
  padding: 0.55rem 0.85rem;
  background: rgba(255, 255, 255, 0.06);
  color: rgba(255, 255, 255, 0.78);
  border: 1px solid rgba(255, 255, 255, 0.08);
  font-weight: 600;
}

.description-text {
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.8;
  margin-bottom: 1rem;
}

.btn-back,
.btn-book {
  border-radius: 14px;
  font-weight: 700;
  padding: 0.75rem 1.2rem;
}

.btn-back {
  color: rgba(255, 255, 255, 0.82);
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.12);
}

.btn-back:hover {
  color: #ffffff;
  background: rgba(255, 255, 255, 0.1);
}

.btn-book {
  color: #ffffff;
  background: linear-gradient(135deg, #dc3545, #a71d2a);
  border: 0;
  box-shadow: 0 12px 24px rgba(220, 53, 69, 0.25);
}

.btn-book:hover {
  color: #ffffff;
  background: linear-gradient(135deg, #bb2d3b, #842029);
  box-shadow: 0 16px 32px rgba(220, 53, 69, 0.35);
}

.movie-info-table {
  --bs-table-bg: transparent;
  --bs-table-color: rgba(255, 255, 255, 0.75);
  --bs-table-border-color: rgba(255, 255, 255, 0.08);
}

.movie-info-table th {
  width: 180px;
  color: rgba(255, 255, 255, 0.5);
  font-weight: 700;
}

.movie-info-table td {
  color: rgba(255, 255, 255, 0.82);
}

@media (max-width: 991px) {
  .poster-box,
  .movie-poster {
    min-height: 420px;
  }

  .details-content {
    padding: 2rem;
  }
}

@media (max-width: 575px) {
  .details-page {
    padding-top: 1rem;
  }

  .details-card,
  .movie-info-card {
    border-radius: 22px;
  }

  .poster-box,
  .movie-poster {
    min-height: 460px;
  }

  .details-content {
    padding: 1.4rem;
  }

  .movie-title {
    font-size: 2rem;
  }

  .btn-back,
  .btn-book {
    width: 100%;
  }
}
</style>