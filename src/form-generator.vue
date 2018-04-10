<template>
  <div>
    <div v-for="(section, i) in schema" :key="i">
      <v-card>
        <v-card-title>{{section.label}}</v-card-title>
        <div v-for="(field, j) in section.fields" :key="j">
          <v-form-generator-field :field="field" :value="model[field.model]" @upd="onInput" v-on:increment="onInput" />
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'v-form-generator',
  props: {
    model: Object,
    schema: Array,
    valid: Boolean
  },
  $_veeValidate: { validator: 'new' },
  components: {
    'v-form-generator-field': require('./form-field.vue').default
  },
  data() {
    return {};
  },
  created: function() {
    this.$watch(
      // () => this.$invalid(),
      value => {
        this.$emit('update:valid', !value);
      }
    );
  },
  methods: {
    onBlur: function() {
      console.info('blur');
    },
    onChange: function() {
      console.info('change');
    },
    onFocus: function() {
      console.info('focus');
    },
    onInput: function(value, fieldName) {
      this.model[fieldName] = value;
      this.$emit('update:model', this.model);
    }
  }
};
</script>
