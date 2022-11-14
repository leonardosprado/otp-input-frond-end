<template>
  <div class="main">
    <div ref="otpCont" class="group-input">
      <input
        v-for="(el, ind) in maxlength"
        :key="el + ind"
        class="otpInput"
        size="lg"
        :type="pwdFieldType"
        :id="'password' + ind"
        v-model="digits[ind]"
        @keydown="handleKeyDown($event, ind)"
        maxlength="6"
      />
    </div>
    <div class="d-center">
      <button class="btn" v-on:click="authenticate()" ref="sendReply">
        Verificar
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      model: {
        otp: "",
      },
      pwdFieldType: "number",
      maxlength: 6,
      digits: [],
    };
  },
  mounted(){
      (this.$refs.otpCont.children)[0].focus()
  },
  methods: {
    authenticate(){
      alert("authenticate");
    },
    isDigitsFull() {
      for (const elem of this.digits) {
        if (elem == null || elem == undefined) {
          return false;
        }
      }
      return true;
    },
    handleKeyDown(event, index) {
      var isDigitsFul = this.isDigitsFull();
      if (
        event.key !== "Tab" &&
        event.key !== "ArrowRight" &&
        event.key !== "ArrowLeft" &&
        !event.ctrlKey
      ) {
        event.preventDefault();
      }

      if (event.key === "Delete") {
        this.digits[index] = null;
        // this.$set(this.digits, index, null); //Vue 2
        if (index != this.maxlength - 1) {
          this.$refs.otpCont.children[index + 1].focus();
        }
        return;
      }

      if (event.key === "Backspace") {
        this.digits[index] = null;
        // this.$set(this.digits, index, null); // Vue 2
        if (index != 0) {
          this.$refs.otpCont.children[index - 1].focus();
        }
        return;
      }
      if (new RegExp("^([0-9])$").test(event.key)) {
        // this.$set(this.digits, index, event.key);  // Vue 2
        this.digits[index] = event.key;
        if (index != this.maxlength - 1) {
          this.$refs.otpCont.children[index + 1].focus();
        }
        if (index == this.maxlength - 1) {
          this.$refs.sendReply.focus();
        }
        if (isDigitsFul) {
          this.model.otp = this.digits.join("");
        }
      }
    },
  },
};
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  .main {
    max-width: 1000px;
  }

  input:focus-visible, button:focus-visible  {
    outline-offset: 0px;
    outline: none
  }
  .d-center{
    display: flex;
    justify-content: center;
  }
  .group-input {
    display: flex;
    justify-content: center;
    /* max-width: 300px; */
  }
  /* Chrome, Safari, Edge, Opera */
  .otpInput::-webkit-outer-spin-button,
  .otpInput::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  .otpInput[type="number"] {
    -moz-appearance: textfield;
  }
  .otpInput {
    width: 90px;
    height: 90px;
    margin-right: 10px !important;
    border: 1px solid #cad1d7 !important;
    padding: 0px !important;
    text-align: center;
    border-radius: 6px !important;
    font-size: 20px;
  }
  .btn{
    margin-top: 18px;
     background-color: #CC092F;
    color: white;
    padding-right: 1rem;
    padding-left: 1rem;
    -webkit-box-shadow: 0px 3px 6px #00000029;
    box-shadow: 0px 3px 6px #00000029;
    opacity: 1;
    display: inline-block;
    font-weight: 600;
    text-align: center;
    vertical-align: middle;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    
    border: 1px solid transparent;
    padding: 0.625rem 1.25rem;
    font-size: 0.875rem;
    line-height: 1.5;
    border-radius: 0.375rem;
    -webkit-transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, -webkit-box-shadow 0.15s ease-in-out;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, -webkit-box-shadow 0.15s ease-in-out;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out, -webkit-box-shadow 0.15s ease-in-out;
  }
}
</style>
