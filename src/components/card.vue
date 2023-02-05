<template>
  <div @click="toggleOpen" id="frame">
    <i v-if="keepOpen" :class="'fa-solid fa-' + iconName"></i>
    <i v-else class="fa-solid fa-question"></i>
  </div>
</template>

<script>

export default {
  name: "card",
  props: {
    iconName: {
      required: true,
      type: String,
    },
    index: {
      required: true,
      type: Number,
    },
    keepOpen: {
      required: false,
      default: false,
      type: Boolean,
    },
  },
  data() {
    return {
      opened: this.keepOpen,
    };
  },
  methods: {
    toggleOpen() {
      if (!this.keepOpen) {
        //   this.opened = this.$emit("check", this.iconName);
        this.$emit("check", { index: this.index, name: this.iconName });
      }
    },
  },
};
</script>

<style>
#frame {
  display: inline-block;
  background-color: rgb(224, 104, 13);
  padding: 5px;
  min-width: 70px;
  min-height: 70px;
  max-width: 70px;
  max-height: 70px;
  border-radius: 5%;

}
i {
  color: #000;
  font-size: 54px;
  cursor: pointer;
}
</style>
