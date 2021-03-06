<template>
<div :class="{
  'veui-dropdown': true,
  'veui-dropdown-expanded': expanded
}" :ui="ui">
  <veui-button
    class="veui-dropdown-button"
    :ui="ui"
    :disabled="disabled"
    @click="expanded = !expanded"
    slot="target"
    ref="button">
    <span class="veui-dropdown-label">
      <slot name="label" :label="label">{{ label }}</slot>
    </span>
    <icon class="veui-dropdown-icon" :name="icons[expanded ? 'collapse': 'expand']"></icon>
  </veui-button>
  <veui-overlay
    v-if="expanded"
    target="button"
    :open="expanded"
    :options="realOverlayOptions"
    :overlay-class="overlayClass">
    <div ref="box" class="veui-dropdown-options" v-outside:button="close">
      <div v-for="option in options"
        :key="option.value"
        :ui="ui"
        class="veui-dropdown-option"
        :class="{
          'veui-dropdown-option-disabled': option.disabled
        }"
        @click.stop="handleClick(option)">
        <slot name="button" v-bind="option">
          <span class="veui-dropdown-option-label">{{ option.label }}</span>
        </slot>
      </div>
    </div>
  </veui-overlay>
</div>
</template>

<script>
import Icon from './Icon'
import Button from './Button'
import Overlay from './Overlay'
import dropdown from '../mixins/dropdown'
import icons from '../mixins/icons'
import overlay from '../mixins/overlay'

export default {
  name: 'veui-dropdown',
  components: {
    'icon': Icon,
    'veui-button': Button,
    'veui-overlay': Overlay
  },
  mixins: [dropdown, icons, overlay],
  props: {
    ui: String,
    label: String,
    disabled: {
      type: Boolean,
      default: false
    },
    icon: String,
    options: Array
  },
  methods: {
    handleClick (option) {
      if (option.disabled) {
        return
      }
      this.expanded = false
      this.$emit('click', option.value)
    }
  }
}
</script>
