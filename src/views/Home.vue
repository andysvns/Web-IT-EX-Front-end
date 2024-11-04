<template>
  <div>
    <v-main class="background" :style="backgroundStyle">
      <img src="../assets/Star Animate.png" alt="" class="rotating-image" />
      <div class="welcome">
        <h3 class="hello-text">Hello World! We are</h3>
        <h1 class="main-title">
          <span
            :class="{
              'typing-animation': isTypingFirstLine,
              'typing-done': !isTypingFirstLine,
            }"
          >
            {{ firstLineText }}
          </span>
        </h1>
        <span class="typing-animation highlight">{{ secondLineText }}</span>
      </div>
    </v-main>

    <v-row class="h-section-1 fade-up">
      <v-col class="h-card-1">
        <h2>{{ HomeAboutUsDetails.title }}</h2>
        <p v-html="formatText(HomeAboutUsDetails.desc)"></p>
      </v-col>
      <v-col class="h-card-2">
        <img class="h-people" :src="HomeAboutUsDetails.img" alt="" />
        <div class="h-banner">
          <h1>10+</h1>
          <h4>Years of experience</h4>
        </div>
      </v-col>
    </v-row>

    <div class="h-section-2">
      <img src="../assets/s3-effect.png" alt="" />
      <div class="s2-card-1">
        <v-row>
          <v-col
            v-for="(task, index) in LiskTaskDetails"
            :key="index"
            cols="12"
            md="6"
            class="s2-card-col"
          >
            <div class="s2-block">
              <v-icon size="28" class="impact-icon">
                {{ task.icon }}
              </v-icon>
              <div class="s2-block-text">
                <h3>{{ task.title }}</h3>
                <p v-html="formatText(task.desc)"></p>
              </div>
            </div>
          </v-col>
        </v-row>
      </div>

      <div class="side-text">
        <h3>What We do</h3>
        <p>
          IT Excellence ເຮົາທຸ້ມເທເພື່ອພັດທະນາຜະລິດຕະພັນໃຫ້ມີປະສິດທິພາບທັນສະໄໝ
          ແລະ ຕອບສະໜອງຄວາມຕ້ອງການທີ່ສຳຄັນທີ່ສຸດຂອງອຸກສາຫະກຳ
        </p>
        <p class="highlight-text">
          Upgrade Our<br />Products
          <span class="object1">Better</span>
          <span class="object2">Bolder</span>
        </p>
      </div>
    </div>

    <div class="h-section-3 fade-up">
      <h2>Our Impact in Numbers</h2>
      <v-row class="h-s-row-1">
        <v-col v-for="(item, index) in iconDetails" :key="index" class="h-card">
          <img
            :src="currentImages[index]"
            @mouseover="handleMouseOver(index)"
            @mouseleave="handleMouseLeave(index)"
            :alt="item.title"
          />
          <h1>{{ item.num_text }}</h1>
          <p v-html="formatText(item.desc)"></p>
        </v-col>
      </v-row>
    </div>

    <div class="h-section-4">
      <v-row class="h-s4-row">
        <v-col cols="12" md="6" class="h-s4-card1">
          <div>
            <h2>Our Products</h2>
            <p>
              ພວກເຮົາພາກພູມໃຈໃນການພັດທະນາ Application Website ຜະລິດຕະພັນຊອບແວຣ໌
              ແລະ
              ເຮົາມຸ່ງໝັ້ນທີ່ຈະນໍາສະເໜີຜະລິດຕະພັນຄຸນນະພາບສູງທີ່ຂັບເຄື່ອນຄວາມສໍາເລັດໃຫ້ກັບລູກຄ້າຂອງເຮົາ
              ບໍ່ວ່າຈະເປັນການສໍາຫຼວດ solutions ຕ່າງໆ ແລະ
              ຄົ້ນພົບວິທີທີ່ສາມາດຊ່ວຍຍົກລະດັບທຸລະກິດໃຫ້ໄປສູ່ອີກລະດັບໜື່ງໄດ້
            </p>
          </div>
          <div class="s4-card1-name">
            <transition name="fade" mode="out-in">
              <h2 :key="currentTitle">{{ currentTitle }}</h2>
            </transition>
            <div class="arrow">
              <v-btn icon @click="prevSlide">
                <v-icon size="40">mdi-arrow-left-thin</v-icon>
              </v-btn>
              <v-btn icon @click="nextSlide">
                <v-icon size="40">mdi-arrow-right-thin</v-icon>
              </v-btn>
            </div>
          </div>
        </v-col>
        <v-col cols="12" md="6" class="carousel-container">
          <v-carousel
            hide-delimiters
            v-model="currentSlide"
            hide-delimiter-background
            :show-arrows="false"
            @change="updateTitle"
            :continuous="false"
            height="400"
          >
            <v-carousel-item
              v-for="(product, index) in OurproductDetails"
              :key="product.our_product_id"
              reverse-transition="fade-transition"
              transition="fade-transition"
              contain
              class="custom-carousel-item"
            >
              <v-img
                :src="product.img"
                :alt="product.title"
                contain
                height="100%"
              />
            </v-carousel-item>
          </v-carousel>
        </v-col>
      </v-row>
    </div>

    <div class="h-section-5">
      <h2>Our Partners</h2>
      <div class="h-s5-row">
        <div class="h-col-partner-text">
          <p>Trusted by over<br />20+ companies</p>
        </div>
        <div
          v-for="(partner, index) in partnersDetails"
          :key="index"
          class="h-col-partner"
        >
          <img
            :src="partner.img"
            :alt="partner.partner_name"
            class="partner-icon"
          />
        </div>
      </div>
    </div>

    <div class="h-section-6">
      <h2>Our Stacks and Tools</h2>
      <v-row class="h-s6-row">
        <v-col class="h-tooltext">
          <!-- Group the unique stack types -->
          <h3
            v-for="type in [
              ...new Set(StacksTool.map((tool) => tool.stack_name)),
            ]"
            :key="type"
            @click="selectItem(type.toLowerCase())"
            :class="{ active: selected === type.toLowerCase() }"
          >
            {{ type }}
          </h3>
        </v-col>
        <v-col class="h-tool">
          <transition
            name="fade"
            @before-enter="beforeEnter"
            @enter="enter"
            @leave="leave"
          >
            <div :key="selected" class="h-tool-content">
              <v-row class="h-tool-row">
                <v-col
                  v-for="tool in groupedTools[selected]"
                  :key="tool.our_st_id"
                  class="h-tool-item"
                >
                  <!-- Use the direct img URL from the API -->
                  <img :src="tool.img" :alt="tool.tool_name" />
                  <p>{{ tool.tool_name }}</p>
                </v-col>
              </v-row>
            </div>
          </transition>
        </v-col>
      </v-row>
    </div>

    <!-- <div class="h-section-6">
      <h2>Our Stacks and Tools</h2>
      <v-row class="h-s6-row">
        <v-col class="h-tooltext">
          <h3
            @click="selectItem('programming')"
            :class="{ active: selected === 'programming' }"
          >
            Programming, Markup, style sheet language
          </h3>
          <h3
            @click="selectItem('frontend')"
            :class="{ active: selected === 'frontend' }"
          >
            Front-end
          </h3>
          <h3
            @click="selectItem('backend')"
            :class="{ active: selected === 'backend' }"
          >
            Back-end
          </h3>
        </v-col>
        <v-col class="h-tool">
          <transition
            name="fade"
            @before-enter="beforeEnter"
            @enter="enter"
            @leave="leave"
          >
            <div :key="selected" class="h-tool-content">
              <div v-if="selected === 'programming'">
                <v-row class="h-tool-row">
                  <v-col
                    v-for="(tool, index) in stacks.programming"
                    :key="index"
                    class="h-tool-item"
                  >
                    <img
                      :src="require(`../assets/icon/${tool.src}`)"
                      :alt="tool.alt"
                    />
                    <p>{{ tool.name }}</p>
                  </v-col>
                </v-row>
              </div>
              <div v-if="selected === 'frontend'">
                <v-row class="h-tool-row">
                  <v-col
                    v-for="(tool, index) in stacks.frontend"
                    :key="index"
                    class="h-tool-item"
                  >
                    <img
                      :src="require(`../assets/icon/${tool.src}`)"
                      :alt="tool.alt"
                    />
                    <p>{{ tool.name }}</p>
                  </v-col>
                </v-row>
              </div>
              <div v-if="selected === 'backend'">
                <v-row class="h-tool-row">
                  <v-col
                    v-for="(tool, index) in stacks.backend"
                    :key="index"
                    class="h-tool-item"
                  >
                    <img
                      :src="require(`../assets/icon/${tool.src}`)"
                      :alt="tool.alt"
                    />
                    <p>{{ tool.name }}</p>
                  </v-col>
                </v-row>
              </div>
            </div>
          </transition>
        </v-col>
      </v-row>
    </div> -->
  </div>
