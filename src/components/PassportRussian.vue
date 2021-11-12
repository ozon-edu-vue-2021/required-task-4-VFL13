<template>
  <div>
    <div class="row">
      <MyInput
        class="row-item"
        v-model="russian.series"
        placeholder="Серия"
        :errorText="rules.only4Digit.text"
        :rule="rules.only4Digit.rule"
        @not-valid="russianValidation.series = false"
        @valid="russianValidation.series = true"
      />
      <MyInput
        class="row-item"
        v-model="russian.number"
        placeholder="Номер"
        :errorText="rules.only6Digit.text"
        :rule="rules.only6Digit.rule"
        @not-valid="russianValidation.number = false"
        @valid="russianValidation.number = true"
      />
      <MyInputDate
        class="row-item"
        inputType="date"
        min="1900-01-01"
        :max="new Date().toJSON().slice(0, 10)"
        v-model="russian.date"
        placeholder="Дата выдачи"
        :errorText="rules.date.text"
        :rule="rules.date.rule"
        @not-valid="russianValidation.date = false"
        @valid="russianValidation.date = true"
      />
    </div>
  </div>
</template>

<script>
import MyInput from "./MyInput";
import MyInputDate from "./MyInputDate";

export default {
  name: "PassportRussian",
  components: {
    MyInput,
    MyInputDate,
  },
  data() {
    return {
      russian: {
        series: "",
        number: "",
        date: "",
      },
      russianValidation: {
        series: false,
        number: false,
        date: false,
      },
      rules: {
        date: {
          rule: /^(\d{4})(\/|-)(0?[1-9]|1[012])(\/|-)(0?[1-9]|[12][0-9]|3[01])$/,
          text: "не верная дата",
        },
        only4Digit: {
          rule: /^\d{4}$/,
          text: "4 цифры",
        },
        only6Digit: {
          rule: /^\d{6}$/,
          text: "6 цифр",
        },
      },
    };
  },
  computed: {
    formValid() {
      return (
        this.russianValidation.series &&
        this.russianValidation.number &&
        this.russianValidation.date
      );
    },
  },
  watch: {
    formValid: {
      handler: function (val) {
        this.$emit("valid", val);
        if (val) {
          this.$emit("setData", this.russian);
        }
      },
    },
  },
};
</script>

<style scoped></style>
