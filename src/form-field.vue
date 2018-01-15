<template>
    <div>
        <div v-if="field.field_id === 'unit_field' ||
					field.field_id === 'single_line' ||
					field.field_id === 'ssn' ||
					field.field_id === 'phone'
					">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.editable"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :error-messages="errors.collect(field.name)"
                    :name="field.name"
                    :id="field.name"
                    v-validate="field.required && 'required'"
                    :data-vv-delay="delay"
                    :data-vv-name="field.name"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'multi_line'">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.editable"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :error-messages="errors.collect(field.name)"
                    :name="field.name"
                    :id="field.name"
                    v-validate="field.required && 'required'"
                    :data-vv-delay="delay"
                    :data-vv-name="field.name"
                    multi-line
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'email'">
            <v-text-field
                    v-model.lazy="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :error-messages="errors.collect(field.name)"
                    :name="field.name"
                    :id="field.name"
                    v-validate="field.required && 'required|' || '' + 'email'"
                    :data-vv-delay="delay"
                    :data-vv-name="field.name"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'date'">
            <component :is="field.field_id"
                       :field="field"
                        @change="onChange"
                        @focus="onFocus"
                        @input="onInput"/>
        </div>
        <div v-else-if="field.field_id === 'choice' || field.field_id === 'state'">
            <v-select
                    v-model="localValue"
                    :items="field.choices"
                    item-text="label"
                    item-value="value"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    single-line
                    bottom

            />
        </div>
        <div v-else-if="field.field_id === 'text_display'">
            <div v-html="field.label"></div>
        </div>
        <div v-else-if="field.field_id === 'file'">
            No File Field Right Now
        </div>
        <div v-else-if="field.field_id === 'signature'">
            <vueSignature ref="signature"/>
        </div>

        <div v-else-if="field.field_id === 'password'">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :append-icon="field.passwordVisible ? 'visibility_off' : 'visibility'"
                    :append-icon-cb="appendPasswordIconCheckbox()"
                    :type="field.passwordVisible ? 'text' : 'password'"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>


        <div v-else>
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :counter="field.counter"
                    :hint="field.hint"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            ></v-text-field>


            <v-alert v-if="field.type != 'text'" color="error" icon="warning" value="true">
                <strong>The {{field.type}} type is not yet implemented.</strong> <br>
                {{field}}
            </v-alert>

        </div>
    </div>
</template>

<script>
  import { forEach } from "lodash"

  let fieldComponents = {}
  let coreFields = require.context("./fields/core", false, /^\.\/field([\w-_]+)\.vue$/)
  forEach(coreFields.keys(), (key) => {
    forEach(coreFields(key).default.fieldTypes, (fieldType) => {
      fieldComponents[fieldType] = coreFields(key).default
    })
  })
  console.log(fieldComponents)

  export default {
    inject: ['$validator'],
    name: 'v-form-generator-field',
    components: fieldComponents,
    props: {
      field: Object,
      value: null
    },
    data () {
      return {
        localValue: this.value,
        delay: 600
      }
    },

    methods: {
      onBlur: function () {
        this.$emit('blur')
      },
      onChange: function () {
        this.$emit('change')
      },
      onFocus: function () {
        this.$emit('focus')
      },
      onInput: function () {
        this.$emit('upd', this.localValue, this.field.name)
      },
      appendPasswordIconCheckbox () {
        return () => this.field.passwordVisible = !this.field.passwordVisible
      }
    }
  }
</script>
