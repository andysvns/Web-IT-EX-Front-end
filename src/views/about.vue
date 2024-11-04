<template>
  <div>
    <v-main class="background-b">
      <v-container fluid class="ab-container">
        <div class="ab-text-1">
          <h1>About Us</h1>
        </div>
      </v-container>
    </v-main>
    <div>
      <v-row class="ab-section-1">
        <v-col cols="12" md="6" class="people">
          <v-img :src="intro.img" alt="People" />
        </v-col>
        <v-col cols="12" md="6" class="ab-section-1-text">
          <div class="ab-text-detail">
            <h1>{{ intro.title }}</h1>
            <p v-html="formattedDesc"></p>

            <div class="ab-section-1-icon">
              <v-row>
                <v-col
                  v-for="(impact, index) in iconDetails"
                  :key="index"
                  cols="12"
                  md="4"
                >
                  <div class="ab-inner-detail">
                    <div class="img-impact">
                      <v-img :src="impact.img_hover" max-height="60" contain>
                      </v-img>
                    </div>
                    <div class="ab-icon-detail">
                      <h1>{{ impact.num_text }}</h1>
                      <p v-html="formatText(impact.desc)"></p>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </div>
          </div>
        </v-col>
      </v-row>
    </div>
    <div class="ab-main-3">
      <div class="ab-section-2">
        <div class="ab-card-1 fade-up">
          <h2>Vision</h2>
          <p>
            ບໍລິສັດຊັ້ນນຳທີ່
            <span class="ab-highlight">ເຂົ້າໃຈຄວາມຕ້ອງການຂອງລູກຄ້າ</span> ແລະ
            <span class="ab-highlight">ເປົ້າໝາຍຂອງຄູ່ຄ້າທາງທຸລະກິດ</span>
            ຢ່າງແທ້ຈິງ ພວກເຮົາພ້ອມພັດທະນາປັບປຸງທາງດ້ານຜະລິດຕະພັນ ແລະ <br />
            ການບໍລິການຢ່າງຕໍ່ເນື່ອງ
            <span class="ab-highlight"
              >ເພື່ອໃຫ້ລູກຄ້າເກີດຄວາມພືງພໍໃຈສູງສຸດ</span
            >
          </p>
        </div>
        <div class="ab-card-2 fade-up">
          <h2>Mission</h2>
          <p>
            ພວກເຮົາຈະເປັນ ພະລັງຂັບເຄື່ອນໃນການພັດທະນາ ບໍລິສັດ ອົງກອນ
            ໃຫ້ປະສົບຜົນສໍາເລັດດ້ວຍ
            <span class="ab-highlight"
              >ການບໍລິການທີ່ມີປະສິດທິພາບຂອງພວກເຮົາ</span
            >
          </p>
        </div>
      </div>
    </div>

    <div class="ab-main-4">
      <h2>Our Team Members</h2>

      <div class="member-section">
        <v-row
          v-for="(row, rowIndex) in chunkedMembers"
          :key="rowIndex"
          :class="{ 'pic-first-row': rowIndex === 0 }"
        >
          <v-col
            v-for="(member, memberIndex) in row"
            :key="memberIndex + rowIndex * rowSize"
            class="profile-pic"
            cols="12"
            :md="rowIndex === 0 ? 6 : 3"
          >
            <v-avatar :size="avatarSize">
              <img :src="member.mem_img" alt="Profile Picture" />
            </v-avatar>
            <p>{{ member.mem_name }}</p>
            <span>{{ member.mem_position }}</span>
          </v-col>
        </v-row>
      </div>
    </div>
    <!-- <div class="ab-main-4">
      <h2>Our Team Members</h2>

      <div class="member-section">
        <v-row class="pic-first-row">
          <v-col
            v-for="(member, index) in teamMembers.slice(0, 2)"
            :key="index"
            class="profile-pic"
          >
            <v-avatar :size="avatarSize">
              <img :src="member.avatar" alt="Profile Picture" />
            </v-avatar>
            <p>{{ member.name }}</p>
            <span>{{ member.position }}</span>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            v-for="(member, index) in teamMembers.slice(2, 6)"
            :key="index + 2"
            class="profile-pic"
          >
            <v-avatar :size="avatarSize">
              <img :src="member.avatar" alt="Profile Picture" />
            </v-avatar>
            <p>{{ member.name }}</p>
            <span>{{ member.position }}</span>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            v-for="(member, index) in teamMembers.slice(6)"
            :key="index + 6"
            class="profile-pic"
          >
            <v-avatar :size="avatarSize">
              <img :src="member.avatar" alt="Profile Picture" />
            </v-avatar>
            <p>{{ member.name }}</p>
            <span>{{ member.position }}</span>
          </v-col>
        </v-row>
      </div>
    </div> -->
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      avatarSize: this.getAvatarSize(),

      loading: true,
      error: null,
      intro: {
        img: "",
        title: "",
        desc: "",
      },
      iconDetails: [],
      memberDetails: [],
    };
  },
  methods: {
    formatText(text) {
      return text ? text.replace(/\n/g, "<br>") : "";
    },
    getAvatarSize() {
      if (window.innerWidth < 480) {
        return 120;
      } else if (window.innerWidth < 768) {
        return 150;
      } else if (window.innerWidth < 1024) {
        return 180;
      } else {
        return 240;
      }
    },
    handleResize() {
      this.avatarSize = this.getAvatarSize();
    },
    async fetchaboutInfo() {
      try {
        const introResponse = axios.get(
          "http://localhost:3000/api/intro/getfirst"
        );
        const impactResponse = axios.get(
          "http://localhost:3000/api/impact/getall"
        );
        const memberResponse = axios.get(
          "http://localhost:3000/api/member/getall"
        );

        // Await both responses
        const [intro, impactData, memberdata] = await Promise.all([
          introResponse,
          impactResponse,
          memberResponse,
        ]);
        this.intro = intro.data; // Set data from the awaited intro response
        this.iconDetails = impactData.data; // Set data from the awaited impact response
        this.memberDetails = memberdata.data; // Set data from the awaited impact response

        // Wait for DOM updates
        this.$nextTick(() => {
          this.observeElements();
        });
      } catch (err) {
        this.error = "Failed to fetch contact information";
      }
    },
    observeElements() {
      const elements = document.querySelectorAll(".fade-up");

      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add("show");
            observer.unobserve(entry.target);
          }
        });
      });

      elements.forEach((el) => observer.observe(el));
    },
  },
  mounted() {
    window.addEventListener("resize", this.handleResize);
    this.observeElements();

    this.fetchaboutInfo();
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.handleResize);
  },
  computed: {
    formattedDesc() {
      return this.intro.desc.replace(/\n/g, "<br>");
    },
    chunkedMembers() {
      const result = [];
      const members = [...this.memberDetails];

      // First row with 2 members
      if (members.length > 0) {
        result.push(members.slice(0, 2));

        // Remaining members in groups of 4
        for (let i = 2; i < members.length; i += 4) {
          result.push(members.slice(i, i + 4));
        }
      }

      return result;
    },
    rowSize() {
      return 4; // Number of members per row after first row
    },
  },
};
</script>
<style scoped>
.ab-container {
  padding: 0 1.5rem;
}

