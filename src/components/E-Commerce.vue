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

<style scoped>
.home {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.overlay,
.overlay > img {
  width: 100%;
}
.overlay {
  position: absolute;
  z-index: 1;
  top: -12%;
  left: 0;
}
.box {
  display: flex;
  position: relative;
  z-index: 2;
  width: 70%;
  height: 60vh;
  background-color: white;
  border-radius: 10px;
  padding: 48px 32px;
  box-shadow: 0 4px 6px -1px black, 0 2px 4px -2px black;
}

.skeleton-image {
  flex: 30%;
  background-color: #ddd;
  height: 100%;
  padding-bottom: 20px;
}

.skeleton-content {
  flex: 70%;
  padding-left: 20px;
  display: grid;
  grid-template-rows: 30% 100%;
  gap: 20px;
}

.skeleton-top {
  width: 100%;

  background-color: #ddd;
}

.skeleton-bottom {
  width: 100%;
  height: 70%;
  background-color: #ddd;
}

.skeleton-desc {
  width: 100%;
  height: 100px;
  background-color: #ddd;
}
.allContent {
  width: 100%;
  display: grid;
  grid-template-columns: 30% 65%;
  gap: 16px;
  height: 100%;
}
.image {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 35%;
}

.image img {
  width: 80%;
}

.content {
  flex: 65%;
  font-family: "Inter", sans-serif;
  display: flex;
  width: 100%;
  flex-direction: column;
  justify-content: space-between;
}

.title {
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 34px;
  /* identical to box height */
  padding-bottom: 17px;
}

hr {
  border: 1px solid rgba(0, 0, 0, 0.2);
}

.rate {
  display: flex;
  justify-content: space-between;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
  line-height: 22px;
  /* identical to box height */

  color: #3f3f3f;
}
.rateTitle {
  text-align: left;
}
.rateStar {
  text-align: right;
}

.filled {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 1px solid;
  background-color: #002772;
  margin-right: 2px;
}
.filled:last-child {
  margin-right: 0;
}

.unfilled {
  display: inline-block;
  width: 10px;
  height: 10px;
  border: 1px solid #002772;
  border-radius: 50%;
  margin-right: 2px;
}
.unfilled:last-child {
  margin-right: 0;
}
.desc {
  font-style: normal;
  font-weight: 400;
  font-size: 20px;
  line-height: 24px;

  color: #1e1e1e;
}
.price {
  font-size: 32px;
  font-weight: 600;
  margin-bottom: 10px;
}
.btn {
  margin-top: 16px;
  width: 100%;
  display: flex;
  gap: 12px;
}
.buy {
  width: 35%;
  height: 36px;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  color: white;
}

.next {
  width: 35%;
  height: 36px;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  background: white;
}
</style>
