<template>
  <div class="component-base-filter">
    <h3 class="filter-title">{{ filter }}</h3>
    <div class="filter-wrapper">
      <button
        :class="important ? 'important' : ''"
        class="button"
        @click="open">
        <span
          :class="selected ? 'is-active' : ''"
          class="selected-filter">{{ $options.filters.capitalize(selected) || allText }}</span>
        <span class="arrow-wrapper"><SvgIconChevron
          :width="11"
          :height="11"/></span>
      </button>
      <transition name="fade">
        <div
          v-on-clickaway="close"
          v-if="dropdown"
          class="dropdown">
          <h4
            class="dropdown-title"
            @click="close">{{ title }}</h4>
          <ul class="option-list">
            <li
              class="option-item"
              role="button"
              @click="select('')"
            ><strong>{{ allText }}</strong></li>
            <li
              v-for="(option, index) in options"
              :key="index"
              class="option-item"
              role="button"
              @click="select(option.selection)">
              {{ option.text }}
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { directive as onClickaway } from 'vue-clickaway'
import SvgIconChevron from './SvgIconChevron'

export default {
  components: {
    SvgIconChevron
  },
  directives: {
    onClickaway: onClickaway
  },
  props: {
    allText: {
      type: String,
      required: true
    },
    filter: {
      type: String,
      required: true
    },
    important: {
      type: Boolean,
      default: false
    },
    options: {
      type: Array,
      required: true
    },
    selected: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      dropdown: false
    }
  },
  methods: {
    open() {
      this.dropdown = true
    },
    close() {
      this.dropdown = false
    },
    select(selection) {
      this.close()
      this.$emit('select', selection)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~assets/css/settings';

.filter-wrapper {
  position: relative;
}

.selected-filter {
  flex: 1;
  &.is-active {
    font-weight: 700;
  }
}

.filter-title {
  margin-top: 0;
  margin-bottom: 5px;
  font-size: 0.9rem;
  text-transform: uppercase;
}

.button {
  font-size: 1.1rem;
  padding: 5px 10px;
  width: 100%;
  border-radius: 4px;
  border: 1px solid $color--black;
  display: flex;
  align-items: center;
  text-align: left;
  &.important {
    background: $color--white;
  }
}

.dropdown {
  position: absolute;
  top: 0;
  left: 0;
  background: $color--gray;
  width: 100%;
  z-index: 10;
  box-shadow: 0 0 20px rgba($color--black, 0.1);
  border: 1px solid $color--black;
  border-radius: 4px;
  padding-bottom: 10px;
}

.dropdown-title {
  padding: 0 10px 10px 10px;
  margin: 8px 0 8px 0;
  border-bottom: 1px solid darken($color--gray, 10%);
}

.option-item {
  padding: 3px 10px;
  cursor: pointer;
  &:hover {
    text-decoration: underline;
  }
}
</style>