</template>

<script>
import axios from "axios";
// import Ourproducts from "./ourproducts.vue";

export default {
  name: "TypingText",
  data() {
    return {
      fullFirstLine: "IT Excellence",
      firstLineText: "",
      fullSecondLine: "Key of Success",
      secondLineText: "",
      typingSpeed: 90,
      isTypingFirstLine: true,

      currentSlide: 0,
      bannersDetails: null,
      HomeAboutUsDetails: {
        title:'',
        img:'',
        desc:'',
      },
      partnersDetails: [],
      LiskTaskDetails: [],
      iconDetails: [], // Will be populated from API
      currentImages: [], // To track current image states

      OurproductDetails: [],
      StacksType: [],
      StacksTool: [],

      selected: "",
    };
  },

  mounted() {
    this.typeText("firstLineText", this.fullFirstLine, 0);
    setTimeout(() => {
      this.isTypingFirstLine = false;
      this.typeText("secondLineText", this.fullSecondLine, 500);
    }, this.fullFirstLine.length * this.typingSpeed + 500);

    this.startAutoSlide();
    this.initIntersectionObserver();
    this.fetchHomeInfo();
  },
  methods: {
    
    async fetchHomeInfo() {
      try {
        const bannerResponse = axios.get(
          "http://localhost:3000/api/background/getfirst"
        );
        const partnersResponse = axios.get(
          "http://localhost:3000/api/partners/getall"
        );
        const HomeAboutUsResponse = axios.get(
          "http://localhost:3000/api/homeabout/getfirst"
        );
        const ListTaskResponse = axios.get(
          "http://localhost:3000/api/listtask/getall"
        );
        const impactResponse = axios.get(
          "http://localhost:3000/api/impact/getall"
        );

        const OurProductResponse = axios.get(
          "http://localhost:3000/api/products/getall"
        );
        const StacksTypeResponse = axios.get(
          "http://localhost:3000/api/stacktype/getall/"
        );
        const StacksToolResponse = axios.get(
          "http://localhost:3000/api/stacktool/getall/"
        );

        // Await both responses
        const [
          banner,
          PartnersData,
          aboutus,
          ListTask,
          impactData,
          OurProductData,
          StacksTypes,
          StacksTools,
        ] = await Promise.all([
          bannerResponse,
          partnersResponse,
          HomeAboutUsResponse,
          ListTaskResponse,
          impactResponse,
          OurProductResponse,
          StacksTypeResponse,
          StacksToolResponse,
        ]);

        this.bannersDetails = banner.data; // Set data from the awaited intro response
        this.partnersDetails = PartnersData.data; // Set data from the awaited impact response
        this.HomeAboutUsDetails = aboutus.data; // Set data from the awaited impact response
        this.LiskTaskDetails = ListTask.data; // Set data from the awaited impact response
        this.iconDetails = impactData.data;
        this.iconDetails = impactData.data;
        this.OurproductDetails = OurProductData.data;
        this.StacksType = StacksTypes.data;
        this.StacksTool = StacksTools.data;

        // Initialize currentImages with the gif images
        this.currentImages = this.iconDetails.map((item) => item.img_hover);

        if (this.StacksType.length > 0) {
          this.selected = this.StacksType[0].stack_name.toLowerCase();
        }
        this.$nextTick(() => {
          this.initIntersectionObserver();
        });
      } catch (err) {
        this.error = "Failed to fetch contact information";
      }
    },

    handleMouseOver(index) {
      // Show static image on hover
      this.currentImages[index] = this.iconDetails[index].img;
    },
    handleMouseLeave(index) {
      // Show gif when not hovering
      this.currentImages[index] = this.iconDetails[index].img_hover;
    },

    typeText(dataProperty, fullText, delay) {
      this[dataProperty] = "";
      setTimeout(() => {
        let index = 0;
        const typeInterval = setInterval(() => {
          this[dataProperty] += fullText.charAt(index);
          index++;
          if (index > fullText.length) {
            clearInterval(typeInterval);
          }
        }, this.typingSpeed);
      }, delay);
    },
    initIntersectionObserver() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add("visible");
          }
        });
      });

      // Target the elements you want to fade up
      const fadeElements = document.querySelectorAll(
        ".h-section-1, .h-section-3"
      );
      fadeElements.forEach((el) => {
        el.classList.add("fade-up");
        observer.observe(el);
      });
    },

    updateTitle(index) {
      this.currentSlide = index;
    },

    prevSlide() {
      this.currentSlide =
        this.currentSlide > 0
          ? this.currentSlide - 1
          : this.OurproductDetails.length - 1;
    },

    nextSlide() {
      this.currentSlide =
        this.currentSlide < this.OurproductDetails.length - 1
          ? this.currentSlide + 1
          : 0;
    },
    startAutoSlide() {
      setInterval(() => {
        this.nextSlide();
      }, 3000); // Slide every 3 seconds
    },

    selectItem(item) {
      this.selected = item;
    },
    beforeEnter(el) {
      el.style.opacity = 0;
    },
    enter(el, done) {
      el.offsetHeight; // trigger reflow
      el.style.transition = "opacity 0.5s";
      el.style.opacity = 1;
      done();
    },
    leave(el, done) {
      el.style.transition = "opacity 0.5s";
      el.style.opacity = 0;
      done();
    },
    formatText(text) {
      return text ? text.replace(/\n/g, "<br>") : '';
    }
  },

  computed: {
    currentTitle() {
      return this.OurproductDetails.length > 0
        ? this.OurproductDetails[this.currentSlide].title
        : "";
    },
    backgroundStyle() {
      return this.bannersDetails
        ? {
            backgroundImage: `url(${this.bannersDetails.img})`,
          }
        : {};
    },

    groupedTools() {
      return this.StacksTool.reduce((acc, tool) => {
        // Use stack_name as the grouping key
        const type = tool.stack_name?.toLowerCase() || "other";
        if (!acc[type]) acc[type] = [];
        acc[type].push(tool);
        return acc;
      }, {});
    },
  },
};
</script>

