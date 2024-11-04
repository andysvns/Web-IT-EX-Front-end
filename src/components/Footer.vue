<template>
  <v-footer class="footer">
    <v-container fluid>
      <v-row class="footer-warp">
        <v-col cols="12" md="3">
          <v-card-text class="f-card">
            <img src="../assets/logo1.png" alt="" class="footer-logo" />
            <p>
              ພວກເຮົາຈະເປັນພະລັງຂັບເຄື່ອນໃນການພັດທະນາ<br />
              ບໍລິສັດ ອົງກອນ ໃຫ້ປະສົບຜົນສຳເລັກ <br />
              ບໍລິການທີ່ມີປະສິດທິພາບຂອງພວກເຮົາ
            </p>
          </v-card-text>
        </v-col>
        <v-col cols="12" md="3">
          <v-card-text class="f-card">
            <h2>About IT Excellence</h2>
            <p>
              <router-link to="/about" class="text-link">
                About Us
              </router-link>
            </p>
            <p>
              <router-link to="/ourproducts" class="text-link">
                Our Products
              </router-link>
            </p>
          </v-card-text>
        </v-col>
        <v-col cols="12" md="3">
          <v-card-text class="f-card">
            <h2>Contact Us</h2>
            <p v-if="contactfooter">
              {{ contactfooter.phone_number }}<br />
              {{ contactfooter.email }}<br />
              {{ contactfooter.address }}<br />
              <!-- <a 
                v-if="contactfooter.address_url" 
                :href="contactfooter.address_url" 
                target="_blank"
                class="text-link"
              >
                View on Map
              </a> -->
            </p>
            <p v-else>Loading contact information...</p>
          </v-card-text>
        </v-col>
        <v-col cols="12" md="3">
          <v-card-text class="f-card">
            <h2>Follow Us</h2>
            <div class="social-icons">
              <div
                v-for="platform in SocialTypeDetails"
                :key="platform.s_type_name"
                class="social-icon-wrapper"
              >
                <v-menu
                  v-if="platform.SocialUrlDetails.length > 1"
                  offset-y
                  :close-on-content-click="false"
                  transition="slide-y-transition"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon v-bind="attrs" v-on="on" class="footer-icon">
                      <v-icon>{{ platform.icon }}</v-icon>
                    </v-btn>
                  </template>
                  <v-card class="social-menu theme-card">
                    <v-list dense>
                      <v-list-item
                        v-for="(urlDetail, index) in platform.SocialUrlDetails"
                        :key="index"
                        @click="openSocialLink(urlDetail.social_url)"
                        class="social-menu-item theme-list-item"
                      >
                        <v-list-item-title>{{
                          urlDetail.title
                        }}</v-list-item-title>
                      </v-list-item>
                    </v-list>
                  </v-card>
                </v-menu>

                <v-btn
                  v-else
                  icon
                  class="footer-icon"
                  @click="
                    openSocialLink(platform.SocialUrlDetails[0].social_url)
                  "
                >
                  <v-icon>{{ platform.icon }}</v-icon>
                </v-btn>
              </div>
            </div>
          </v-card-text>
        </v-col>
      </v-row>
      <v-divider class="divider"></v-divider>
      <v-card-text class="copyright-text">
        <p>
          CompanyName @ {{ new Date().getFullYear() }}. All rights reserved.
        </p>
      </v-card-text>
    </v-container>
  </v-footer>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      contactfooter: {
        phone_number: "",
        email: "",
        address_url: "",
        address: "",
      },
      SocialTypeDetails: [],
      loading: true,
      error: null,
    };
  },
  mounted() {
    this.fetchSocialUrl();
  },
  methods: {
    async fetchSocialUrl() {
      try {
        // Fetch all data concurrently
        const [socialTypeResponse, socialUrlResponse, contactResponse] =
          await Promise.all([
            axios.get("http://localhost:3000/api/socialtype/getall"),
            axios.get("http://localhost:3000/api/socialurl/getall"),
            axios.get("http://localhost:3000/api/contact/getall"),
          ]);

        // Handle contact data
        const contactData = contactResponse.data;
        this.contactfooter = Array.isArray(contactData)
          ? contactData[0]
          : contactData;

        // Handle social data
        const socialTypeData = socialTypeResponse.data;
        const socialUrlData = socialUrlResponse.data;

        this.SocialTypeDetails = socialTypeData.map((type) => ({
          ...type,
          SocialUrlDetails: socialUrlData.filter(
            (url) => url.social_type_id === type.social_type_id
          ),
        }));

        this.loading = false;
      } catch (err) {
        this.error = "Failed to fetch data";
        console.error("Error fetching data:", err);
        this.loading = false;
      }
    },

    openSocialLink(url) {
      window.open(url, "_blank");
    },
  },
};
</script>

<style scoped>
.footer {
  padding: 0 20px;
}

.footer-warp {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0;
  padding: 0;
  padding-top: 10px;
}

.footer-logo {
  width: 100px;
  height: 35px;
  margin-bottom: 10px;
}

.f-card {
  color: white;
  padding: 0;
}

.text-link {
  color: #ffffff;
  text-decoration: none;
}

.f-card h2 {
  margin-bottom: 10px;
}

.social-icons {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.social-icon-wrapper {
  position: relative;
}

.footer-icon {
  color: white !important;
  margin: 0 !important;
}

.theme-card {
  background-color: #ffffff; /* White background for the pop-up */
  padding: 8px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.theme-list-item {
  color: #333 !important; /* Dark text for readability */
  padding: 8px 16px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.theme-list-item:hover {
  background-color: rgba(0, 0, 0, 0.05) !important; /* Light gray on hover */
}

.social-menu-item {
  min-width: 150px;
}

.divider {
  margin: 10px 0;
  background-color: white;
}

.copyright-text {
  color: white;
  text-align: center;
}

/* Dark theme for the menu */
::v-deep .v-list {
  background-color: #ffffff !important;
  padding: 0;
}

::v-deep .v-list-item {
  color: #333 !important;
}

@media (max-width: 768px) {
  .footer {
    padding: 0 10px;
  }

  .footer-logo {
    width: 80px;
    height: 30px;
  }

  .f-card {
    padding: 0px 0px;
  }

  .f-card p {
    margin-top: 2%;
  }

  .social-icons {
    gap: 4px;
  }

  .social-menu-item {
    min-width: 120px;
  }
}

@media (max-width: 480px) {
  .footer {
    padding: 0 5px;
  }

  .footer-logo {
    width: 60px;
    height: 25px;
  }

  .f-card {
    padding: 0px 5%;
  }
}
</style>
