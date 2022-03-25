<template>
  <div>
    <v-container
      class="grey lighten-5 pa-1"
      style="color:black"
    >
      <v-row
        align="start"
        justify="center"
        no-gutters
        style="height: 200px;"
      >
        <v-col
          md="3"
          style="height: 200px;background-color:#fafafa;"
          class="placement-image-1"
        >
        <img src="https://cdn.shopify.com/s/files/1/0408/1015/2103/products/2022-03-22_22h50_48_200x.png?v=1648107845" alt="thumbnail" width="auto" height="100%" >
        </v-col>
        <v-col
          md="9"
          style=""
        >
          <v-row
            align="start"
            no-gutters
            style="height:60px"
          >
          <v-col>
          <p class="ma-4 text-h4">
          be good do good TシャツOt1809
          </p>
          </v-col>
          </v-row>
          <v-row
            align="start"
            no-gutters
            style="height:50px"
          >
          <v-col>
          <p class="ma-4 text-h5">
          ¥6,100
          </p>
          </v-col>
          </v-row>
          <v-row
            align="start"
            no-gutters
            style="height:50px"
          >
          <v-col>
          <p class="ma-4 text-h5">
          White / M
          </p>
          </v-col>
          </v-row>
          <v-row
            key=1
            align="start"
            justify="start"
            no-gutters
            style="height: 40px;"
          >
          <v-col md="1">
            <p class="text-center text-h5">-</p>
          </v-col>
          <v-col md="1">
            <p class="text-center text-h5">4</p>
          </v-col>
          <v-col md="1">
            <p class="text-center text-h5">+</p>
          </v-col>
          <v-col md="9">
             <p class="text-right text-h5 text-right mx-12"><button  @click="()=>{}" >削除</button></p>
          </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
// https://masa-enjoy.com/vuetify-beginner#toc4
// https://vuetifyjs.com/ja/styles/text-and-typography/
/*
style, class, 
<p class="text-center text-button" style="color:red;">+</p>
        <v-col
          md="3"
          style="height: 200px;"
          class="placement-image-1"
        >
*/
</script>

<style scoped>
.placement-image-1{
  height:200px; /* can be leave out to inherent from parent v-row style="height: 200px;" */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgb(255, 255, 255);
}

</style>