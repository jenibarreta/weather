<template>
  <v-app>
    <v-main class="weather-page">

      <!-- Navigation -->
      <v-app-bar color="#245B8F" flat height="76">
        <v-container class="d-flex align-center nav-container">
          <div class="d-flex align-center ga-2">
            <v-avatar color="#F4C95D" size="42">
              <v-icon color="#245B8F" size="27">
                mdi-weather-partly-cloudy
              </v-icon>
            </v-avatar>

            <span class="text-h6 font-weight-bold text-white">
              Weather App
            </span>
          </div>

          <v-spacer />

          <div class="d-none d-sm-flex align-center ga-2">
            <v-btn
              to="/dashboard"
              variant="text"
              color="white"
              prepend-icon="mdi-view-dashboard"
              class="nav-button"
            >
              Dashboard
            </v-btn>

            <v-btn
              to="/weather"
              variant="flat"
              color="#F4C95D"
              class="nav-button active-nav"
              prepend-icon="mdi-weather-partly-cloudy"
            >
              Weather
            </v-btn>
          </div>

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

            <v-list min-width="200" rounded="lg">
              <v-list-item
                prepend-icon="mdi-account-circle"
                :title="user?.name || 'My Account'"
                :subtitle="user?.email || ''"
              />

              <v-divider />

              <v-list-item
                prepend-icon="mdi-view-dashboard"
                title="Dashboard"
                to="/dashboard"
              />

              <v-list-item
                prepend-icon="mdi-logout"
                title="Logout"
                to="/logout"
              />
            </v-list>
          </v-menu>
        </v-container>
      </v-app-bar>

      <v-container class="py-8 main-container">

        <!-- Header -->
        <section class="page-header mb-8">
          <div class="d-flex align-center ga-2 mb-2">
            <v-icon color="#245B8F" size="22">
              mdi-weather-cloudy
            </v-icon>

            <span class="section-label">
              WEATHER INFORMATION
            </span>
          </div>

          <h1 class="text-h4 font-weight-bold main-heading">
            Weather Forecast
          </h1>

          <p class="text-body-1 readable-text mt-2">
            Discover current weather conditions and hourly forecasts
            in your chosen city.
          </p>
        </section>

        <!-- City Search -->
        <v-card
          rounded="xl"
          flat
          class="search-card pa-4 pa-md-5 mb-7"
        >
          <div class="d-flex align-center ga-2 mb-4">
            <v-avatar color="#E8F0FA" size="40">
              <v-icon color="#245B8F">
                mdi-map-search
              </v-icon>
            </v-avatar>

            <div>
              <h2 class="text-subtitle-1 font-weight-bold main-heading">
                Search Location
              </h2>

              <p class="text-caption readable-text">
                Enter a city to view its weather information.
              </p>
            </div>
          </div>

          <v-form @submit.prevent="searchWeather">
            <v-row align="center">
              <v-col cols="12" md="9">
                <v-text-field
                  v-model="cityInput"
                  label="City Name"
                  placeholder="Example: Manila, Tokyo, London"
                  prepend-inner-icon="mdi-map-marker"
                  variant="outlined"
                  density="comfortable"
                  hide-details
                  clearable
                  :disabled="loading"
                  class="city-search"
                />
              </v-col>

              <v-col cols="12" md="3">
                <v-btn
                  type="submit"
                  color="#245B8F"
                  block
                  size="large"
                  rounded="lg"
                  :loading="loading"
                  :disabled="loading"
                  class="search-button"
                >
                  <v-icon start>mdi-magnify</v-icon>
                  Search Weather
                </v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card>

        <!-- Error -->
        <v-alert
          v-if="errorMessage"
          type="error"
          variant="tonal"
          rounded="lg"
          class="mb-6"
          prepend-icon="mdi-alert-circle"
        >
          {{ errorMessage }}
        </v-alert>

        <!-- Loading -->
        <v-card
          v-if="loading"
          class="loading-card pa-8 text-center mb-6"
          rounded="xl"
          flat
        >
          <v-progress-circular
            indeterminate
            color="#245B8F"
            size="52"
            width="5"
          />

          <h3 class="text-h6 font-weight-bold main-heading mt-5">
            Getting Weather Information
          </h3>

          <p class="readable-text mt-2">
            Please wait while we retrieve the latest weather data.
          </p>
        </v-card>

        <template v-else-if="weather">

          <!-- Current Weather -->
          <v-card
            rounded="xl"
            flat
            class="weather-hero mb-7"
          >
            <v-row align="center" no-gutters>
              <v-col cols="12" md="7" class="pa-6 pa-md-8">
                <div class="d-flex align-center ga-2 mb-4">
                  <v-avatar color="#FFF4D5" size="38">
                    <v-icon color="#245B8F">
                      mdi-map-marker
                    </v-icon>
                  </v-avatar>

                  <div>
                    <p class="location-label">
                      CURRENT LOCATION
                    </p>

                    <h2 class="text-h6 font-weight-bold location-name">
                      {{ weather.city }}
                    </h2>
                  </div>
                </div>

                <div class="d-flex align-center flex-wrap">
                  <h2 class="temperature-text">
                    {{ weather.temperature }}°
                  </h2>

                  <div class="ml-4 mt-2">
                    <h3 class="text-h6 font-weight-bold condition-text">
                      {{ weather.description }}
                    </h3>

                    <p class="text-body-2 condition-subtitle mt-1">
                      Feels like {{ weather.feelsLike }}°C
                    </p>
                  </div>
                </div>

                <v-divider class="my-5" />

                <div class="d-flex align-center ga-2">
                  <v-icon color="#245B8F" size="19">
                    mdi-clock-outline
                  </v-icon>

                  <span class="text-body-2 readable-text">
                    Current weather conditions
                  </span>
                </div>
              </v-col>

              <v-col
                cols="12"
                md="5"
                class="weather-icon-section text-center pa-6"
              >
                <v-icon
                  color="#245B8F"
                  size="145"
                  class="main-weather-icon"
                >
                  {{ weather.icon }}
                </v-icon>

                <p class="text-body-1 font-weight-bold mt-2 main-heading">
                  {{ weather.description }}
                </p>

                <p class="text-caption readable-text mt-1">
                  Live weather overview
                </p>
              </v-col>
            </v-row>
          </v-card>

          <!-- Details Heading -->
          <div class="d-flex align-center ga-2 mb-4">
            <v-icon color="#245B8F" size="23">
              mdi-chart-box-outline
            </v-icon>

            <h2 class="text-h5 font-weight-bold main-heading">
              Weather Details
            </h2>
          </div>

          <!-- Weather Statistics -->
          <v-row class="mb-6">
            <v-col cols="12" sm="4">
              <v-card class="detail-card pa-5" rounded="xl" flat>
                <div class="d-flex align-center justify-space-between mb-5">
                  <v-avatar color="#E8F0FA" size="52">
                    <v-icon color="#245B8F" size="28">
                      mdi-water-percent
                    </v-icon>
                  </v-avatar>

                  <v-chip color="#245B8F" variant="tonal" size="small">
                    Humidity
                  </v-chip>
                </div>

                <p class="detail-label">
                  Relative Humidity
                </p>

                <h3 class="detail-value">
                  {{ weather.humidity }}<span>%</span>
                </h3>

                <p class="detail-description">
                  Moisture level in the air
                </p>
              </v-card>
            </v-col>

            <v-col cols="12" sm="4">
              <v-card class="detail-card pa-5" rounded="xl" flat>
                <div class="d-flex align-center justify-space-between mb-5">
                  <v-avatar color="#FFF4D5" size="52">
                    <v-icon color="#245B8F" size="28">
                      mdi-weather-windy
                    </v-icon>
                  </v-avatar>

                  <v-chip color="#245B8F" variant="tonal" size="small">
                    Wind
                  </v-chip>
                </div>

                <p class="detail-label">
                  Wind Speed
                </p>

                <h3 class="detail-value">
                  {{ weather.windSpeed }}
                  <span>km/h</span>
                </h3>

                <p class="detail-description">
                  Current wind movement
                </p>
              </v-card>
            </v-col>

            <v-col cols="12" sm="4">
              <v-card class="detail-card pa-5" rounded="xl" flat>
                <div class="d-flex align-center justify-space-between mb-5">
                  <v-avatar color="#E8F0FA" size="52">
                    <v-icon color="#245B8F" size="28">
                      mdi-thermometer
                    </v-icon>
                  </v-avatar>

                  <v-chip color="#245B8F" variant="tonal" size="small">
                    Feels Like
                  </v-chip>
                </div>

                <p class="detail-label">
                  Apparent Temperature
                </p>

                <h3 class="detail-value">
                  {{ weather.feelsLike }}<span>°C</span>
                </h3>

                <p class="detail-description">
                  How the temperature feels
                </p>
              </v-card>
            </v-col>
          </v-row>

          <!-- Chart -->
          <v-card
            rounded="xl"
            flat
            class="chart-card pa-4 pa-md-6"
          >
            <div class="d-flex align-center justify-space-between flex-wrap ga-3 mb-6">
              <div>
                <div class="d-flex align-center ga-2 mb-2">
                  <v-avatar color="#E8F0FA" size="38">
                    <v-icon color="#245B8F">
                      mdi-chart-line
                    </v-icon>
                  </v-avatar>

                  <h2 class="text-h6 font-weight-bold main-heading">
                    Hourly Forecast
                  </h2>
                </div>

                <p class="text-body-2 readable-text">
                  Temperature changes over the next 12 hours.
                </p>
              </div>

              <v-chip
                color="#F4C95D"
                variant="flat"
                prepend-icon="mdi-chart-areaspline"
                class="chart-chip"
              >
                Chart.js
              </v-chip>
            </div>

            <v-divider class="mb-5" />

            <div class="chart-container">
              <Line
                :data="chartData"
                :options="chartOptions"
              />
            </div>

            <div class="d-flex align-center justify-center ga-2 mt-4">
              <v-icon color="#245B8F" size="18">
                mdi-information-outline
              </v-icon>

              <p class="text-caption readable-text">
                Temperature values are displayed in degrees Celsius.
              </p>
            </div>
          </v-card>

        </template>

        <!-- Footer -->
        <v-divider class="my-8" />

        <div class="d-flex flex-column flex-sm-row align-center justify-space-between ga-3 pb-4">
          <div class="d-flex align-center ga-2">
            <v-icon color="#245B8F" size="22">
              mdi-weather-partly-cloudy
            </v-icon>

            <span class="footer-text font-weight-bold">
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
// @ts-nocheck

