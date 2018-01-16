<template>
    <v-select
            v-model="localValue"
            :items="field.choices"
            item-text="label"
            item-value="value"
            :label="field.label"
            :required="field.required"
            :readonly="field.readonly"
            :disabled="field.disabled"
            bottom

    />
</template>
<script>
  import abstractField from "../abstractField"

  export default {
    mixins: [abstractField],
    fieldTypes: ['choice', 'state',],
    data () {
      return {}
    },
    methods: {}
  }
</script>
