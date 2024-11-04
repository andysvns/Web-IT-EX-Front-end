<template>
  <div>
    <v-main class="background-b">
      <v-container fluid class="ab-container">
        <div class="c-text-1">
          <h1>Contact Us</h1>
          <h4>We're good listeners and even better problem-solvers</h4>
        </div>
      </v-container>
    </v-main>
    <div class="c-banner">
      <div class="c-detail">
        <v-row>
          <v-col>
            <div class="c-info-inner">
              <div class="c-info-icon">
                <v-icon size="24px">mdi-phone</v-icon>
              </div>
              <div>
                <h3>Phone number</h3>
                <p>{{ contact.phone_number }}</p>
              </div>
            </div>
          </v-col>
          <v-col>
            <div class="c-info-inner">
              <div class="c-info-icon">
                <v-icon size="24px">mdi-email</v-icon>
              </div>
              <div>
                <h3>Email</h3>
                <p>{{ contact.email }}</p>
              </div>
            </div>
          </v-col>
          <v-col>
            <div class="c-info-inner">
              <div class="c-info-icon">
                <v-icon size="24px">mdi-map-marker</v-icon>
              </div>
              <div>
                <div>
                  <h3>Location</h3>
                  <p v-html="formattedDesc"></p>
                </div>
              </div>
            </div>
          </v-col>
        </v-row>
        <div class="c-googlemap">
          <h3>Find Us on Google Map</h3>
          <iframe
            :src="contact.address_url"
            width="600"
            height="450"
            style="border: 0"
            allowfullscreen=""
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
          ></iframe>
        </div>
      </div>
    </div>
    <section class="c-section-2"></section>
  </div>
</template>
<script>
import axios from "axios";

export default {
  //   name: 'Contact',
  data() {
    return {
      contact: {
        phone_number: "",
        email: "",
        address_url: "",
        address: "",
      },
      loading: true,
      error: null,
    };
  },
  mounted() {
    this.fetchContactInfo();
  },
  methods: {
    async fetchContactInfo() {
      try {
        const response = await axios.get(
          "http://localhost:3000/api/contact/getall"
        );
        this.contact = response.data[0]; // Assuming the API returns an array and we want the first item
        this.loading = false;
      } catch (err) {
        this.error = "Failed to fetch contact information";
        this.loading = false;
        console.error(err);
      }
    },
  },
  computed: {
    formattedDesc() {
      return this.contact.address.replace(/\n/g, "<br>");
    },
  },
};
</script>

<style scoped>
.c-text-1 {
  color: aliceblue;
  align-items: center;
  justify-content: center;
  text-align: center;
  align-content: center;
  gap: 4px;
}

.c-text-1 h1 {
  font-size: 4rem;
  /* Adjusted for better responsiveness */
  font-weight: 700;
  padding-bottom: 15px;
}

.c-text-1 h4 {
  font-size: 1.125rem;
  /* Adjusted for better responsiveness */
  font-weight: 400;
}

.c-banner {
  display: flex;
  justify-content: center;
  margin-top: -2%;
}

.c-detail {
  width: 80%;
  /* Adjusted for responsiveness */
  max-width: 1200px;
  /* Max width for large screens */
  height: auto;
  /* Allow height to adjust based on content */
  background-color: white;
  margin-top: -5%;
  border-radius: 15px;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.2);
  padding: 3%;
  padding-top: 5%;
}

.c-info-inner {
  display: flex;
  margin: 0 5px;
  margin-bottom: 1rem;
  /* Space between info items */
}

.c-info-icon {
  padding: 15px;
}

.c-googlemap {
  margin-top: 10px;
  font-weight: bold;
}

.c-googlemap iframe {
  width: 100%;
  /* Make map responsive */
  height: 500px;
  /* Maintain aspect ratio */
}

.c-googlemap h3 {
  text-align: left;
}

.c-section-2 {
  background-color: white;
  width: 100vw;
  margin-bottom: 3%;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .c-text-1 h1 {
    font-size: 3rem;
    /* Smaller font size for smaller screens */
  }

  .c-text-1 h4 {
    font-size: 1.5rem;
    /* Smaller font size for smaller screens */
  }

  .c-detail {
    width: 90%;
    /* Increase width for smaller screens */
  }

  .c-googlemap iframe {
    height: 400px;
    /* Maintain aspect ratio */
  }
}

@media (max-width: 480px) {
  .c-text-1 h1 {
    font-size: 3rem;
    /* Even smaller font size for very small screens */
  }

  .c-text-1 h4 {
    font-size: 1rem;
    /* Smaller font size for very small screens */
  }

  .c-info-inner {
    /* flex-direction: row; */
    /* Stack info items vertically */
    align-items: flex-start;
  }
  .c-info-inner h3 {
    font-size: 1rem;
  }
  .c-info-inner p {
    font-size: 0.9rem;
  }

  .c-info-icon {
    padding: 10px;
    /* Adjust padding for smaller screens */
  }
}
</style>