import { ref, onMounted } from 'vue'

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Filler
} from 'chart.js'

import { Line } from 'vue-chartjs'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Filler
)

// User profile
const user = ref(null)

onMounted(() => {
  const savedUser = localStorage.getItem('google_user')

  if (savedUser) {
    try {
      user.value = JSON.parse(savedUser)
    } catch (error) {
      console.error('Unable to load user profile:', error)
    }
  }

  searchWeather()
})

// Weather states
const cityInput = ref('Manila')
const loading = ref(false)
const errorMessage = ref('')
const weather = ref(null)

// Chart data
const chartData = ref({
  labels: [],
  datasets: [
    {
      label: 'Temperature (°C)',
      data: [],
      borderColor: '#245B8F',
      backgroundColor: 'rgba(244, 201, 93, 0.25)',
      pointBackgroundColor: '#F4C95D',
      pointBorderColor: '#245B8F',
      pointRadius: 5,
      pointHoverRadius: 7,
      borderWidth: 3,
      tension: 0.4,
      fill: true
    }
  ]
})

// Chart options
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,

  plugins: {
    legend: {
      display: true,
      position: 'top',
      labels: {
        color: '#334155',
        font: {
          size: 12,
          weight: 'bold'
        },
        usePointStyle: true
      }
    },

    tooltip: {
      enabled: true,
      backgroundColor: '#183B5B',
      titleColor: '#FFFFFF',
      bodyColor: '#FFFFFF',
      padding: 12,
      displayColors: false
    }
  },

  scales: {
    y: {
      beginAtZero: false,
      ticks: {
        color: '#52657A'
      },
      grid: {
        color: '#E5EAF1'
      },
      title: {
        display: true,
        text: 'Temperature (°C)',
        color: '#334155',
        font: {
          weight: 'bold'
        }
      }
    },

    x: {
      ticks: {
        color: '#52657A'
      },
      grid: {
        display: false
      },
      title: {
        display: true,
        text: 'Time',
        color: '#334155',
        font: {
          weight: 'bold'
        }
      }
    }
  }
}

