<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2022-2024 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <Pager
    :aria-label="$t('links-grid.label')"
    :class="['TopicsLinkCardGrid', topicStyle]"
    :pages="pages"
  >
    <template #page="{ page }">
      <Row :columns="{
        large: compactCards ? 3 : 2,
        medium: compactCards ? 3 : 2,
      }">
        <Column
          v-for="item in page"
          :key="item.title"
        >
          <TopicsLinkCardGridItem :item="item" :compact="compactCards" />
        </Column>
      </Row>
    </template>
    <BreakpointEmitter @change="handleBreakpointChange" />
  </Pager>
</template>

<script>
import BreakpointEmitter from 'docc-render/components/BreakpointEmitter.vue';
import Column from 'docc-render/components/ContentNode/Column.vue';
import Pager from 'theme/components/Pager.vue';
import Row from 'docc-render/components/ContentNode/Row.vue';
import { TopicSectionsStyle } from 'docc-render/constants/TopicSectionsStyle';
import { BreakpointName } from 'docc-render/utils/breakpoints';
import { page } from 'docc-render/utils/arrays';
import TopicsLinkCardGridItem from './TopicsLinkCardGridItem.vue';

export default {
  name: 'TopicsLinkCardGrid',
  components: {
    BreakpointEmitter,
    Column,
    Pager,
    Row,
    TopicsLinkCardGridItem,
  },
  data: () => ({
    breakpoint: BreakpointName.large,
  }),
  props: {
    items: {
      type: Array,
      required: true,
    },
    pageSize: {
      type: Number,
      required: false,
    },
    topicStyle: {
      type: String,
      default: TopicSectionsStyle.compactGrid,
      validator: v => v === TopicSectionsStyle.compactGrid || v === TopicSectionsStyle.detailedGrid,
    },
    usePager: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    compactCards: ({ topicStyle }) => topicStyle === TopicSectionsStyle.compactGrid,
    defaultPageSize: ({
      breakpoint,
      items,
      topicStyle,
      usePager,
    }) => (usePager ? {
      [TopicSectionsStyle.compactGrid]: {
        [BreakpointName.large]: 6,
        [BreakpointName.medium]: 6,
        [BreakpointName.small]: 1,
      },
      [TopicSectionsStyle.detailedGrid]: {
        [BreakpointName.large]: 4,
        [BreakpointName.medium]: 2,
        [BreakpointName.small]: 1,
      },
    }[topicStyle][breakpoint] : (
      items.length
    )),
    pages: ({
      items,
      defaultPageSize,
      pageSize,
    }) => page(items, (pageSize || defaultPageSize)),
  },
  methods: {
    handleBreakpointChange(breakpoint) {
      this.breakpoint = breakpoint;
    },
  },
};
</script>