<style scoped>
/* Welcome section */

.background {
  background-size: cover;
  background-position: center;
  width: 100vw;
  position: relative;
  min-height: 100vh;
  z-index: 0;
  overflow: hidden;
}

.rotating-image {
  width: 150%;
  /* Make the image larger to allow rotation without revealing edges */
  height: 150%;
  /* Scale the height proportionally */
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  opacity: 0.8;
  animation: rotate-back-and-forth 120s linear infinite;
}

/* Keyframes for rotating clockwise and then counterclockwise */
@keyframes rotate-back-and-forth {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }

  50% {
    transform: translate(-50%, -50%) rotate(360deg);
    /* Complete one full clockwise rotation */
  }

  100% {
    transform: translate(-50%, -50%) rotate(0deg);
    /* Rotate back to the original position (counterclockwise) */
  }
}

.welcome {
  align-items: center;
  text-align: start;
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100vh;
}

.hello-text {
  animation: fadeDown 0.5s ease-out;
  transition-delay: 1s;
  font-weight: 500;
  font-size: 1.5rem;
}

.main-title {
  /* align-items: center; */
  margin: 0;
  font-size: 4vw;
}

.highlight {
  text-align: start;
  font-size: 4vw;
  /* margin-right: 5px; */
  color: #f5821f;
  font-weight: 800;
}