// Weather descriptions and icons
const getWeatherCondition = (code: number) => {
  const conditions = {
    0: ['Clear Sky', 'mdi-weather-sunny'],
    1: ['Mainly Clear', 'mdi-weather-sunny'],
    2: ['Partly Cloudy', 'mdi-weather-partly-cloudy'],
    3: ['Overcast', 'mdi-weather-cloudy'],
    45: ['Foggy', 'mdi-weather-fog'],
    48: ['Depositing Rime Fog', 'mdi-weather-fog'],
    51: ['Light Drizzle', 'mdi-weather-rainy'],
    53: ['Moderate Drizzle', 'mdi-weather-rainy'],
    55: ['Dense Drizzle', 'mdi-weather-pouring'],
    61: ['Slight Rain', 'mdi-weather-rainy'],
    63: ['Moderate Rain', 'mdi-weather-pouring'],
    65: ['Heavy Rain', 'mdi-weather-pouring'],
    71: ['Slight Snow', 'mdi-weather-snowy'],
    73: ['Moderate Snow', 'mdi-weather-snowy'],
    75: ['Heavy Snow', 'mdi-weather-snowy'],
    80: ['Rain Showers', 'mdi-weather-pouring'],
    81: ['Moderate Rain Showers', 'mdi-weather-pouring'],
    82: ['Violent Rain Showers', 'mdi-weather-pouring'],
    95: ['Thunderstorm', 'mdi-weather-lightning'],
    96: ['Thunderstorm with Hail', 'mdi-weather-lightning-rainy'],
    99: ['Heavy Thunderstorm', 'mdi-weather-lightning-rainy']
  }

  return conditions[code] || [
    'Weather Unavailable',
    'mdi-weather-cloudy'
  ]
}

