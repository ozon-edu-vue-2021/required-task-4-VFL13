<template>
  <div>
    <div class="row">
      <MyInput
        class="row-item"
        v-model="otherCountry.lastName"
        placeholder="Фамилия на латинице"
        :errorText="rules.onlyLatin.text"
        :rule="rules.onlyLatin.rule"
        @not-valid="passportValidation.lastName = false"
        @valid="passportValidation.lastName = true"
      />
      <MyInput
        class="row-item"
        v-model="otherCountry.firstName"
        placeholder="Имя на латинице"
        :errorText="rules.onlyLatin.text"
        :rule="rules.onlyLatin.rule"
        @not-valid="passportValidation.firstName = false"
        @valid="passportValidation.firstName = true"
      />
    </div>
    <div class="row">
      <MyInput
        class="row-item"
        v-model="otherCountry.number"
        placeholder="Номер паспорта"
        @not-valid="passportValidation.number = false"
        @valid="passportValidation.number = true"
      />
      <MySelect
        label="Страна выдачи"
        :items="citizenships"
        prop-name="nationality"
        v-model="otherCountry.country"
        @not-valid="passportValidation.country = false"
        @valid="passportValidation.country = true"
        class="row-item"
      />
      <MySelect
        label="Тип паспорта"
        :items="passportTypes"
        prop-name="type"
        v-model="otherCountry.passportType"
        @not-valid="passportValidation.passportType = false"
        @valid="passportValidation.passportType = true"
        class="row-item"
      />
    </div>
  </div>
</template>

<script>
import citizenships from "../assets/data/citizenships.json";
import passportTypes from "../assets/data/passport-types.json";
import MyInput from "./MyInput";
import MySelect from "./MySelect";

export default {
  name: "PassportOtherCountry",
  components: { MySelect, MyInput },
  data() {
    return {
      citizenships: null,
      passportTypes: null,
      otherCountry: {
        lastName: "",
        firstName: "",
        number: "",
        country: "",
        passportType: "",
      },
      passportValidation: {
        lastName: false,
        firstName: false,
        number: false,
        country: false,
        passportType: false,
      },
      rules: {
        onlyLatin: {
          rule: /^[A-Za-z -]+$/,
          text: "допустимы только латинские буквы",
        },
      },
    };
  },
  created() {
    this.citizenships = citizenships;
    this.passportTypes = passportTypes;
  },
  computed: {
    formValid() {
      if (
        this.passportValidation.number &&
        this.passportValidation.country &&
        this.passportValidation.passportType &&
        this.passportValidation.lastName &&
        this.passportValidation.firstName
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
  methods: {},
  watch: {
    formValid: {
      handler: function (val) {
        this.$emit("valid", val);
        if (val) {
          this.$emit("setData", this.otherCountry);
        }
      },
    },
  },
};
</script>

<style scoped></style>
