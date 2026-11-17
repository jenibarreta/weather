<template>
  <v-app>
    <v-main class="dashboard-page">

      <!-- Navigation -->
      <v-app-bar flat color="#245B8F" height="76">
        <v-container class="d-flex align-center">
          <div class="d-flex align-center ga-2">
            <v-avatar color="#F4C95D" size="42">
              <v-icon color="#245B8F" size="28">
                mdi-weather-partly-cloudy
              </v-icon>
            </v-avatar>

            <span class="text-h6 font-weight-bold text-white">
              Weather App
            </span>
          </div>

          <v-spacer />

          <v-btn
            to="/dashboard"
            variant="text"
            class="nav-btn active-nav"
            prepend-icon="mdi-view-dashboard"
          >
            Dashboard
          </v-btn>

          <v-btn
            to="/weather"
            variant="text"
            class="nav-btn"
            prepend-icon="mdi-weather-cloudy"
          >
            Weather
          </v-btn>

          <v-menu location="bottom end">
            <template #activator="{ props }">
              <v-btn
                v-bind="props"
                icon
                class="ml-2"
                aria-label="Profile menu"
              >
                <v-avatar size="40" color="#F4C95D">
                  <v-img
                    v-if="user?.picture"
                    :src="user.picture"
                    alt="Profile"
                  />
                  <v-icon v-else color="#245B8F">
                    mdi-account
                  </v-icon>
                </v-avatar>
              </v-btn>
            </template>

            <v-card min-width="210" rounded="lg">
              <v-list density="compact">
                <v-list-item
                  prepend-icon="mdi-account-circle"
                  :title="user?.name || 'My Account'"
                  :subtitle="user?.email || ''"
                />
                <v-divider />
                <v-list-item
                  prepend-icon="mdi-logout"
                  title="Logout"
                  @click="goToLogout"
                />
              </v-list>
            </v-card>
          </v-menu>
        </v-container>
      </v-app-bar>

      <v-container max-width="1100" class="py-8">

        <!-- Welcome -->
        <div class="welcome-section mb-8">
          <div class="d-flex align-center ga-2 mb-2">
            <v-icon color="#245B8F" size="22">
              mdi-view-dashboard-outline
            </v-icon>
            <span class="section-label">
              WEATHER DASHBOARD
            </span>
          </div>

          <h1 class="text-h4 font-weight-bold main-heading">
            Welcome back, {{ firstName }}!
          </h1>

          <p class="text-body-1 readable-text mt-2">
            Your weather updates and forecasts are just one click away.
          </p>
        </div>

        <!-- Profile -->
        <v-card
          class="profile-card pa-5 pa-md-6 mb-8"
          rounded="xl"
          flat
        >
          <v-row align="center">
            <v-col cols="12" md="8">
              <div class="d-flex align-center ga-4">
                <v-avatar
                  size="76"
                  color="#E8F0FA"
                  class="profile-avatar"
                >
                  <v-img
                    v-if="user?.picture"
                    :src="user.picture"
                    alt="Google profile"
                  />
                  <v-icon v-else color="#245B8F" size="40">
                    mdi-account
                  </v-icon>
                </v-avatar>

                <div class="profile-information">
                  <div class="d-flex align-center ga-2 mb-1">
                    <v-icon color="#245B8F" size="17">
                      mdi-google
                    </v-icon>
                    <span class="profile-label">
                      GOOGLE ACCOUNT
                    </span>
                  </div>

                  <h2 class="text-h6 font-weight-bold profile-name">
                    {{ user?.name || 'Google User' }}
                  </h2>

                  <p class="text-body-2 profile-email">
                    {{ user?.email || '' }}
                  </p>
                </div>
              </div>
            </v-col>

            <v-col cols="12" md="4" class="text-md-right">
              <v-chip
                color="#245B8F"
                variant="tonal"
                prepend-icon="mdi-check-circle"
                class="font-weight-medium"
              >
                Account Connected
              </v-chip>
            </v-col>
          </v-row>
        </v-card>

        <!-- Weather Banner -->
        <v-card
          class="weather-banner mb-8"
          rounded="xl"
          flat
        >
          <v-row align="center" no-gutters>
            <v-col cols="12" md="8" class="pa-6 pa-md-8">
              <v-chip
                color="#F4C95D"
                class="banner-chip mb-4"
                prepend-icon="mdi-white-balance-sunny"
              >
                YOUR WEATHER COMPANION
              </v-chip>

              <h2 class="text-h4 font-weight-bold text-white mb-3">
                Explore the Weather
              </h2>

              <p class="banner-description mb-6">
                Discover current weather conditions, temperature,
                humidity, wind speed, and hourly forecasts
                in your selected city.
              </p>

              <v-btn
                to="/weather"
                color="#F4C95D"
                size="large"
                rounded="lg"
                class="text-none font-weight-bold"
                append-icon="mdi-arrow-right"
              >
                Explore Weather
              </v-btn>
            </v-col>

            <v-col
              cols="12"
              md="4"
              class="text-center pa-5 pa-md-2"
            >
              <v-icon
                size="150"
                color="#F4C95D"
                class="weather-illustration"
              >
                mdi-weather-partly-cloudy
              </v-icon>

              <p class="text-white text-body-2 font-weight-medium mt-2">
                Check your local forecast
              </p>
            </v-col>
          </v-row>
        </v-card>

        <!-- Features Heading -->
        <div class="mb-5">
          <div class="d-flex align-center ga-2 mb-2">
            <v-icon color="#245B8F" size="23">
              mdi-information-outline
            </v-icon>

            <h2 class="text-h5 font-weight-bold main-heading">
              Weather Features
            </h2>
          </div>

          <p class="text-body-1 readable-text">
            Useful information to help you understand the weather.
          </p>
        </div>

        <!-- Feature Cards -->
        <v-row>
          <v-col cols="12" sm="6" md="4">
            <v-card class="feature-card pa-5" rounded="xl" flat>
              <div class="d-flex align-center justify-space-between mb-5">
                <v-avatar color="#E8F0FA" size="54">
                  <v-icon color="#245B8F" size="30">
                    mdi-thermometer
                  </v-icon>
                </v-avatar>

                <v-icon color="#A7B8CC" size="25">
                  mdi-chevron-right
                </v-icon>
              </div>

              <h3 class="text-h6 font-weight-bold main-heading">
                Temperature
              </h3>

              <p class="feature-description mt-2">
                Monitor current temperature and understand
                how hot or cold your selected location is.
              </p>

              <v-btn
                to="/weather"
                variant="text"
                color="#245B8F"
                class="mt-3 px-0"
                append-icon="mdi-arrow-right"
              >
                View Details
              </v-btn>
            </v-card>
          </v-col>

          <v-col cols="12" sm="6" md="4">
            <v-card class="feature-card pa-5" rounded="xl" flat>
              <div class="d-flex align-center justify-space-between mb-5">
                <v-avatar color="#FFF4D5" size="54">
                  <v-icon color="#245B8F" size="30">
                    mdi-water-percent
                  </v-icon>
                </v-avatar>

                <v-icon color="#A7B8CC" size="25">
                  mdi-chevron-right
                </v-icon>
              </div>

              <h3 class="text-h6 font-weight-bold main-heading">
                Humidity
              </h3>

              <p class="feature-description mt-2">
                Check moisture levels in the air and
                understand the humidity of your location.
              </p>

              <v-btn
                to="/weather"
                variant="text"
                color="#245B8F"
                class="mt-3 px-0"
                append-icon="mdi-arrow-right"
              >
                View Details
              </v-btn>
            </v-card>
          </v-col>

          <v-col cols="12" sm="6" md="4">
            <v-card class="feature-card pa-5" rounded="xl" flat>
              <div class="d-flex align-center justify-space-between mb-5">
                <v-avatar color="#E8F0FA" size="54">
                  <v-icon color="#245B8F" size="30">
                    mdi-chart-line
                  </v-icon>
                </v-avatar>

                <v-icon color="#A7B8CC" size="25">
                  mdi-chevron-right
                </v-icon>
              </div>

              <h3 class="text-h6 font-weight-bold main-heading">
                Hourly Forecast
              </h3>

              <p class="feature-description mt-2">
                Explore upcoming weather changes through
                an easy-to-read temperature chart.
              </p>

              <v-btn
                to="/weather"
                variant="text"
                color="#245B8F"
                class="mt-3 px-0"
                append-icon="mdi-arrow-right"
              >
                View Details
              </v-btn>
            </v-card>
          </v-col>
        </v-row>

        <!-- Footer -->
        <v-divider class="my-8" />

        <div class="d-flex flex-column flex-sm-row align-center justify-space-between ga-3 pb-4">
          <div class="d-flex align-center ga-2">
            <v-icon color="#245B8F" size="21">
              mdi-weather-cloudy
            </v-icon>
            <span class="footer-text">
              Weather App
            </span>
          </div>

          <p class="footer-text">
            Weather data powered by Open-Meteo
          </p>
        </div>

      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