// Search city and get weather information
const searchWeather = async () => {
  const city = cityInput.value?.trim()

  if (!city) {
    errorMessage.value = 'Please enter a city name.'
    return
  }

  loading.value = true
  errorMessage.value = ''
  weather.value = null

  try {
    // Step 1: Get city coordinates
    const geoResponse = await $fetch(
      'https://geocoding-api.open-meteo.com/v1/search',
      {
        query: {
          name: city,
          count: 1,
          language: 'en',
          format: 'json'
        }
      }
    )

    if (!geoResponse.results || geoResponse.results.length === 0) {
      throw new Error('City not found. Please check the city name.')
    }

    const location = geoResponse.results[0]

    // Step 2: Get weather forecast
    const forecastResponse = await $fetch(
      'https://api.open-meteo.com/v1/forecast',
      {
        query: {
          latitude: location.latitude,
          longitude: location.longitude,
          current: [
            'temperature_2m',
            'relative_humidity_2m',
            'apparent_temperature',
            'weather_code',
            'wind_speed_10m'
          ].join(','),
          hourly: 'temperature_2m',
          forecast_days: 1,
          timezone: 'auto'
        }
      }
    )

    const current = forecastResponse.current

    const [description, icon] = getWeatherCondition(
      current.weather_code
    )

    // Current weather information
    weather.value = {
      city: `${location.name}, ${location.country}`,
      temperature: Math.round(current.temperature_2m),
      feelsLike: Math.round(current.apparent_temperature),
      humidity: current.relative_humidity_2m,
      windSpeed: current.wind_speed_10m,
      description,
      icon
    }

    // Step 3: Prepare next 12 hours for Chart.js
    const hourlyTimes = forecastResponse.hourly.time
    const hourlyTemperatures =
      forecastResponse.hourly.temperature_2m

    const currentTime = new Date(current.time).getTime()

    let startIndex = hourlyTimes.findIndex((time) => {
      return new Date(time).getTime() >= currentTime
    })

    if (startIndex < 0) {
      startIndex = 0
    }

    const nextTimes = hourlyTimes.slice(
      startIndex,
      startIndex + 12
    )

    const nextTemperatures = hourlyTemperatures.slice(
      startIndex,
      startIndex + 12
    )

    const labels = nextTimes.map((time) => {
      const timePart = time.split('T')[1] || '00:00'
      const hour = Number(timePart.substring(0, 2))

      return new Date(
        2000,
        0,
        1,
        hour
      ).toLocaleTimeString('en-US', {
        hour: 'numeric',
        hour12: true
      })
    })

    chartData.value = {
      labels,
      datasets: [
        {
          label: 'Temperature (°C)',
          data: nextTemperatures,
          borderColor: '#245B8F',
          backgroundColor: 'rgba(244, 201, 93, 0.25)',
          pointBackgroundColor: '#F4C95D',
          pointBorderColor: '#245B8F',
          pointRadius: 5,
          pointHoverRadius: 7,
          borderWidth: 3,
          tension: 0.4,
          fill: true
        }
      ]
    }

  } catch (error) {
    console.error('Weather search error:', error)

    errorMessage.value =
      error?.data?.reason ||
      error?.message ||
      'Unable to load weather information. Please try again.'
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.weather-page {
  min-height: 100vh;
  background: #F5F8FC;
}

.nav-container,
.main-container {
  max-width: 1200px;
}

.nav-button {
  text-transform: none;
  font-weight: 600;
  letter-spacing: 0;
}

.active-nav {
  color: #183B5B !important;
}

.section-label {
  color: #245B8F;
  font-size: 13px;
  font-weight: 800;
  letter-spacing: 1.1px;
}

.main-heading {
  color: #183B5B;
}

.readable-text {
  color: #52657A;
  line-height: 1.7;
}

.search-card,
.loading-card,
.detail-card,
.chart-card {
  background: #FFFFFF;
  border: 1px solid #E0E8F1;
  box-shadow: 0 4px 16px rgba(36, 91, 143, 0.045);
}

.city-search {
  color: #183B5B;
}

.search-button {
  font-weight: 700;
  letter-spacing: 0;
}

.weather-hero {
  background: linear-gradient(110deg, #FFFFFF 55%, #EAF2FA 100%);
  border: 1px solid #E0E8F1;
  border-left: 6px solid #F4C95D;
  overflow: hidden;
}

.location-label {
  color: #64748B;
  font-size: 11px;
  font-weight: 800;
  letter-spacing: 1px;
}

.location-name {
  color: #183B5B;
}

.temperature-text {
  color: #245B8F;
  font-size: clamp(65px, 10vw, 96px);
  line-height: 1;
  font-weight: 800;
  letter-spacing: -3px;
}

.condition-text {
  color: #183B5B;
}

.condition-subtitle {
  color: #64748B;
}

.weather-icon-section {
  background: rgba(36, 91, 143, 0.035);
  min-height: 260px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.main-weather-icon {
  filter: drop-shadow(0 8px 10px rgba(36, 91, 143, 0.12));
}

.detail-card {
  height: 100%;
  min-height: 220px;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.detail-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 9px 22px rgba(36, 91, 143, 0.09);
}

.detail-label {
  color: #52657A;
  font-size: 13px;
  font-weight: 600;
}

.detail-value {
  color: #245B8F;
  font-size: 35px;
  font-weight: 800;
  margin-top: 8px;
  line-height: 1.3;
}

.detail-value span {
  font-size: 16px;
  font-weight: 600;
}

.detail-description {
  color: #64748B;
  font-size: 12px;
  margin-top: 7px;
}

.chart-chip {
  color: #183B5B;
  font-weight: 700;
}

.chart-container {
  position: relative;
  width: 100%;
  height: 350px;
}

.footer-text {
  color: #64748B;
  font-size: 12px;
}

:deep(.v-btn) {
  text-transform: none;
  letter-spacing: 0;
}

@media (max-width: 600px) {
  .temperature-text {
    font-size: 66px;
  }

  .weather-icon-section {
    min-height: 190px;
  }

  .main-weather-icon {
    font-size: 110px !important;
  }

  .chart-container {
    height: 270px;
  }

  .detail-card {
    min-height: 190px;
  }
}
</style>
