<template>
  <div ref="component-container">
    <fieldset
      v-for="(item, idx) in schema.fieldsets"
      v-bind:key="idx"
    >
      <template v-for="(field, f_idx) in item">
        <TextInput
          v-if="field.type === 'text'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:reset="field.reset"
          v-bind:inline="field.inline"
          v-on:input="$emit('input', field.model, $event)"
        ></TextInput>
        <NumberInput
          v-if="field.type === 'number'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:reset="field.reset"
          v-bind:inline="field.inline"
          v-on:input="$emit('input', field.model, $event)"
        ></NumberInput>
        <TimeInput
          v-if="field.type === 'time'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:inline="field.inline"
          v-on:input="$emit('input', field.model, $event)"
        ></TimeInput>
        <FileInput
          v-if="field.type === 'file'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:filter="field.filter"
          v-on:input="$emit('input', field.model, $event)"
        ></FileInput>
        <CheckboxInput
          v-if="field.type === 'checkbox'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-on:input="$emit('input', field.model, $event)"
        ></CheckboxInput>
        <SwitchInput
          v-if="field.type === 'switch'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-on:input="$emit('input', field.model, $event)"
        ></SwitchInput>
        <RadioInput
          v-if="field.type === 'radio'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:options="field.options"
          v-on:input="$emit('input', field.model, $event)"
        ></RadioInput>
        <SelectInput
          v-if="field.type === 'select'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:options="field.options"
          v-on:input="$emit('input', field.model, $event)"
        ></SelectInput>
        <ListInput
          v-if="field.type === 'list'"
          v-bind:key="f_idx"
          v-bind:value="getModelValue(field.model)"
          v-bind:label="field.label"
          v-bind:name="field.model"
          v-bind:list-options="field.listOptions"
          v-bind:options="field.options"
          v-on:input="$emit('input', field.model, $event)"
        ></ListInput>
        <Theme
          v-if="field.type === 'theme'"
          v-bind:key="f_idx"
          v-bind:options="field.options"
          v-on:input="$emit('input', field.model, $event)"
        ></Theme>
      </template>
    </fieldset>
  </div>
</template>

<script>
// Reference for how to do all this stuff dynamically:
// https://css-tricks.com/creating-vue-js-component-instances-programmatically/

import TextInput from './elements/Text.vue'
import NumberInput from './elements/Number.vue'
import TimeInput from './elements/Time.vue'
import FileInput from './elements/File.vue'
import CheckboxInput from './elements/Checkbox.vue'
import SwitchInput from './elements/Switch.vue'
import RadioInput from './elements/Radio.vue'
import SelectInput from './elements/Select.vue'
import ListInput from './elements/List.vue'
import Theme from './elements/Theme.vue'

export default {
  name: 'Form',
  components: {
    TextInput,
    NumberInput,
    TimeInput,
    FileInput,
    CheckboxInput,
    SwitchInput,
    RadioInput,
    SelectInput,
    ListInput,
    Theme
  },
  props: {
    model: {
      type: Object,
      default: function () { return {} }
    },
    schema: {
      type: Object,
      default: function () { return {} }
    }
  },
  methods: {
    getModelValue: function (model) {
      const modelProps = model.split('.')
      let modelValue = this.model
      for (const key of modelProps) {
        modelValue = modelValue[key]
      }

      return modelValue
    }
  }
}
</script>

<style lang="less">
body {
  .form-control {
    margin: 4px 0px;

    input:not(.inline),
    select:not(.inline),
    textarea:not(.inline) {
      width: 100%;
      display: block;
    }
  }

  // Shows an input and a button side by side
  .input-button-group {
    display: flex;
    input {
      flex: 9;
      margin-right: 4px;
    }

    button {
      flex: 1;
    }
  }
}

body.darwin {
  fieldset {
    border: none;
    border-bottom: 1px solid rgb(210, 210, 210);

    &:last-child {
      border-bottom: none;
    }
  }

  // Generic inputs
  input, select, textarea, button {
    background-color: white;
    border: 1px solid rgb(210, 210, 210);
    border-radius: 6px;
    padding: 2px 4px;
  }

  &.dark {
    fieldset {
      border-bottom-color: rgb(50, 50, 50);
    }
  }
}

body.win32 {
  fieldset {
    border: none;
  }

  // Generic inputs
  input, select, textarea, button {
    background-color: white;
    border: 2px solid rgb(90, 90, 90);
    border-radius: 0px;
    padding: 8px 8px;
  }
}

body.linux {
  fieldset {
    border: none;
  }

  input, select, textarea, button {
    border-radius: 4px;
    padding: 8px;
  }

  button {
    background-color: rgb(230, 230, 230);
  }
}
</style>
