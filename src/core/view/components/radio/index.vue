<template>
  <uni-radio
    :disabled="disabled"
    v-on="$listeners"
    @click="_onClick"
  >
    <div
      class="uni-radio-wrapper"
      :style="{
        '--HOVER-BD-COLOR': activeBorderColor
      }"
    >
      <div
        :class="{
          'uni-radio-input-checked': radioChecked,
          'uni-radio-input-disabled': disabled,
        }"
        :style="radioStyle"
        class="uni-radio-input"
      />
      <slot />
    </div>
  </uni-radio>
</template>
<script>
import {
  emitter,
  listeners
} from 'uni-mixins'
export default {
  name: 'Radio',
  mixins: [emitter, listeners],
  props: {
    checked: {
      type: [Boolean, String],
      default: false
    },
    id: {
      type: String,
      default: ''
    },
    disabled: {
      type: [Boolean, String],
      default: false
    },
    value: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: '#007AFF'
    },
    backgroundColor: {
      type: String,
      default: ''
    },
    borderColor: {
      type: String,
      default: ''
    },
    activeBackgroundColor: {
      type: String,
      default: ''
    },
    activeBorderColor: {
      type: String,
      default: ''
    },
    iconColor: {
      type: String,
      default: '#ffffff'
    }
  },
  data () {
    return {
      radioChecked: this.checked,
      radioValue: this.value
    }
  },
  computed: {
    radioStyle () {
      if (this.disabled) {
        return {
          backgroundColor: '#E1E1E1',
          borderColor: '#D1D1D1'
        }
      }
      const style = {}
      // 兼容旧版本样式
      if (this.radioChecked) {
        style.color = this.iconColor
        style.backgroundColor = this.activeBackgroundColor || this.color
        style.borderColor = this.activeBorderColor || style.backgroundColor
      } else {
        if (this.borderColor) style.borderColor = this.borderColor
        if (this.backgroundColor) style.backgroundColor = this.backgroundColor
      }
      return style
    }
  },
  watch: {
    checked (val) {
      this.radioChecked = val
    },
    value (val) {
      this.radioValue = val
    }
  },
  listeners: {
    'label-click': '_onClick',
    '@label-click': '_onClick'
  },
  created () {
    this.$dispatch('RadioGroup', 'uni-radio-group-update', {
      type: 'add',
      vm: this
    })
    this.$dispatch('Form', 'uni-form-group-update', {
      type: 'add',
      vm: this
    })
  },
  beforeDestroy () {
    this.$dispatch('RadioGroup', 'uni-radio-group-update', {
      type: 'remove',
      vm: this
    })
    this.$dispatch('Form', 'uni-form-group-update', {
      type: 'remove',
      vm: this
    })
  },
  methods: {
    _onClick ($event) {
      if (this.disabled || this.radioChecked) {
        return
      }
      this.radioChecked = true
      this.$dispatch('RadioGroup', 'uni-radio-change', $event, this)
    },
    _resetFormData () {
      this.radioChecked = this.min
    }
  }
}
</script>
<style>
	uni-radio {
		-webkit-tap-highlight-color: transparent;
		display: inline-block;
		cursor: pointer;
	}

	uni-radio[hidden] {
		display: none;
	}

	uni-radio[disabled] {
		cursor: not-allowed;
	}

	uni-radio .uni-radio-wrapper {
		display: -webkit-inline-flex;
		display: inline-flex;
		-webkit-align-items: center;
		align-items: center;
		vertical-align: middle;
	}

	uni-radio .uni-radio-input {
		-webkit-appearance: none;
		appearance: none;
		margin-right: 5px;
		outline: 0;
		border: 1px solid #D1D1D1;
		background-color: #ffffff;
		border-radius: 50%;
		width: 22px;
		height: 22px;
		position: relative;
	}

  @media (any-hover: hover) {
    uni-radio:not([disabled]) .uni-radio-input:hover {
      border-color: var(--HOVER-BD-COLOR, #007aff) !important;
    }
  }

	uni-radio .uni-radio-input.uni-radio-input-checked:before {
		font: normal normal normal 14px/1 "uni";
		content: "\EA08";
		font-size: 18px;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -48%) scale(0.73);
		-webkit-transform: translate(-50%, -48%) scale(0.73);
	}

	uni-radio .uni-radio-input.uni-radio-input-disabled {
		background-color: #E1E1E1;
		border-color: #D1D1D1;
	}

	uni-radio .uni-radio-input.uni-radio-input-disabled:before {
		color: #ADADAD;
	}

	uni-radio-group {
		display: block;
	}
</style>