const user = ref<any>(null)

const firstName = computed(() => {
  return user.value?.name?.split(' ')[0] || 'there'
})

onMounted(() => {
  const savedUser = localStorage.getItem('google_user')

  if (savedUser) {
    try {
      user.value = JSON.parse(savedUser)
    } catch {
      localStorage.removeItem('google_user')
      localStorage.removeItem('google_token')
      navigateTo('/login')
    }
  }
})

const goToLogout = () => {
  navigateTo('/logout')
}
</script>

<style scoped>
.dashboard-page {
  min-height: 100vh;
  background: #F5F8FC;
}

.section-label {
  color: #245B8F;
  font-size: 13px;
  font-weight: 800;
  letter-spacing: 1.2px;
}

.main-heading {
  color: #183B5B;
}

.readable-text {
  color: #52657A;
  line-height: 1.7;
}

.nav-btn {
  color: white;
  text-transform: none;
  font-weight: 600;
  letter-spacing: 0;
}

.active-nav {
  color: #F4C95D;
  background: rgba(255, 255, 255, 0.10);
}

.profile-card,
.feature-card {
  background: #FFFFFF;
  border: 1px solid #E0E8F1;
  box-shadow: 0 4px 16px rgba(36, 91, 143, 0.045);
}

.profile-avatar {
  border: 2px solid #E0EAF5;
}

