<template>
  <div>
    <div class="row">
      <h3 class="row-item">Личные данные</h3>
    </div>
    <div class="row">
      <MyInput
        class="row-item"
        v-model="user.lastName"
        placeholder="Фамилия"
        :errorText="rules.onlyRussian.text"
        :rule="rules.onlyRussian.rule"
        @not-valid="userValidation.lastName = false"
        @valid="userValidation.lastName = true"
      />
      <MyInput
        class="row-item"
        v-model="user.firstName"
        placeholder="Имя"
        :errorText="rules.onlyRussian.text"
        :rule="rules.onlyRussian.rule"
        @not-valid="userValidation.firstName = false"
        @valid="userValidation.firstName = true"
      />
      <MyInput
        class="row-item"
        v-model="user.patronymic"
        placeholder="Отчество"
        :errorText="rules.onlyRussian.text"
        :rule="rules.onlyRussian.rule"
        @not-valid="userValidation.patronymic = false"
        @valid="userValidation.patronymic = true"
      />
    </div>
    <div class="row">
      <MyInputDate
        class="row-item"
        inputType="date"
        min="1900-01-01"
        :max="new Date().toJSON().slice(0, 10)"
        v-model="user.birthDate"
        placeholder="Дата рождения"
        :errorText="rules.date.text"
        :rule="rules.date.rule"
        @not-valid="userValidation.birthDate = false"
        @valid="userValidation.birthDate = true"
      />
    </div>
    <div class="row">
      <MyInput
        class="row-item"
        v-model="user.email"
        placeholder="E-mail"
        :errorText="rules.email.text"
        :rule="rules.email.rule"
        @not-valid="userValidation.email = false"
        @valid="userValidation.email = true"
      />
    </div>
    <div class="row">
      <h5 class="row-item">Пол</h5>
    </div>
    <div class="row">
      <MyRadioInput
        class="row-item"
        @input="toggleGender()"
        label="Мужской"
        :checked="genderChecked"
      />
      <MyRadioInput
        class="row-item"
        @input="toggleGender()"
        label="Женский"
        :checked="!genderChecked"
      />
    </div>
  </div>
</template>

<script>
import MyInput from "./MyInput";
import MyRadioInput from "./MyRadioInput";
import MyInputDate from "./MyInputDate";

export default {
  name: "Personal",
  components: {
    MyInput,
    MyInputDate,
    MyRadioInput,
  },
  data() {
    return {
      user: {
        lastName: "",
        firstName: "",
        patronymic: "",
        birthDate: "",
        email: "",
        gender: "male",
      },
      userValidation: {
        lastName: false,
        firstName: false,
        patronymic: false,
        birthDate: false,
        email: false,
      },
      rules: {
        onlyRussian: {
          rule: /^[А-ЯЁа-яё -]+$/,
          text: "допустимы только русские буквы",
        },
        email: {
          rule: /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,9})+$/,
          text: "неверный e-mail",
        },
        date: {
          rule: /^(\d{4})(\/|-)(0?[1-9]|1[012])(\/|-)(0?[1-9]|[12][0-9]|3[01])$/,
          text: "не верная дата",
        },
      },
    };
  },
  computed: {
    genderChecked() {
      return this.user.gender === "male";
    },
    formValid() {
      let key = "";
      for (key of Object.keys(this.userValidation)) {
        if (!this.userValidation[key]) {
          return false;
        }
      }
      return true;
    },
  },
  methods: {
    toggleGender() {
      this.user.gender = this.user.gender === "male" ? "female" : "male";
    },
  },
  watch: {
    formValid: {
      handler: function (val) {
        this.$emit("valid", val);
        if (val) {
          this.$emit("setUser", this.user);
        }
      },
    },
  },
};
</script>

<style scoped></style>
