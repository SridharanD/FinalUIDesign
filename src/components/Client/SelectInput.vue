<template>
  <md-field :class="error && 'md-invalid'">
    <md-icon v-if="icon" class="input-icon">{{ icon }}</md-icon>
    <md-select
      v-model="localValue"
      :required="required"
      :name="name"
      :placeholder="displayName"
      :disabled="disabled"
    >
      <!-- <md-option value="" v-if="displayName"> {{ displayName }} </md-option> -->
      <md-option
        v-for="(option, index) in options"
        :key="index"
        :value="option.value"
        >{{ option.text }}</md-option
      >
    </md-select>
    <md-tooltip v-if="tooltip" md-direction="right">{{ tooltip }}</md-tooltip>
    <span class="md-error" style="text-align: left">{{ error }}</span>
  </md-field>
</template>
<script>
export default {
  name: "SelectInput",
  props: {
    name: String,
    options: Array,
    displayName: String,
    value: [String, Number],
    error: String,
    required: Boolean,
    autofocus: Boolean,
    icon: String,
    tooltip: String,
    placeHolder: String,
    disabled: Boolean,
  },
  computed: {
    localValue: {
      get: function () {
        return this.value;
      },
      set: function (val) {
        this.$emit("input", val);
      },
    },
  },
};
</script>