/* Typing Animation */

.typing-animation {
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  position: relative;
  /* Ensure the pseudo-element is positioned relative to this element */
  padding-right: 10px;
}

.typing-animation::after {
  content: "";
  position: absolute;
  right: 0;
  /* Align the cursor to the right end of the text */
  top: 15px;
  /* Adjust cursor height */
  height: 1.2em;
  /* Adjust this value to control the height of the cursor */
  width: 5px;
  /* Width of the cursor */
  background: rgb(255, 255, 255);
  /* Color of the cursor */
  animation: blink-caret 1s step-end infinite;
}

.typing-done {
  border-right: none;
  /* Hide cursor when typing is done */
}

/* Animation for the blinking cursor */
@keyframes blink-caret {
  from,
  to {
    background: transparent;
  }

  50% {
    background: rgb(255, 255, 255);
  }
}

/* Section 1 */

.h-section-1 {
  transition-delay: 0.5s;

  margin: 0 0;
  margin: 5%;
}

.h-card-1 {
  padding: 0;
  align-items: center;
}

.h-card-1 h2,
.h-section-3 h2,
.h-s4-card1 h2 {
  font-size: 2rem;
  padding-bottom: 3%;
  font-weight: 900;
}

.h-card-1 p {
  font-size: 1.5rem;
  padding-bottom: 3%;
  padding-right: 8%;
}

