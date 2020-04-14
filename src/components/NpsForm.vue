<template>
  <section class="main-content">
    <div class="container">
      <transition name="nps">
        <div v-if="this.validationError" class="validation-error">
          <p>
            Please fill up the requied
            <span>(*)</span> fields
          </p>
        </div>
      </transition>
      <div v-if="!submitted" class="servey_form">
        <div class="input-group">
          <label>
            1. On a scale of one to ten, how likely are you to recommend our product to a friend or colleague?
            <span
              class="required"
            >*</span>
          </label>
          <div class="product_score">
            <button
              :key="item"
              v-for="item in [1,2,3,4,5,6,7,8,9,10]"
              :class="{'active': servey.productScore === item }"
              @click="productScore(item)"
            >{{item}}</button>
          </div>
        </div>
        <div class="input-group">
          <label>
            2. How satisfied were you with our service?
            <span class="required">*</span>
          </label>
          <div class="service_score">
            <button
              :key="item"
              v-for="item in ['very unsatisfied','unsatified','satisfied','very satisfied']"
              @click="serviceScore(item)"
              :class="{'active-service': servey.serviceScore === item }"
            >{{item}}</button>
          </div>
        </div>
        <div class="input-group">
          <label>3. How can we improve our product?</label>
          <textarea v-model="servey.suggestion" placeholder="Type your suggestion ..."></textarea>
        </div>
        <div class="input-group">
          <h3>
            Please leave your contact information
            <span class="required">*</span>
          </h3>
          <div class="info-group">
            <label>Name:</label>
            <input v-model="servey.clientName" type="text" />
          </div>
          <div class="info-group">
            <label>Email:</label>
            <input v-model="servey.clientEmail" type="email" />
          </div>
        </div>
        <button @click="submit()" class="submit">Submit</button>
      </div>

      <div class="confirmation">
        <transition name="nps">
          <div v-if="submitted" class="success">
            <svg
              width="94"
              height="94"
              viewBox="0 0 94 94"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M47 0C21.0386 0 0 21.0441 0 47C0 72.9588 21.0412 94 47 94C72.9559 94 93.9971 72.9588 93.9971 47C93.9971 21.0471 72.9559 0 47 0ZM47 85.1874C25.9089 85.1874 8.81264 68.0911 8.81264 47C8.81264 25.9089 25.9118 8.81264 47 8.81264C68.0882 8.81264 85.1844 25.9089 85.1844 47C85.1844 68.0911 68.0885 85.1874 47 85.1874ZM65.4711 27.0074C63.4735 25.8149 60.9209 26.5227 59.7724 28.5936L44.0126 56.9023L35.2355 46.2186C33.6463 44.3885 31.5343 43.4399 29.5397 44.6324C27.5421 45.8279 26.7166 48.7038 28.0093 50.5487L40.2234 65.4153C41.137 66.6019 42.1796 67.313 43.2696 67.5157L43.2785 67.5335L43.3902 67.5394C43.637 67.5775 47.3266 68.259 48.9158 65.4156L67.0051 32.924C68.1529 30.8495 67.4688 28.2029 65.4711 27.0074Z"
                fill="#3DB39E"
              />
            </svg>

            <h3>Thank you for your servey!</h3>
          </div>
        </transition>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "NpsForm",
  data() {
    return {
      validationError: false,
      savedServeys: [],
      servey: {
        productScore: null,
        serviceScore: null,
        suggestion: null,
        clientName: null,
        clientEmail: null,
        createAt: new Date().getTime()
      },
      serveys: [
        {
          productScore: 7,
          serviceScore: "satisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "tanvirh27@gmail.com",
          createAt: 1586304000000
        },
        {
          productScore: 8,
          serviceScore: "unsatisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "user01@gmail.com",
          createAt: 1586390400000
        },
        {
          productScore: 10,
          serviceScore: "very satisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "user02@gmail.com",
          createAt: 1586509200000
        },
        {
          productScore: 3,
          serviceScore: "very unsatisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "dummyuser03@gmail.com",
          createAt: 1586595600000
        },
        {
          productScore: 6,
          serviceScore: "satisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "dummyuser04@gmail.com",
          createAt: 1586649600000
        },
        {
          productScore: 5,
          serviceScore: "satisfied",
          suggestion: "test",
          clientName: "tanvir",
          clientEmail: "user05@gmail.com",
          createAt: 1586736000000
        }
      ],
      submitted: false
    };
  },

  mounted() {
    this.savedServeys = JSON.parse(localStorage.getItem("servey"));
    if (!this.savedServeys) {
      this.savedServeys = [...this.serveys];
      localStorage.setItem("servey", JSON.stringify(this.savedServeys));
    }
  },

  methods: {
    /**
     * this method is responsible for form validation
     *
     */
    formValidation() {
      if (
        !this.servey.productScore ||
        !this.servey.productScore ||
        !this.servey.clientName ||
        !this.servey.clientEmail
      ) {
        return false;
      }
    },

    /**
     * this method resets form data
     *
     */
    resetForm() {
      this.servey.suggestion = "";
      this.servey.clientName = "";
      this.servey.clientEmail = "";
    },

    /**
     * this method is responsible for pushing product score to servey list
     *
     */
    productScore(item) {
      this.servey.productScore = item;
      this.isActive = true;
    },

    /**
     * this method is responsible for pushing servuce score to servey list
     *
     */
    serviceScore(item) {
      this.servey.serviceScore = item;
    },

    /**
     * this method is responsible form submission
     *
     */
    submit() {
      if (this.formValidation() == false) {
        this.validationError = true;
      } else {
        this.serveys = JSON.parse(localStorage.getItem("servey"));
        this.savedServeys.push({
          ...this.servey,
          ...{ createAt: new Date().getTime() }
        });
        localStorage.setItem("servey", JSON.stringify(this.savedServeys));
        this.resetForm();
        this.validationError = false;
        this.submitted = true;
      }
    }
  }
};
</script>

