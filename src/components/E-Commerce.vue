<script>
export default {
  data() {
    return {
      results: [],
      count: 0,
      isLoading: true,
      bgColor: "",
      color: "",
    };
  },
  methods: {
    async fetchFakeStoreAPI() {
      const api = await fetch("https://fakestoreapi.com/products");
      const results = await api.json();
      this.results = results;

      const currentCategory = this.results[this.count].category;

      if (currentCategory === "men's clothing") {
        this.bgColor = "linear-gradient(180deg, #D6E6FF 65%, white 50%)"; // Warna biru
        this.color = "#002772";
      } else if (currentCategory === "women's clothing") {
        this.bgColor = "linear-gradient(180deg, #FDE2FF 65%, white 50%"; // Warna pink
        this.color = "#720060";
      } else {
        this.bgColor = "linear-gradient(180deg, #DCDCDC 65%, white 50%"; // Warna abu-abu
      }

      this.isLoading = false;
    },

    nextProduct() {
      this.isLoading = true;
      if (this.count === this.results.length) {
        this.count = 0;
      }
      this.fetchFakeStoreAPI();
    },
  },
  computed: {
    isUnavailableCategory() {
      const category = this.results[this.count]?.category;
      return category === "jewelery" || category === "electronics";
    },
  },
  mounted() {
    this.fetchFakeStoreAPI();
  },
};
</script>

<template>
  <div class="home">
    <div :style="{ background: bgColor }" class="home">
      <div class="overlay">
        <img
          src="https://ecommerce-api-9f7ae.web.app/img/bg-shape.29c2da3c.svg"
          alt="bg overlay"
        />
      </div>
      <div class="box" v-if="isLoading || isFetching">
        <div class="skeleton-image"></div>
        <div class="skeleton-content">
          <div class="skeleton-top"></div>
          <div class="skeleton-bottom"></div>
        </div>
      </div>
      <div class="box" v-else>
        <div v-if="isUnavailableCategory" class="allContentUnavailable">
          <button class="next" @click="nextProduct(count++)">
            Next Product
          </button>
        </div>
        <div v-else class="allContent">
          <div class="image">
            <img :src="results[count].image" alt="image" />
          </div>
          <div class="content">
            <div class="content-right-top">
              <div :style="{ color: color }" class="title">
                {{ results[count].title }}
              </div>
              <div class="rate">
                <div class="rateTitle">{{ results[count].category }}</div>
                <div class="rateStar">
                  {{ results[count].rating.rate }}/5
                  <span class="rating">
                    <span
                      v-for="i in 5"
                      :key="i"
                      :class="{
                        filled: i <= Math.round(results[count].rating.rate),
                        unfilled: i > Math.round(results[count].rating.rate),
                      }"
                      :style="{
                        backgroundColor:
                          i <= Math.round(results[count].rating.rate)
                            ? color
                            : '',
                      }"
                    ></span>
                  </span>
                </div>
              </div>
              <hr />
              <div class="desc">{{ results[count].description }}</div>
            </div>
            <div class="content-right-bottom">
              <hr />
              <div :style="{ color: color }" class="price">
                ${{ results[count].price }}
              </div>
              <div class="btn">
                <button
                  :style="{ background: color, border: '2px solid' + color }"
                  class="buy"
                >
                  Buy Now
                </button>
                <button
                  :style="{ color: color, border: '2px solid' + color }"
                  class="next"
                  @click="nextProduct(count++)"
                >
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
