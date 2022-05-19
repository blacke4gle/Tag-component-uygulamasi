<template>
  <div class="tag-container">
    <Tag
      v-for="(tag, index) in tags"
      :key="tag"
      :tag="tag"
      :index="index"
      :tagColor="color"
      @removeOneTagEvent="removeOneTag($event)"
    />
    <input type="text" @keydown.enter="addTag" @keydown.backspace="removeTag" />
    <div class="error" v-if="error">Bu etiket daha önce eklenmiş!!!</div>
  </div>
</template>

<script>
import Tag from "./Tag";
export default {
  components: {
    Tag,
  },
  data() {
    return {
      tags: [],
      error: false,
    };
  },
  methods: {
    addTag(event) {
      let text = event.target;
      let matchedTag = false;

      if (text.value.length > 0) {
        this.tags.forEach((tag) => {
          if (tag.toLowerCase() === text.value.toLowerCase()) {
            matchedTag = true;
          }
        });
        if (!matchedTag) {
          this.tags.push(text.value);

          text.value = "";
        } else {
          this.error = true;
          setTimeout(() => {
            this.error = false;
          }, 1500);
        }
      }
    },
    removeTag(e) {
      if (e.target.value.length <= 0) {
        this.tags.splice(this.tags.length - 1, 1);
      }
    },
    removeOneTag(index) {
      this.tags.splice(index, 1);
    },
  },
  props: {
    value: {
      required: false,
    },
    color: {
      type: String,
      required: false,
      default: "primary",
    },
  },
  created() {
    if (this.value) {
      if (this.value.length > 0) {
        //deneme,test
        this.tags = this.value.split(",");
      }
    }
  },
  watch: {
    tags() {
      this.$emit("input", this.tags.join(","));
    },
  },
};
</script>

<style scoped>
.tag-container {
  border: 1px solid rgb(214, 214, 214);
  padding: 20px;
  border-radius: 3px;
}
input {
  outline: none;
  height: 30px;
  width: 100px;
}
.error {
  font-size: 15px;
  font-family: tahoma;
  color: red;
  margin-top: 7px;
  font-weight: 300;
}
</style>
