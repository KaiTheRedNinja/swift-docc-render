<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<script>
import WordBreak from 'docc-render/components/WordBreak.vue';
import ChangedToken from './DeclarationToken/ChangedToken.vue';
import LinkableToken from './DeclarationToken/LinkableToken.vue';
import RawText from './DeclarationToken/RawText.vue';
import SyntaxToken from './DeclarationToken/SyntaxToken.vue';
import Highlighted from './DeclarationToken/Highlighted.vue';

const TokenKind = {
  attribute: 'attribute',
  externalParam: 'externalParam',
  genericParameter: 'genericParameter',
  identifier: 'identifier',
  internalParam: 'internalParam',
  keyword: 'keyword',
  label: 'label',
  number: 'number',
  string: 'string',
  text: 'text',
  typeIdentifier: 'typeIdentifier',
  highlightDiff: 'highlightDiff',
  added: 'added',
  removed: 'removed',
};

export default {
  name: 'DeclarationToken',
  render: function _render(createElement) {
    const {
      kind,
      text,
      tokens,
    } = this;
    switch (kind) {
    case TokenKind.text: {
      const props = { text };
      return createElement(RawText, { props });
    }
    case TokenKind.typeIdentifier: {
      const props = { identifier: this.identifier };
      return createElement(LinkableToken, {
        class: 'type-identifier-link',
        props,
      }, [
        createElement(WordBreak, text),
      ]);
    }
    case TokenKind.attribute: {
      const { identifier } = this;
      return identifier ? (
        createElement(LinkableToken, {
          class: 'attribute-link',
          props: { identifier },
        }, [
          createElement(WordBreak, text),
        ])
      ) : (
        createElement(SyntaxToken, { props: { kind, text } })
      );
    }
    case TokenKind.added:
    case TokenKind.removed:
      return createElement(ChangedToken, { props: { tokens, kind } });
    case TokenKind.highlightDiff:
      return createElement(Highlighted, {}, (tokens || []).map(token => (
        _render.bind(token)(createElement)
      )));
    default: {
      const props = {
        kind,
        text,
      };
      return createElement(SyntaxToken, { props });
    }
    }
  },
  constants: { TokenKind },
  props: {
    kind: {
      type: String,
      required: true,
    },
    identifier: {
      type: String,
      required: false,
    },
    // ran into an issue with empty spaces when relying on the default slot
    // for passing text, so making it an explicit prop for now as a workaround
    text: {
      type: String,
      required: false,
    },
    /**
     * Used only when rendering `added/removed` tokens.
     * @type {DeclarationToken[]}
     */
    tokens: {
      type: Array,
      required: false,
      default: () => ([]),
    },
  },
};
</script>

<style scoped lang="scss">
@import 'docc-render/styles/_core.scss';

.token-method {
  font-weight: $font-weight-bold;
}

.token-keyword {
  color: var(--syntax-keyword, var(--color-syntax-keywords));
}

.token-number {
  color: var(--syntax-number, var(--color-syntax-numbers));
}

.token-string {
  color: var(--syntax-string, var(--color-syntax-strings));
}

.token-attribute,
.attribute-link {
  color: var(--syntax-attribute, var(--color-syntax-keywords));
}

.token-internalParam {
  color: var(--color-syntax-param-internal-name);
}

.type-identifier-link {
  color: var(--syntax-type, var(--color-syntax-other-type-names));
}

.token-removed {
  background-color: var(--color-highlight-red);
}

.token-added {
  background-color: var(--color-highlight-green);
}
</style>