.ab-text-1 {
  padding: 2rem 0;
  /* Adjust vertical padding */
}

.ab-text-1 h1 {
  font-size: 4rem;
  /* Default font size for large screens */
  font-weight: 700;
  margin: 0;
}

.ab-section-1 {
  margin: 3% 6%;
  border-radius: 8px;
  overflow: hidden;
  background-color: #fff;
  animation: fadeUp 0.5s ease-out;
  transition-delay: 1s;
  /* Ensure visibility of border-radius */
}

.ab-text-1 {
  /* margin-top: 10%; */
  color: aliceblue;
  align-items: center;
  justify-content: center;
  text-align: center;
  align-content: center;
  gap: 4px;
}

.ab-text-1 h1 {
  font-size: 64px;
  font-weight: 700;
}

.people {
  padding: 0px;
}

.people img {
  width: 100%;
  height: 100%;
}

.ab-section-1-text {
  background-color: #0772ba26;
  align-items: center;
  justify-content: center;
  align-content: center;
}

.ab-text-detail {
  padding: 5px 10%;
}

.ab-text-detail h1 {
  font-weight: 800;
  margin-bottom: 20px;
}

.ab-text-detail p {
  font-size: 1.25rem;
  font-weight: 500;
  margin-bottom: 5%;
}