.h-card-2 {
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.h-people {
  width: 80%;
}

.h-banner {
  display: flex;
  background-color: #0772ba;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  width: 20vw;
  max-width: 350px;
  margin-top: -12%;
  margin-left: -50%;
  padding: 2% 5%;
}

.h-banner h1 {
  color: #fbbc04;
  font-size: 3.5rem;
  font-weight: 800;
  padding: 0 6%;
}

.h-banner h4 {
  padding: 0 6%;
  color: white;
  font-weight: 300;
  font-size: 1.28rem;
}

/* Section 2 */

.h-section-2 {
  background-image: url("../assets/service-bg.png");
  background-size: cover;
  background-position: center;
  padding: 5%;
  width: 100vw;
  height: auto;
  display: flex;
  align-items: center;
  justify-content: center;
}

.h-section-2 img {
  width: 90vw;

  /* Make the image larger to allow rotation without revealing edges */
  /* min-height: 50vh; */
  align-items: center;
  position: absolute;
  animation: moveDots 10s ease-in-out infinite;
  display: flex;
}

@keyframes moveDots {
  0% {
    transform: translateY(0);
  }

  25% {
    transform: translateY(-10px) translateX(5px);
  }

  50% {
    transform: translateY(0) translateX(10px);
  }

  75% {
    transform: translateY(10px) translateX(5px);
  }

  100% {
    transform: translateY(0);
  }
}

.s2-card-1 {
  background-color: #ffffff;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  padding: 2%;
  width: 60vw;
  height: auto;
  z-index: 1;
}

.s2-card-col {
  padding: 2% 0;
  /* margin-right: 10%; */
}

.s2-block {
  display: flex;
  flex-direction: row;
  align-items: start;
  gap: 5px;
}

.impact-icon {
  padding: 2%;
  color: #0772ba;
  border-radius: 50%;
  background-color: #e0f7fa;
  margin: 4%;
}

.s2-block-text {
  margin-right: 5%;
}

.s2-block-text h3 {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 5% 0;
}

.s2-block-text p {
  font-size: 1.25rem;
  text-align: justify;
  font-weight: 500;
}

.side-text {
  padding: 4%;
  width: 40vw;
}

.side-text h3 {
  color: white;
  font-size: 2rem;
  font-weight: 500;
}

.side-text {
  color: white;
  font-size: 1.25rem;
}

.highlight-text {
  color: #f5821f;
  font-weight: bold;
  font-size: 3rem;
}

.object1,
.object2 {
  font-size: 3rem;
  font-weight: bold;
  color: white;
  position: absolute;
  /* Position objects absolutely within the container */
}

.object1 {
  /* margin-left: 0.5%; */
  z-index: 1;
  /* Object 1 is behind Object 2 */
  animation: moveUpDown 2.5s infinite ease-in-out;
}

.object2 {
  z-index: 2;
  /* Object 2 is in front of Object 1 */
  animation: moveDownUp 2.5s infinite ease-in-out;
  animation-delay: 0s;
}

@keyframes moveUpDown {
  0%,
  100% {
    opacity: 0;
    /* Object 2 stays hidden */
    transform: translateY(0);
  }

  0%,
  50% {
    opacity: 1;
    /* Object 2 fades in */
    transform: translateY(0);
  }

  0%,
  100% {
    opacity: 0;
    /* Object 2 fades out */
    transform: translateY(-60px);
    /* Slide Object 2 down */
  }
}

@keyframes moveDownUp {
  0%,
  100% {
    transform: translateY(0);
    /* Start and end at the original position */
  }

  50% {
    opacity: 0;
    transform: translateY(60px);
    /* Move down by 30px */
  }
}

/* Section 3 */

.h-section-3 {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition-delay: 0.5s;
  margin: 8% 0;
}

.h-s-row-1 {
  margin: 0;
  padding: 0;
  width: 95vw;
}

.h-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #e0f7fa;
  border-radius: 50%;
  padding: 1.5% 0;
  margin: 0 9%;
  cursor: pointer;
}

.h-card img {
  width: 45%;
}