.profile-label {
  color: #245B8F;
  font-size: 11px;
  font-weight: 800;
  letter-spacing: 0.9px;
}

.profile-name {
  color: #183B5B;
}

.profile-email {
  color: #52657A;
  overflow-wrap: anywhere;
}

.weather-banner {
  background: linear-gradient(120deg, #245B8F, #3477AD);
  overflow: hidden;
}

.banner-chip {
  color: #183B5B;
  font-weight: 800;
  letter-spacing: 0.4px;
}

.banner-description {
  color: #F0F5FB;
  max-width: 580px;
  line-height: 1.8;
  font-size: 15px;
}

.weather-illustration {
  filter: drop-shadow(0 8px 12px rgba(0, 0, 0, 0.12));
}

.feature-card {
  height: 100%;
  min-height: 255px;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.feature-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 9px 22px rgba(36, 91, 143, 0.10);
}

.feature-description {
  color: #52657A;
  font-size: 14px;
  line-height: 1.7;
}

.footer-text {
  color: #64748B;
  font-size: 12px;
}

@media (max-width: 600px) {
  .nav-btn {
    min-width: auto;
    padding: 0 7px;
    font-size: 11px;
  }

  .nav-btn :deep(.v-icon) {
    margin-inline-end: 3px;
  }

  .weather-illustration {
    font-size: 105px !important;
  }

  .profile-card {
    padding: 18px !important;
  }
}
</style>