.ab-section-1-icon {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.ab-inner-detail {
  display: flex;
  flex-direction: row;
  align-items: self-start;

  /* justify-content: space-between; */
  /* text-align: center; */
  gap: 15px;
}

.img-impact {
  max-width: 60px;
}

.ab-inner-detail h1 {
  font-size: 24px;
  margin: 0 0;
  padding: 0 0;
}

.ab-inner-detail p {
  margin: 0 0;
  padding: 0 0;
  font-size: 1rem;
}

.ab-main-3 {
  background-image: url("../assets/effect-ab-bg.png");
  background-size: cover;
  background-position: center;
  height: 680px;
  /* display: flex; */
  /* align-items: center; */
  /* justify-content: center; */
  padding: 2rem 0;
}

.ab-section-2 {
  /* margin: 0 auto; */
  margin: 0% 7%;
  display: flex;
  justify-content: space-between;
  align-items: self-start;
  padding: 0 0;
}

.ab-card-1,
.ab-card-2 {
  background-color: #ffffffb2;
  border-radius: 8px;

  width: 45%;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-radius: 10px;
  font-size: 18px;
}

.ab-card-1 h2,
.ab-card-2 h2 {
  font-size: 1.75rem;
  font-weight: 800;
  padding: 0.5rem 0;
}

.ab-card-1 p,
.ab-card-2 p {
  padding: 2% 0;
  font-size: 22px;
}

.ab-card-1 {
  margin-top: 5%;
}

.ab-card-2 {
  margin-top: 16%;
}

.ab-highlight {
  color: #0772ba;
  font-size: 23px;
  font-weight: 500;
}

.ab-main-4 {
  display: flex;
  flex-direction: column;
  /* align-items: center; */
  text-align: center;
  /* justify-content: center; */
  width: 100vw;
  height: auto;
  text-align: center;
  padding: 2rem 1rem;
  margin-bottom: 10%;
}

.ab-main-4 h2 {
  font-size: 2.5rem;
  font-weight: 900;
  margin-bottom: 2rem;
}

.pic-first-row {
  margin: 0 25%;
}

.member-section {
  width: 100%;
}

.profile-pic {
  text-align: center;
  margin-bottom: 2rem;
}

.profile-pic p {
  margin-top: 10px;
  font-size: 1.25rem;
  font-weight: 600;
}

.profile-pic span {
  display: block;
  font-size: 1rem;
  color: #555;
}

@media (max-width: 1024px) {
  .ab-main-4 h2 {
    font-size: 2rem;
  }

  .member-section v-row {
    margin: 0;
  }

  .profile-pic v-avatar {
    size: 180px;
  }
}

@media (max-width: 768px) {
  .ab-text-1 h1 {
    font-size: 3rem;
    /* Adjust for medium screens */
  }

  .ab-text-detail {
    padding: 5px 3%;
  }

  .ab-text-detail h1 {
    font-size: 20px;
    /* Adjust for smaller screens */
  }

  .ab-text-detail p {
    font-size: 1rem;
  }

  .ad-icon {
    font-size: 200%;
  }

  .ab-icon-detail h1 {
    font-size: 20px;
  }

  .ab-main-3 {
    height: 600px;
    padding: 1.5rem 0;
  }

  .ab-section-2 {
    padding: 0 4%;
    flex-direction: column;
    align-items: center;
  }

  .ab-card-1,
  .ab-card-2 {
    width: 80%;
    padding: 15px;
  }

  .ab-card-2 {
    margin-top: 5%;
  }

  .ab-card-1 h2,
  .ab-card-2 h2 {
    font-size: 1.5rem;
  }

  .ab-card-1 p,
  .ab-card-2 p {
    font-size: 18px;
  }

  .ab-highlight {
    font-size: 21px;
  }

  .ab-main-4 h2 {
    font-size: 1.75rem;
  }

  .profile-pic p {
    font-size: 1.125rem;
  }

  .profile-pic span {
    font-size: 0.875rem;
  }
}

@media (max-width: 480px) {
  .ab-text-1 h1 {
    font-size: 2rem;
    /* Adjust for small screens */
  }

  .ab-text-detail {
    padding: 5px 2%;
  }

  .ab-text-detail h1 {
    font-size: 1.25rem;
  }

  .ab-text-detail p {
    font-size: 1rem;
  }

  .ab-main-3 {
    height: 500px;
    padding: 1rem 0;
  }

  .ab-section-2 {
    padding: 0 2%;
    flex-direction: column;
    align-items: center;
    gap: 20px;
  }

  .ab-card-1,
  .ab-card-2 {
    width: 100%;
    padding: 10px;
  }

  .ab-card-2 {
    margin-top: 3%;
  }

  .ab-card-1 h2,
  .ab-card-2 h2 {
    font-size: 1.25rem;
  }

  .ab-card-1 p,
  .ab-card-2 p {
    font-size: 1rem;
  }

  .ab-highlight {
    font-size: 1.125rem;
  }

  .ab-main-4 h2 {
    font-size: 1.5rem;
  }

  .profile-pic p {
    font-size: 1rem;
  }

  .profile-pic span {
    font-size: 0.75rem;
  }
}

@keyframes fadeUp {
  0% {
    opacity: 0;
    transform: translateY(30px);
    /* Starts 20px below its original position */
  }

  100% {
    opacity: 1;
    transform: translateY(0);
    /* Moves to its original position */
  }
}

.fade-up {
  opacity: 0;
  transform: translateY(100px);
  transition: opacity 0.5s ease-out, transform 0.5s ease-out;
}

.fade-up.show {
  opacity: 1;
  transform: translateY(0);
}

.ab-card-1 {
  transition-delay: 0.5s;
}

.ab-card-2 {
  transition-delay: 0.8s;
}
</style>