.h-card h1 {
  font-size: 2rem;
  font-weight: 800;
}

.h-card p {
  font-size: 1rem;
}

/* Section 4 */

.h-section-4 {
  margin: 5%;
}

.h-s4-row {
  display: flex;
  justify-content: space-between;
  margin: 0;
  padding: 0;
}

.h-s4-card1 {
  padding: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.h-s4-card1 h2 {
  padding-bottom: 2%;
}

.h-s4-card1 p {
  padding-right: 18%;
  font-size: 1.25rem;
}

.s4-card1-name {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 0;
}

.s4-card1-name h2 {
  color: #0772ba;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0.5;
}

.arrow {
  margin-right: 10%;
  display: flex;
  align-items: center;
}

.carousel-container {
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: auto;
  height: 400px;
}

.carousel-image {
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.custom-carousel-item {
  display: flex;
  justify-content: center;
  align-items: center;
}

.custom-carousel-item .v-image__image {
  background-size: contain !important;
  background-position: center center;
  max-width: 100%;
  max-height: 100%;
  margin: auto;
}

/* Section 5 */

.h-section-5 {
  margin: 5%;
  margin-bottom: 8%;
  text-align: center;
}

.h-section-5 h2 {
  font-size: 2rem;
  font-weight: 900;
  text-align: center;
  padding: 2%;
}

.h-s5-row {
  margin: 0 5%;
  padding: 0 0;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.h-col-partner-text {
  /* padding: 2% 4%; */
  display: flex;
  align-items: center;
  font-size: 1rem;
}

.h-col-partner {
  display: flex;
  align-items: center;
  justify-content: end;
  padding: 1% 3%;
}

.h-col-partner img {
  height: 75px;
  object-fit: contain;
  width: auto;
  /* filter: grayscale(100%) */
}

/* Section 6 */

.h-section-6 {
  background-image: url("../assets/h-s6-bg.png");
  background-size: cover;
  background-position: center;
  padding: 5%;
  height: 650px;
}

.h-section-6 h2 {
  font-size: 2rem;
  font-weight: 900;
  padding: 0;
  margin-bottom: 1%;
}

.h-s6-row {
  margin: 0 0;
}

.h-tooltext {
  padding: 1% 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
  position: relative;
}

.h-tooltext::before {
  content: "";
  position: absolute;
  top: 10%;
  bottom: 10%;
  left: 0;
  width: 2px;
  background-color: rgb(192, 192, 192);
}

.h-tooltext h3 {
  cursor: pointer;
  font-size: 1.75rem;
  color: #9a9a9a;
  padding: 0 2%;
  position: relative;
  z-index: 1;
  /* Make sure the text stays above the line */
  font-weight: 500;
  transition: color 1s ease;
}

.h-tooltext h3.active {
  color: #2c2c33;
  font-weight: 700;
  border-left: none;
}

.h-tooltext h3.active::before {
  content: "";
  position: absolute;
  left: 0;
  top: 10%;
  /* Adjust this value to position the top of the border */
  bottom: 10%;
  /* Adjust this value to position the bottom of the border */
  width: 3px;
  background-color: black;
  transition: all 1s ease;
}

.h-tool {
  align-content: center;
}

.h-tool-row {
  margin-left: 10%;
}

.h-tool-item {
  padding: 0;
  text-align: center;
}

.h-tool-item img {
  width: 75px;
  border-radius: 15%;
}

@media (max-width: 1200px) {
  .main-title,
  .highlight {
    font-size: 5vw;
    /* Adjust font size for medium screens */
  }

  .hello-text {
    font-size: 1.25rem;
  }

  .h-section-2 {
    display: flex;
    flex-direction: column-reverse;
  }

  .s2-card-1 {
    width: 98%;
    margin-bottom: 3%;
    padding: 5%;
  }

  .side-text {
    width: 80vw;
  }

  .h-card-1 h2,
  .h-section-3 h2,
  .h-s4-card1 h2,
  .h-section-5 h2,
  .h-section-6 h2 {
    font-size: 1.75rem;
  }

  .h-card-1 p {
    font-size: 1.125rem;
    padding-right: 4%;
  }

  .h-banner h1 {
    font-size: 2rem;
  }

  .h-banner h4 {
    font-size: 1rem;
  }

  .h-card h1,
  .h-tooltext h3 {
    font-size: 1.5rem;
  }

  .s2-block-text h3 {
    margin: 2% 0;
  }

  .impact-icon {
    margin: 2%;
  }

  .h-card p {
    font-size: 0.75rem;
  }

  .h-section-4 {
    margin-top: 10%;
  }

  .h-s4-card1 p,
  .s2-block-text p {
    font-size: 1.125rem;
  }

  .s4-card1-name {
    margin: 10% 0;
  }

  .s4-card1-name h2 {
    font-size: 1.5rem;
  }

  .arrow {
    margin-right: 0;
  }

  .h-col-partner-text {
    font-size: 0.85rem;
  }

  .h-col-partner img {
    height: 60px;
  }

  .h-tool-item img {
    width: 60px;
  }

  .h-col-partner {
    justify-content: center;
  }

  .h-section-6 {
    height: 500px;
  }
}

@media (max-width: 768px) {
  .hello-text {
    font-size: 1rem;
  }

  .typing-animation::after {
    height: 1em;
  }

  .h-section-1 {
    display: flex;
    flex-direction: column;
    /* margin: 5% 2%; */
  }

  .h-card-1 p,
  .h-card-1 h2 {
    padding: 2%;
  }

  .h-banner {
    width: 25vw;
    margin-top: -10%;
    margin-left: -65%;
  }

  .h-banner h1 {
    font-size: 1.75rem;
  }

  .h-banner h4 {
    font-size: 0.825rem;
  }

  .highlight-text,
  .object1,
  .object2 {
    font-size: 2.5rem;
  }

  .side-text {
    font-size: 1.125rem;
  }

  .h-card {
    margin: 0 5%;
  }

  .h-card p {
    font-size: 0.75rem;
  }

  .h-card h1,
  .side-text h3,
  .h-card-1 h2,
  .h-section-3 h2,
  .h-s4-card1 h2,
  .h-section-5 h2,
  .h-section-6 h2 {
    font-size: 1.5rem;
  }

  .h-s4-card1 p {
    padding: 0;
  }

  .h-col-partner-text {
    padding: 2%;
  }

  .h-col-partner {
    padding: 3% 4%;
  }

  .h-col-partner img {
    height: 50px;
  }

  .h-s5-row {
    justify-content: center;
  }

  .h-s6-row {
    display: flex;
    flex-direction: column;
  }

  .h-tooltext {
    margin-bottom: 5%;
  }

  .h-tooltext h3 {
    font-size: 1.25rem;
  }

  .s2-block-text h3 {
    font-size: 1.25rem;
    margin: 2% 0;
  }

  .h-tool-item img {
    width: 50px;
  }

  .h-tool-row {
    margin-left: 0;
  }

  .h-section-6 {
    height: 400px;
  }
}

@media (max-width: 480px) {
  .main-title,
  .highlight {
    font-size: 8vw;
  }

  .typing-animation::after {
    height: 0.9em;
  }

  .h-banner {
    width: 28vw;
  }

  .h-banner h1 {
    font-size: 1.5rem;
  }

  .h-banner h4 {
    font-size: 0.75rem;
  }

  .h-card {
    margin: 0 1%;
  }

  .h-card h1 {
    font-size: 1.5rem;
  }

  .h-card-1 h2,
  .h-section-3 h2,
  .h-s4-card1 h2,
  .h-section-5 h2,
  .h-section-6 h2 {
    font-size: 1.25rem;
  }

  .h-tooltext h3 {
    font-size: 1rem;
  }

  .highlight-text,
  .object1,
  .object2 {
    font-size: 2rem;
  }

  .s2-block-text p,
  .side-text {
    font-size: 1rem;
  }
}

/* Fade animation */

@keyframes fadeDown {
  0% {
    opacity: 0;
    transform: translateY(-60px);
    /* Starts 20px below its original position */
  }

  100% {
    opacity: 1;
    transform: translateY(0);
    /* Moves to its original position */
  }
}

@keyframes fadeUp {
  0% {
    opacity: 0;
    transform: translateY(30px);
  }

  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade {
  opacity: 1;
  transition: opacity 0.5s ease;
}

.fade-up {
  opacity: 0;
  transform: translateY(100px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.fade-up.visible {
  opacity: 1;
  transform: translateY(0);
}

.h-tool-content {
  display: flex;
  flex-direction: column;
}

.h-tool-item img {
  max-width: 100%;
  height: auto;
}
</style>
