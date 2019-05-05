<template>
  <div class="List">
    <ol class="List__list uk-margin-bottom-remove">
      <li
        v-for="(item, index) in model.items"
        :key="index"
        class="List__list-item"
      >
        <div class="uk-flex uk-flex-middle">
          <input
            v-model="model.items[index].text"
            class="uk-form-small uk-width-1-1"
            :aria-label="`List item ${index}`"
          >
          <a
            class="assets__item-trash"
            aria-label="Remove item"
            @click="removeItem(index)"
          >
            <i class="uk-icon-minus-circle"></i>
          </a>
        </div>

        <ol class="List__children uk-margin-bottom-remove">
          <li
            v-for="(item, childIndex) in model.items[index].children"
            :key="childIndex"
            class="List__children-item uk-flex uk-flex-middle"
          >
            <input
              v-model="model.items[index].children[childIndex]"
              class="uk-form-small uk-width-1-1"
              :aria-label="`Child ${childIndex} of list item ${index}`"
            >
            <a
              class="assets__item-trash"
              aria-label="Remove item"
              @click="removeChild(childIndex, model.items[index])"
            >
              <i class="uk-icon-minus-circle"></i>
            </a>
          </li>
        </ol>
        <a
          class="blok__small-btn uk-flex uk-flex-right uk-margin-small-top"
          @click="addChild(model.items[index])"
        >
          <i class="uk-icon-plus-circle uk-margin-small-right"/>
            Add Child
        </a>
      </li>
    </ol>
    <a
      v-if="!limitReached"
      class="blok__full-btn uk-margin-small-top"
      @click="addItem"
    >
      <i class="uk-icon-plus-circle uk-margin-small-right"/>
        Add item
    </a>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  computed: {
    limitReached() {
      return this.options.limit && this.model.items.length >= this.options.limit;
    },
  },
  watch: {
    model: {
      deep: true,
      handler(value) {
        this.$emit('changed-model', value);
      },
    },
  },
  methods: {
    initWith() {
      return {
        items: [
          {
            text: '',
            children: []
          }
        ],
        plugin: 'list-marcroemmelt',
      };
    },
    addItem() {
      this.model.items.push(
        {
          text: '',
          children: []
        }
      );
    },
    addChild(listItem) {
      listItem.children.push('');
    },
    removeItem(index) {
      this.model.items = this.model.items.filter((_, i) => i !== index);
    },
    removeChild(childIndex, listItem) {
      listItem.children = listItem.children.filter((_, i) => i !== childIndex);
    }
  },
}
</script>

<style>
.List__list {
  padding-left: 0;
}
.List__list-item {
  margin-bottom: 10px;
}
.List__list-item + .List__list-item {
  margin-top: 5px;
}
.List__children {
  padding-left: 24px;
  padding-right: 24px;
}
.List__children-item {
  margin-top: 5px;
}
.List__children-item > input {
  font-size: 0.5em;
}
</style>
