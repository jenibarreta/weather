<template>
  <v-app>
    <v-main class="logout-page">
      <v-container class="fill-height d-flex align-center justify-center pa-4">

        <v-card
          width="100%"
          max-width="460"
          rounded="xl"
          elevation="0"
          class="logout-card pa-6 pa-sm-9 text-center"
        >
          <!-- App Logo -->
          <v-avatar
            color="#FFF4D5"
            size="82"
            class="mb-5"
          >
            <v-icon color="#245B8F" size="42">
              mdi-weather-partly-cloudy
            </v-icon>
          </v-avatar>

          <!-- Heading -->
          <p class="section-label mb-2">
            WEATHER APP
          </p>

          <h1 class="text-h4 font-weight-bold heading mb-3">
            Leaving So Soon?
          </h1>

          <p class="text-body-1 description mb-7">
            Are you sure you want to log out of your account?
          </p>

          <!-- Confirmation Panel -->
          <v-card
            rounded="lg"
            flat
            class="confirmation-panel pa-4 mb-7"
          >
            <div class="d-flex align-center text-left ga-3">
              <v-avatar color="#E8F0FA" size="44">
                <v-icon color="#245B8F" size="24">
                  mdi-shield-account-outline
                </v-icon>
              </v-avatar>

              <div>
                <p class="text-body-2 font-weight-bold heading">
                  Your account is protected
                </p>

                <p class="text-caption description mt-1">
                  You will need to sign in again to access your dashboard.
                </p>
              </div>
            </div>
          </v-card>

          <!-- Logout Button -->
          <v-btn
            block
            color="#245B8F"
            size="large"
            rounded="lg"
            class="logout-button mb-3"
            @click="confirmLogout"
          >
            <v-icon start>
              mdi-logout
            </v-icon>

            Yes, Log Out
          </v-btn>

          <!-- Cancel Button -->
          <v-btn
            block
            variant="outlined"
            color="#245B8F"
            size="large"
            rounded="lg"
            class="cancel-button"
            @click="cancelLogout"
          >
            <v-icon start>
              mdi-arrow-left
            </v-icon>

            Stay Logged In
          </v-btn>

          <!-- Footer -->
          <v-divider class="my-7" />

          <div class="d-flex align-center justify-center ga-2">
            <v-icon color="#245B8F" size="19">
              mdi-weather-partly-cloudy
            </v-icon>

            <span class="footer-text font-weight-bold">
              Weather App
            </span>
          </div>

          <p class="text-caption footer-subtitle mt-2">
            Thank you for using our application.
          </p>
        </v-card>

      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
const confirmLogout = async () => {
  localStorage.removeItem('google_user')
  localStorage.removeItem('google_token')

  await navigateTo('/login')
}

const cancelLogout = async () => {
  await navigateTo('/dashboard')
}
</script>

<style scoped>
.logout-page {
  min-height: 100vh;
  background:
    radial-gradient(
      circle at 15% 15%,
      rgba(244, 201, 93, 0.16),
      transparent 30%
    ),
    radial-gradient(
      circle at 85% 85%,
      rgba(36, 91, 143, 0.10),
      transparent 32%
    ),
    #F5F8FC;
}

.logout-card {
  background: #FFFFFF;
  border: 1px solid #E3EAF2;
  box-shadow: 0 15px 45px rgba(36, 91, 143, 0.09);
}

.section-label {
  color: #245B8F;
  font-size: 12px;
  font-weight: 800;
  letter-spacing: 1.5px;
}

.heading {
  color: #183B5B;
}

.description {
  color: #64748B;
  line-height: 1.7;
}

.confirmation-panel {
  background: #F5F8FC;
  border: 1px solid #E5EAF1;
}

.logout-button {
  font-weight: 700;
  letter-spacing: 0;
  box-shadow: 0 4px 10px rgba(36, 91, 143, 0.15);
}

.cancel-button {
  font-weight: 700;
  letter-spacing: 0;
  border-width: 1.5px;
}

.footer-text {
  color: #245B8F;
  font-size: 13px;
}

.footer-subtitle {
  color: #64748B;
}

:deep(.v-btn) {
  text-transform: none;
}

@media (max-width: 600px) {
  .logout-card {
    padding: 24px !important;
  }
}
</style>

