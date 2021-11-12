<template>
  <div class="form">
    <Personal @valid="setValidPersonal" @setUser="setUser" />
    <Passport @valid="setValidDocument" @setPassport="setPassport" />
    <div class="row">
      <button
        @click="sendData()"
        class="row-item button"
        :disabled="!allDataValid"
        type="submit"
      >
        Отправить
      </button>
    </div>
  </div>
</template>

<script>
import Personal from "./Personal";
import Passport from "./Passport";

export default {
  components: {
    Passport,
    Personal,
  },
  data() {
    return {
      userValid: false,
      documentValid: false,
      serverError: null,
      user: null,
      passport: null,
    };
  },
  computed: {
    allDataValid() {
      return this.userValid && this.documentValid;
    },
  },
  methods: {
    setValidPersonal(value) {
      this.userValid = value;
    },
    setValidDocument(value) {
      this.documentValid = value;
    },
    setUser(user) {
      this.user = user;
    },
    setPassport(passport) {
      this.passport = passport;
    },
    sendData() {
      if (this.allDataValid) {
        console.info("FORM DATA:", {
          user: this.user,
          passport: this.passport,
        });
      } else {
        alert("Форма не заполнена");
      }
    },
  },
};
</script>

<style>
.button {
  background-color: dodgerblue;
  border-radius: 10px;
  border: none;
  color: white;
  padding: 6px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}

.button:hover {
  background-color: #4da6ff;
}

.button:disabled,
.button[disabled] {
  background: #8d95a5;
}
.row {
  display: flex;
}
.row-item {
  margin: 6px;
}
form {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 60px;
  margin: 0;
}
</style>