<style lang="scss">
label {
  display: block;
  font-weight: bold;
  line-height: 21px;
  margin-bottom: 20px;
}
textarea {
  outline: none;
  resize: none;
  width: 50%;
  height: 100px;
  padding-left: 10px;
  padding-top: 10px;
  border: 1px solid #e9e9e9;
  border-radius: 5px;
  &::placeholder {
    font-family: "Roboto", sans-serif;
    font-weight: 300;
  }
}
.input-group {
  padding: 10px 0;
  margin-bottom: 10px;
}
.info-group {
  padding-top: 20px;
  margin-bottom: 10px;
  label {
    display: inline-block;
    margin-right: 10px;
  }
  input {
    width: 40%;
    height: 30px;
    border-radius: 5px;
    border: 1px solid #e9e9e9;
    padding-left: 10px;
  }
}

.product_score {
  button {
    width: 54px;
    height: 41px;
    margin-right: 10px;
    background-color: #fff;
    border: 1px solid #fcfcfc;
    box-sizing: border-box;
    box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.25);
    border-radius: 2px;
    margin-bottom: 10px;
    &:hover {
      box-shadow: 1px 0px 9px rgba(48, 48, 48, 0.34);
      transition: box-shadow 0.2s;
    }
    &:focus {
      outline: none;
    }
    .active {
      background-color: #78c38a;
      color: #fff;
      border-radius: 2px;
      border: none;
      box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
    }
  }
}

.service_score {
  button {
    box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.25);
    border-radius: 5px;
    width: 170px;
    height: 70px;
    margin-bottom: 10px;
    border: none;
    margin-right: 25px;
    font-size: 18px;
    line-height: 21px;
    color: #ffffff;
    text-transform: capitalize;
    &:hover {
      box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24) !important;
      transition: all 0.3s;
    }
    &:focus {
      outline: none;
    }
  }
  button:nth-child(1) {
    background: #e8847c;
  }
  button:nth-child(2) {
    background: #e8a66c;
  }
  button:nth-child(3) {
    background: #b9c465;
  }
  button:nth-child(4) {
    background: #78c38a;
  }
}
.active-service {
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24) !important;
}
.submit {
  width: 150px;
  height: 50px;
  background-color: #663a82;
  border: 1px solid #663a82;
  border-radius: 5px;
  color: #fff;
  font-weight: 500;
  font-size: 18px;
  line-height: 21px;
}
.required {
  color: #a52a2a;
}
.validation-error {
  background-color: #f8d7da;
  padding: 3px 15px;
  border-radius: 5px;
  margin-bottom: 20px;
  p {
    span {
      color: #a52a2a;
    }
  }
}

.main-content {
  padding: 40px 0;
}

.confirmation {
  width: 50%;
  margin: 0 auto;
}
.success {
  text-align: center;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24),
    0px 0px 2px rgba(0, 0, 0, 0.12);
  border-radius: 4px;
  padding: 80px 0;
  margin-top: 30px;
}

button.active {
  background-color: #78c38a;
  color: #fff;
  border-radius: 2px;
  border: none;
}

.nps-enter {
  transform: translateX(10px);
  opacity: 0;
}
.nps-enter-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}
.nps-leave-active,
.nps-leave-to {
  opacity: 0;
}
</style>