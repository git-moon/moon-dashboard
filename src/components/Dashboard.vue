<template>
  <div id="dashboard">
    <ResizableBox :elements="drags" draggable>
      <template #default="props">
        <Resizable v-for="drag in props.items" :item="drag" :key="drag.key">
          <component :is="drag.component" v-bind="drag.bind" />
        </Resizable>
      </template>
    </ResizableBox>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

import ResizableBox from './resizable/ResizableBox.vue'
import Resizable from './resizable/Resizable.vue'
import CountSummary from './dashboard/CountSummary.vue'
import DAU from './dashboard/DAU.vue'
import TopReferralPie from './dashboard/TopReferralPie.vue'
import CountIp from './dashboard/CountIp.vue'

export default {
  components: {
    ResizableBox,
    Resizable,
    CountSummary,
    DAU,
    TopReferralPie,
    CountIp,
  },
  data() {
    return {
      drags: [
        {
          key: 'uniqueView',
          component: 'CountSummary',
          width: '50%',
          bind: { summaryKey: 'unique_view', title: '접속유저' },
        },
        {
          key: 'pageView',
          component: 'CountSummary',
          width: '50%',
          bind: { summaryKey: 'page_view', title: '접속횟수' },
        },
        { key: 'dau', cols: 12, component: 'DAU', bind: { title: 'DAU' } },
        {
          key: 'topReferral',
          component: 'TopReferralPie',
          width: '50%',
          bind: { title: 'Top Referral' },
        },
        {
          key: 'countIp',
          component: 'CountIp',
          width: '50%',
          bind: { title: 'Top Referral' },
        },
      ],
    }
  },
  created() {
    this.fetch()
  },
  methods: {
    ...mapActions('dashboard', ['getDailyReport', 'getReferral', 'getRegion']),
    fetch() {
      this.getDailyReport()
      this.getReferral()
      this.getRegion()
    },
  },
}
</script>
