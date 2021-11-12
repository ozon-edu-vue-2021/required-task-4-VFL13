<template>
  <div>
    <div class="row">
      <MyInput
        :required="false"
        class="row-item"
        v-model="user.lastName"
        placeholder="Фамилия"
        :errorText="rules.onlyRussian.text"
        :rule="rules.onlyRussian.rule"
        @not-valid="userValidation.lastName = false"
        @valid="userValidation.lastName = true"
      />
      <MyInput
        :required="false"
        class="row-item"
        v-model="user.firstName"
        placeholder="Имя"
        :errorText="rules.onlyRussian.text"
        :rule="rules.onlyRussian.rule"
        @not-valid="userValidation.firstName = false"
        @valid="userValidation.firstName = true"
      />
    </div>
  </div>
</template>

<script>
import MyInput from "./MyInput";
export default {
  name: "ChangedLastName",
  components: { MyInput },
  data() {
    return {
      user: {
        lastName: "",
        firstName: "",
      },
      userValidation: {
        lastName: false,
        firstName: false,
      },
      rules: {
        onlyRussian: {
          rule: /^[А-ЯЁа-яё -]+$/,
          text: "допустимы только русские буквы",
        },
      },
    };
  },
  computed: {
    formValid() {
      return this.userValidation.firstName || this.userValidation.lastName;
    },
  },
  watch: {
    formValid: {
      handler: function (val) {
        this.$emit("valid", val);
        if (val) {
          this.$emit("setData", this.user);
        }
      },
    },
  },
};
</script>

<style scoped></style>
