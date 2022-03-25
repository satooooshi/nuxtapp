<template>
  <v-card
    :loading="loading"
    class="ma-0"
    max-width="374"
    style="background-color:#fafafa;color:black;"
  >
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img
      height="250"
      src="https://cdn.shopify.com/s/files/1/0408/1015/2103/products/2022-03-22_22h50_48_200x.png?v=1648107845"
    ></v-img>

    <v-card-title>be good do good Tシャツ</v-card-title>

    <v-card-text>
      <v-row
        align="center"
        class="mx-0"
      >
        <v-rating
          :value="4.5"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ms-4">
          4.5 (413)
        </div>
      </v-row>

      <div class="my-4 text-subtitle-1" style="color:red;">
        ¥6,100
      </div>

    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    <v-card-actions>
      <v-btn
        color="black lighten-2"
        text
        @click="handleWhishlist"
      >
        後で見る
      </v-btn>
       <v-btn
        color="black lighten-2"
        text
        @click="handleDetail"
      >
        詳細
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
// https://vuetifyjs.com/ja/components/cards/
  export default {
    data: () => ({
      loading: false,
      selection: 1,
    }),

    methods: {
      reserve () {
        this.loading = true

        setTimeout(() => (this.loading = false), 2000)
      },
    },
  }
</script>