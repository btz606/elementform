<template>
  <div>
    <label for="">{{label}}</label>
    <div>
      <slot></slot>
      <p v-if="errStatus">{{errMessage}}</p>
    </div>
  </div>
</template>

<script>
import Schema from "async-validator";

export default {
  inject: ["kForm"],
  props: ["label", "prop"],
  methods: {},
  data() {
    return {
      errMessage: "",
      errStatus: false
    };
  },
  mounted() {
    this.$on("validate", this.validator);
  },
  methods: {
    validator() {
      //   console.log("开始校验了");s
      // 校验逻辑
      const rules = this.kForm.rules[this.prop];
      const value = this.kForm.model[this.prop];
      const descriptor = { [this.prop]: rules };
      // 动态计算属性，上面的这一行

      // 必须有描述对象，插件要求
      const schema = new Schema(descriptor);
      schema.validate({ [this.prop]: value }, errors => {
        if (errors) {
          this.errMessage = errors[0].message;
          this.errStatus = true;
        } else {
          this.errMessage = "";
          this.errStatus = "";
        }
      });
    }
  }
};
</script>

<style>
</style>
