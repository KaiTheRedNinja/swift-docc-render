<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021-2023 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <div class="parameter-attributes">
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.default)"
      v-bind="{ kind: AttributeKind.default, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.default')
        }) }}<code>{{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.minimum)"
      v-bind="{ kind: AttributeKind.minimum, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.minimum')
        }) }}<code>{{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.minimumExclusive)"
      v-bind="{ kind: AttributeKind.minimumExclusive, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.minimum')
        }) }}<code>&gt; {{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.minimumLength)"
      v-bind="{ kind: AttributeKind.minimumLength, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.minimumLength')
        }) }}<code>{{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.maximum)"
      v-bind="{ kind: AttributeKind.maximum, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.maximum')
        }) }}<code>{{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.maximumExclusive)"
      v-bind="{ kind: AttributeKind.maximumExclusive, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.maximum')
        }) }}<code>&lt; {{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.maximumLength)"
      v-bind="{ kind: AttributeKind.maximumLength, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: attribute.title || $t('parameters.maximumLength')
        }) }}<code>{{ attribute.value }}</code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.allowedTypes)"
      v-bind="{ kind: AttributeKind.allowedTypes, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: $tc('parameters.possible-types', fallbackToValues(attribute).length)
        }) }}<code><template v-for="(possibleType, i) in fallbackToValues(attribute)">
            <template v-for="(token, j) in possibleType">
              <DeclarationToken v-bind="token" :key="`${i}-${j}`"
              /><template v-if="i + 1 < fallbackToValues(attribute).length">, </template>
            </template>
          </template>
        </code>
      </template>
    </ParameterMetaAttribute>
    <ParameterMetaAttribute
      v-if="shouldRender(AttributeKind.allowedValues)"
      v-bind="{ kind: AttributeKind.allowedValues, attributes: attributesObject, changes }">
      <template v-slot="{ attribute }">
        {{ $t('formats.colon', {
          content: $tc('parameters.possible-values', fallbackToValues(attribute).length)
        }) }}<code>{{ fallbackToValues(attribute).join(', ') }}</code>
      </template>
    </ParameterMetaAttribute>
  </div>
</template>

<script>

import DeclarationToken from 'docc-render/components/DocumentationTopic/PrimaryContent/DeclarationToken.vue';
import ParameterMetaAttribute
  from 'docc-render/components/DocumentationTopic/PrimaryContent/ParameterMetaAttribute.vue';

const AttributeKind = {
  allowedTypes: 'allowedTypes',
  allowedValues: 'allowedValues',
  default: 'default',
  maximum: 'maximum',
  maximumExclusive: 'maximumExclusive',
  maximumLength: 'maximumLength',
  minimum: 'minimum',
  minimumExclusive: 'minimumExclusive',
  minimumLength: 'minimumLength',
};

/**
 * Renders `attributes` property,
 * usually in `PropertyTable` or `RestParameters`
 */
export default {
  name: 'ParameterAttributes',
  components: { ParameterMetaAttribute, DeclarationToken },
  constants: { AttributeKind },
  props: {
    attributes: {
      type: Array,
      default: () => ([]),
    },
    changes: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    AttributeKind: () => AttributeKind,
    /**
     * Converts the attributes array into an object.
     * @param {array} attributes - array of attributes
     * @returns {object}
     */
    attributesObject: ({ attributes }) => attributes.reduce((all, attr) => ({
      ...all,
      [attr.kind]: attr,
    }), {}),
  },
  methods: {
    // checks whether provided `kind` should be rendered
    shouldRender(kind) {
      return Object.prototype.hasOwnProperty.call(this.attributesObject, kind);
    },
    fallbackToValues: (attribute) => {
      const attrs = attribute || [];
      return Array.isArray(attrs) ? attrs : (attrs).values;
    },
  },
};
</script>
