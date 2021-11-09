<template>
  <div>
    <div class="row">
      <h3 class="row-item">Паспортные данные</h3>
    </div>
    <div class="row">
      <MySelect
        label="Гражданство"
        :items="citizenships"
        prop-name="nationality"
        v-model="passport.citizenships"
        @not-valid="passportValidation.citizenships = false"
        @valid="passportValidation.citizenships = true"
        class="row-item"
      />
    </div>
    <template v-if="isRussian">
      <PassportRussian
        @valid="setValidRussianPassport"
        @setData="setDataRussianPassport"
      />
    </template>
    <template v-else>
      <PassportOtherCountry
        @valid="setValidOtherCountryPassport"
        @setData="setDataOtherCountryPassport"
      />
    </template>

    <div class="row">
      <h5 class="row-item">Меняли ли вы фамилию или имя</h5>
    </div>

    <div class="row">
      <MyRadioInput
        class="row-item"
        @input="toggleHasChangeLastName()"
        label="Нет"
        :checked="!passport.hasChangeLastName"
      />
      <MyRadioInput
        class="row-item"
        @input="toggleHasChangeLastName()"
        label="Да"
        :checked="passport.hasChangeLastName"
      />
    </div>
    <template v-if="passport.hasChangeLastName">
      <ChangedLastName
        @valid="setValidChangedLastName"
        @setData="setDataChangedLastName"
      />
    </template>
  </div>
</template>

<script>
import MySelect from "./MySelect";
import citizenships from "../assets/data/citizenships.json";
import PassportRussian from "./PassportRussian";
import PassportOtherCountry from "./PassportOtherCountry";
import MyRadioInput from "./MyRadioInput";
import ChangedLastName from "./ChangedLastName";

export default {
  name: "Passport",
  components: {
    ChangedLastName,
    MyRadioInput,
    PassportOtherCountry,
    PassportRussian,
    MySelect,
  },
  data() {
    return {
      citizenships: null,
      passport: {
        citizenships: "",
        russian: null,
        otherCountry: null,
        hasChangeLastName: false,
        changedLastName: null,
      },
      passportValidation: {
        citizenships: false,
        russian: false,
        otherCountry: false,
        changedLastName: false,
      },
    };
  },
  created() {
    this.citizenships = citizenships;
  },
  computed: {
    isRussian() {
      return !(
        this.passportValidation.citizenships &&
        this.passport.citizenships !== "Russia"
      );
    },
    changedLastNameIsValid() {
      return (
        this.passportValidation.changedLastName ||
        !this.passport.hasChangeLastName
      );
    },
    formValid() {
      if (
        ((this.isRussian && this.passportValidation.russian) ||
          (!this.isRussian && this.passportValidation.otherCountry)) &&
        this.changedLastNameIsValid &&
        this.passportValidation.citizenships
      ) {
        return true;
      }
      return false;
    },
  },
  methods: {
    setValidRussianPassport(valid) {
      this.passportValidation.russian = valid;
    },
    setDataRussianPassport(data) {
      this.passport.russian = data;
    },
    setValidOtherCountryPassport(valid) {
      this.passportValidation.otherCountry = valid;
    },
    setDataOtherCountryPassport(data) {
      this.passport.otherCountry = data;
    },
    toggleHasChangeLastName() {
      this.passport.hasChangeLastName = !this.passport.hasChangeLastName;
    },
    setValidChangedLastName(valid) {
      this.passportValidation.changedLastName = valid;
    },
    setDataChangedLastName(data) {
      this.passport.changedLastName = data;
    },
  },
  watch: {
    formValid: {
      handler: function (val) {
        this.$emit("valid", val);
        if (val) {
          this.$emit("setPassport", this.passport);
        }
      },
    },
  },
};
</script>

<style scoped></style>
