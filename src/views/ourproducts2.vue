<template>
  <div>
    <v-main class="background-b main">
      <v-container fluid class="op-h1">
        <div>
          <h1>Our Products</h1>
        </div>
      </v-container>
    </v-main>
    <div class="op-section-1">
      <v-row
        v-for="(product, index) in formattedProducts"
        :key="index"
        :class="['op-row']"
      >
        <v-col class="text-col">
          <h1>{{ index + 1 }}. {{ product.title }}</h1>
          <p v-html="formatText(product.description)" class="p-desc"></p>
        </v-col>
        <v-col class="image-col">
          <img
            :src="product.image"
            :alt="product.title"
            class="product-image"
          />
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      ourProductDetails: [],
      error: null,
    };
  },
  computed: {
    formattedProducts() {
      return this.ourProductDetails.map((product) => ({
        title: product.title,
        image: product.img,
        description: product.desc,
      }));
    },
  },
  methods: {
    formatText(text) {
      return text ? text.replace(/\n/g, "<br>") : "";
    },
    async fetchOurProduct() {
      try {
        const response = await axios.get(
          "http://localhost:3000/api/products/getall"
        );
        this.ourProductDetails = response.data;
      } catch (err) {
        this.error = "Failed to fetch product information";
        console.error(err);
      }
    },
  },
  mounted() {
    this.fetchOurProduct();
  },
};
</script>

<style scoped>
.op-h1 {
  color: aliceblue;
  font-size: 2rem;
  text-align: center;
}

.op-section-1 {
  background-image: url("/src/assets/effect-op-bg.png");
  background-size: cover;
  background-position: center;
  height: auto;
}

.op-row {
  align-items: center;
  margin: 0 3%;
  padding: 5% 0;
}

.image-col {
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 3%;
}

.text-col {
  padding: 1.5%;
  background-color: #f4fbffc6;
  margin: 0 3%;
  border-radius: 5px;
}

.text-col h1 {
  padding-bottom: 2%;
}

.text-col p {
  padding: 15px 0;
  font-size: 22px;
}

.product-image {
  width: 80%;
  height: auto;
}

/* Desktop layout */
@media (min-width: 769px) {
  .op-row {
    display: flex;
    flex-direction: row;
  }
  
  /* Odd numbered products (1,3) - Image right, text left */
  .op-row:nth-child(odd) {
    flex-direction: row;
  }

  /* Even numbered products (2,4) - Image left, text right */
  .op-row:nth-child(even) {
    flex-direction: row-reverse;
  }
}

/* Mobile layout */
@media (max-width: 768px) {
  .op-row {
    flex-direction: column;
    padding: 0;
  }

  .text-col {
    margin-bottom: 20px;
  }

  .text-col h1 {
    font-size: 1.5rem;
  }

  .text-col p {
    font-size: 1rem;
  }

  .product-image {
    width: 60%;
    margin: 0 0 30px 0;
  }

  /* Ensure consistent spacing between sections on mobile */
  .op-row:not(:last-child) {
    margin-bottom: 20px;
  }
  
  .text-col {
    margin: 0;
    padding: 0;
  }
  
  .text-col p {
    padding: 0;
  }
  
  .image-col {
    margin: 0;
  }
  
  .op-row {
    margin: 2% 3%;
    padding: 1% 1%;
  }
}

@media (max-width: 480px) {
  .op-h1 h1 {
    font-size: 2rem;
  }

  .product-image {
    width: 80%;
  }
}
</style>