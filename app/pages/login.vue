<template>
  <v-app>
    <v-main class="login-background">
      <v-container class="fill-height d-flex align-center justify-center pa-4">
        <v-card
          width="100%"
          max-width="480"
          rounded="xl"
          elevation="4"
          class="login-card"
        >
          <!-- Header -->
          <div class="top-decoration">
            <v-icon size="42" color="#F4C95D">
              mdi-weather-partly-cloudy
            </v-icon>
            <span class="text-white font-weight-bold text-h6 ml-2">
              Weather App
            </span>
          </div>

          <div class="pa-6 pa-sm-8">
            <!-- Welcome -->
            <div class="text-center mb-7">
              <v-avatar
                color="#FFF4D5"
                size="82"
                class="mb-4"
              >
                <v-icon color="#245B8F" size="48">
                  mdi-white-balance-sunny
                </v-icon>
              </v-avatar>

              <h1 class="text-h4 font-weight-bold ocean-text">
                Welcome Back!
              </h1>

              <p class="text-body-1 subtitle-text mt-2">
                Your daily weather companion
              </p>

              <p class="text-body-2 description-text mt-3">
                Sign in to explore weather conditions,
                temperature updates, and hourly forecasts.
              </p>
            </div>

            <!-- Features -->
            <v-row dense class="mb-6">
              <v-col cols="4">
                <div class="feature-box">
                  <v-avatar color="#E8F0FA" size="46">
                    <v-icon color="#245B8F" size="26">
                      mdi-thermometer
                    </v-icon>
                  </v-avatar>

                  <p>Temperature</p>
                </div>
              </v-col>

              <v-col cols="4">
                <div class="feature-box">
                  <v-avatar color="#FFF4D5" size="46">
                    <v-icon color="#245B8F" size="26">
                      mdi-water-percent
                    </v-icon>
                  </v-avatar>

                  <p>Humidity</p>
                </div>
              </v-col>

              <v-col cols="4">
                <div class="feature-box">
                  <v-avatar color="#E8F0FA" size="46">
                    <v-icon color="#245B8F" size="26">
                      mdi-chart-line
                    </v-icon>
                  </v-avatar>

                  <p>Forecast</p>
                </div>
              </v-col>
            </v-row>

            <!-- Error Message -->
            <v-alert
              v-if="errorMessage"
              type="error"
              variant="tonal"
              density="comfortable"
              rounded="lg"
              class="mb-5"
              icon="mdi-alert-circle"
            >
              {{ errorMessage }}
            </v-alert>

            <!-- Google Sign In -->
            <v-btn
              block
              size="x-large"
              rounded="lg"
              color="#245B8F"
              class="google-button"
              :loading="loading"
              :disabled="loading"
              @click="loginWithGoogle"
            >
              <v-icon start size="22">mdi-google</v-icon>
              Sign in with Google
            </v-btn>

            <!-- Security Note -->
            <div class="security-note mt-6">
              <v-icon size="18" color="#245B8F">
                mdi-shield-check
              </v-icon>

              <span>
                Secure authentication powered by Google
              </span>
            </div>
          </div>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
// @ts-nocheck

import { ref } from 'vue'

const config = useRuntimeConfig()

const loading = ref(false)
const errorMessage = ref('')

declare global {
  interface Window {
    google: any
  }
}

const loginWithGoogle = () => {
  errorMessage.value = ''

  if (!window.google?.accounts?.oauth2) {
    errorMessage.value =
      'Google Sign-In is not available. Please refresh the page and try again.'
    return
  }

  loading.value = true

  try {
    const client = window.google.accounts.oauth2.initTokenClient({
      client_id: config.public.googleClientId,
      scope: 'openid email profile',

      callback: async (response: any) => {
        if (response.error || !response.access_token) {
          errorMessage.value =
            'Google Sign-In failed. Please try again.'
          loading.value = false
          return
        }

        try {
          const user = await $fetch(
            'https://www.googleapis.com/oauth2/v2/userinfo',
            {
              headers: {
                Authorization: `Bearer ${response.access_token}`
              }
            }
          )

          localStorage.setItem(
            'google_user',
            JSON.stringify(user)
          )

          localStorage.setItem(
            'google_token',
            response.access_token
          )

          await navigateTo('/dashboard')
        } catch (error) {
          console.error('Google user information error:', error)

          errorMessage.value =
            'Unable to retrieve your Google account information.'
        } finally {
          loading.value = false
        }
      },

      error_callback: (error: any) => {
        console.error('Google Sign-In error:', error)

        errorMessage.value =
          'Google Sign-In was cancelled or failed. Please try again.'

        loading.value = false
      }
    })

    client.requestAccessToken()
  } catch (error) {
    console.error('Google Sign-In error:', error)

    errorMessage.value =
      'Something went wrong. Please try again.'

    loading.value = false
  }
}
</script>

<style scoped>
.login-background {
  min-height: 100vh;
  background:
    radial-gradient(circle at 10% 15%, #FFF4D5 0, transparent 28%),
    radial-gradient(circle at 90% 85%, #E8F0FA 0, transparent 30%),
    #F5F8FC;
}

.login-card {
  overflow: hidden;
  background: #FFFFFF;
  border: 1px solid #E5EAF1;
}

.top-decoration {
  min-height: 90px;
  background: #245B8F;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}

.ocean-text {
  color: #245B8F;
}

.subtitle-text {
  color: #40546B;
  font-weight: 600;
}

.description-text {
  color: #64748B;
  line-height: 1.7;
  max-width: 340px;
  margin-left: auto;
  margin-right: auto;
}

.feature-box {
  background: #F5F8FC;
  border: 1px solid #E5EAF1;
  border-radius: 16px;
  min-height: 116px;
  padding: 13px 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 9px;
}

.feature-box p {
  color: #245B8F;
  font-size: 12px;
  font-weight: 700;
  text-align: center;
}

.google-button {
  font-weight: 700;
  letter-spacing: 0.2px;
}

.security-note {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  color: #52657A;
  font-size: 12px;
  font-weight: 500;
}

@media (max-width: 400px) {
  .feature-box p {
    font-size: 10px;
  }
}
</style>


