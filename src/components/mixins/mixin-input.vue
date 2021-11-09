<script>
import debounce from "../../helpers/debounce";

export default {
  name: "mixin-input",
  props: {
    required: {
      type: Boolean,
      default: true,
    },
    label: {
      type: String,
      default: "Label",
    },
    value: {
      type: String,
      default: "",
    },
    inputType: {
      type: String,
      default: "text",
    },
    autofocus: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    errorText: {
      type: String,
      default: "",
    },
    rule: {
      type: RegExp,
    },
  },
  data() {
    return {
      focused: this.autofocus,
      error: "",
      debouncedValidation: null,
      valid: true,
    };
  },
  created() {
    this.debouncedValidation = debounce(this.validate, 1000, false);
  },
  computed: {
    errorMessage() {
      let message = "";
      if (!this.valid && this.required) {
        message = this.value ? this.errorText : "обязательное поле";
      }
      return message;
    },
    isActive() {
      return (!this.disabled && this.focused) || this.value;
    },
    inputClasses() {
      return [
        "input-wrapper",
        {
          "input-wrapper_focused": this.focused,
          "input-wrapper_error": !this.valid,
        },
      ];
    },
    showPlaceholder() {
      return this.$attrs.placeholder && this.$attrs.placeholder.length;
    },
    placeholderClasses() {
      return [
        "input-placeholder",
        {
          "input-placeholder_lifted":
            this.isActive || this.inputType === "date",
        },
      ];
    },
  },
  methods: {
    inputHandler(event) {
      this.valid = true;
      this.$emit("input", event.target.value);
      this.debouncedValidation(event.target.value);
    },
    focusHandler() {
      this.focused = true;
    },
    validate(value) {
      if (this.rule) {
        if (this.rule.test(value)) {
          this.$emit("valid");
          this.valid = true;
        } else {
          this.$emit("not-valid");
          if (!(this.value === "" && !this.required)) {
            this.valid = false;
          }
        }
      } else {
        if (value) {
          this.$emit("valid");
          this.valid = true;
        } else {
          this.$emit("not-valid");
          if (this.required) {
            this.valid = false;
          }
        }
      }
    },
    blurHandler() {
      this.focused = false;
      this.validate(this.value);
    },
  },
};
</script>

<style scoped>
.error {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  height: 16px;
  margin-left: 5px;
  color: salmon;
  font-size: 10px;
}
.input-wrapper {
  position: relative;
  display: inline-flex;
  width: 100%;
  border-radius: 10px;
  border: 2px solid dimgray;
  text-overflow: ellipsis;
  overflow: hidden;
}

.input-wrapper_focused {
  border: 2px solid dodgerblue;
  overflow: unset;
}

.input-wrapper_error {
  border: 2px solid salmon;
  color: salmon;
}

.my-input {
  width: 100%;
  height: 30px;
  padding: 6px 16px 6px 6px;
  box-sizing: border-box;
  border: 0;
  outline: 0;
  background-color: transparent;
}

.input-placeholder {
  position: absolute;
  display: flex;
  padding: 6px 20px 6px 6px;
  width: 100%;
  color: #8d95a5;
  white-space: nowrap;
  text-overflow: ellipsis;
  pointer-events: none;
  transform-origin: left top;
  transition: 300ms;
  box-sizing: border-box;
}

.placeholder-text {
  text-overflow: ellipsis;
  overflow: hidden;
}

.input-placeholder_lifted {
  transform: translateY(-20px) scale(0.75);
}
.icon {
  height: 1.1rem;
  width: 1.1rem;
  padding: 6px;
}
</style>
