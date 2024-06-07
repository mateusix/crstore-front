<template>
    <v-main style="background-color: white; height: 100vh; text-align: center;">
    <v-row style="background-color: #fafafa" justify="center" no-gutters>
      <!-- <img class="mt-1" src="https://i.pinimg.com/736x/15/28/40/152840a623436d49ebc85093d4e7572c.jpg" alt="" style="width: 55px; height: 55px; justify-self: center"> -->
      <v-spacer/>
      <a>
      <svg class="mt-3" width="65" height="43" viewBox="0 0 80 43" fill="red" xmlns="http://www.w3.org/2000/svg"><path d="M0 24.2583H6.08948L9.49868 7.47168H3.40838L0 24.2583Z"></path><path d="M3.94678 4.83583H10.0338L11.016 0H4.92165L3.94678 4.83583Z"></path><path d="M8.49829 28.9853H14.5878L18.283 10.818H22.7267L23.4262 7.47195H18.9449L19.0985 6.6962C19.3853 5.26503 19.8503 3.6004 22.1465 3.6004C23.5071 3.6004 24.8113 3.74801 26.0762 4.27989L26.7692 0.744473C25.361 0.250433 23.8765 -0.00141515 22.381 5.98087e-06C17.4871 5.98087e-06 14.1163 2.9137 12.8824 7.47195H10.7675L10.1065 10.818H12.1902L8.49829 28.9853Z"></path><path d="M27.7899 24.6327C34.9809 24.6327 39.9705 18.0641 39.9705 12.706C39.9705 8.80399 36.2819 7.09766 32.7754 7.09766C24.8849 7.09766 20.5916 14.1613 20.5916 19.0252C20.5916 22.9272 24.3178 24.6327 27.7899 24.6327Z"></path><path d="M47.0008 24.6327C54.1918 24.6327 59.1814 18.0641 59.1814 12.706C59.1814 8.80399 55.4854 7.09766 51.9822 7.09766C44.0958 7.09766 39.7959 14.1613 39.7959 19.0252C39.7959 22.9272 43.5262 24.6327 46.9975 24.6327"></path><path d="M69.082 24.2579H75.1372L80.0001 0.374023H73.9098L72.4585 7.47133C71.6501 7.29637 70.8258 7.20203 69.9981 7.18974C64.0328 7.18974 58.6077 14.778 58.6077 20.2011C58.6077 22.4321 60.0581 24.6318 62.5202 24.6318C66.0193 24.6318 68.4414 22.919 69.4522 21.3138H69.8297L69.082 24.2579Z"></path><path d="M50.3215 36.9096C45.868 40.7017 39.9142 42.6487 33.4431 42.2468C25.2837 41.7454 19.6265 35.4279 18.489 29.0197H18.8763C20.7117 33.111 25.1799 36.8887 30.7873 37.5096C36.2623 38.1137 42.9711 35.6397 46.6525 32.1082L42.5225 28.9844L54.7865 29.0197L52.1471 42.0029L50.3215 36.9096Z"></path></svg>
    </a>
      <v-btn
        v-for="(link, i) in links"
        :key="i"
        :to="link.to"
        class="mx-2 mt-3"
        color="black"
        rounded="xl"
        variant="text"
      >
        {{ link.nome }}
      </v-btn>
      <v-spacer/>
    </v-row>
    
  <v-card
    :disabled="loading"
    :loading="loading"
    class="mx-auto my-12"
    max-width="374"
  >
    <template v-slot:loader="{ isActive }">
      <v-progress-linear
        :active="isActive"
        color="deep-purple"
        height="4"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img
      height="250"
      src="https://pbs.twimg.com/media/FZGHfYVXEAEqH3u.jpg:large"
      cover
    ></v-img>

    <v-card-item>
      <v-card-title>Cafe Badilico</v-card-title>

      <v-card-subtitle>
        <span class="me-1">Boracharia do carlao</span>

        <v-icon color="error" icon="mdi-fire-circle" size="small"></v-icon>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      <v-row align="center" class="mx-0">
        <v-rating
          :model-value="1"
          color="amber"
          density="compact"
          size="small"
          half-increments
          readonly
        ></v-rating>
        <div class="text-grey ms-4">4.5 (413)</div>
      </v-row>

      <div class="my-4 text-subtitle-1">$ • 9999 </div>

      <div>
        luva de receba
      </div>
    </v-card-text>

    <v-card-actions>
      <v-btn
        color="deep-purple-lighten-2"
        text="Reserve"
        block
        border
        @click="reserve"
      ></v-btn>
    </v-card-actions>
  </v-card>
</v-main>
</template>

<script>
definePageMeta({
    layout: 'admin'
  });
  export default {
    name: 'homePage',
    data: () => ({
      links: [
        {
          nome: 'foods',
          to: '/foods'
        },
        {
          nome: 'about Us',
          to: '/cupons'
        },
        {
          nome: 'Team',
          to: '/cupons'
        },
        {
          nome: 'services',
          to: '/cupons'
        },
        {
          nome: 'Contact Us',
          to: '/cupons'
        },
      ],
      loading: false,
      selection: 1,
    }),

    methods: {
      reserve() {
        this.loading = true

        setTimeout(() => (this.loading = false), 2000)
      },
    },
  }
</script>
