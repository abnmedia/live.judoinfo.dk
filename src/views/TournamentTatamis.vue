<template>
  <h2>Tatamis</h2>
  <Tabs @change="changeTab">
    <TabItem v-for="tatamiIndex in tatamiCount" :key="tatamiIndex" :title="'Tatami ' + tatamiIndex">
      <InfoText v-if="tatamiState == 'NOT_LOADED'" icon="cloud-off-outline">Tatami not found. The tatami does not exist.</InfoText>
      <InfoText v-if="tatamiState == 'LOADING'" icon="loading" rotating>Loading tatami..</InfoText>
      <template v-if="tatamiState == 'LOADED'">
        <InfoText v-if="tatamiBlocks.length == 0">This tatami has no matches yet.</InfoText>
        <div class="concurrent-group" v-for="(concurrentGroup, concurrentIndex) in tatamiBlocks" :key="concurrentIndex">
          <div class="sequential-group" v-for="(sequentialGroup, sequentialIndex) in concurrentGroup" :key="sequentialIndex">
            <div v-for="(block, blockIndex) in sequentialGroup" :key="blockIndex"
                :class="{'tatami-block': true, 'finished': block.status == 'FINISHED', 'started': block.status == 'STARTED'}">
              <div class="tatami-block-inner">
                <router-link :to="{ name: 'tournament-category', params: { tournament: this.$route.params.tournament, categoryId: block.id }}">{{ block.name }}</router-link>
                <p>{{ blockTypeFilter(block.type) }} <span v-if="block.status != 'NOT_STARTED'">({{ blockStatusFilter(block.status) }})</span></p>
              </div>
            </div>
          </div>
        </div>
      </template>
    </TabItem>
  </Tabs>
</template>

<script>
/* <TabItem :label="'Tatami ' + (index + 1)" :key="index" v-for="(matches, index) of tatamiMatches"> */
import Tabs from '@/components/Tabs.vue'
import TabItem from '@/components/TabItem.vue'
import InfoText from '@/components/InfoText.vue'
import { mapState, mapGetters } from 'vuex'
import { blockTypeFilter, blockStatusFilter } from '@/store/filters.js'

export default {
  components: { Tabs, TabItem, InfoText, },
  mounted() {
    this.$store.dispatch('subscribeTatami', 0);
  },
  methods: {
    changeTab: function(index) {
      this.$store.dispatch('subscribeTatami', index);
    },
    blockTypeFilter: blockTypeFilter,
    blockStatusFilter: blockStatusFilter,
  },
  computed: {
    ...mapState({
      tatami: state => state.tatami,
      tatamiState: state => state.tatamiState,
      tatamiCount: state => state.tournament.tatamiCount,
    }),
    ...mapGetters({
      tatamiBlocks: 'tatamiBlocks',
    }),
  },
}
</script>

<style scoped lang="scss">
  @import "../styles/constants.module.scss";

  .concurrent-group {
    display: flex;
    align-items: stretch;
  }

  .sequential-group {
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .tatami-block {
    border: 1px solid $color6;
    margin: 0 5px 10px 5px;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 5px 15px 0px;
    padding: 10px 0;
    flex: 1;
  }

  .tatami-block.finished {
    border-left: 5px solid #a3be8c;
  }

  .tatami-block.started {
    border-left: 5px solid #ebcb8b;
  }

  .tatami-block a, .tatami-block p {
    display: block;
    text-align: center;
    margin: 0;
  }

  .tatami-block a {
    color: $color1;
  }

  .tatami-block p {
    color: $color4;
    font-size: .9em;
  }

  @media (max-width: 767px) {
    .tatami-block {
      margin: 5px;
    }

    .tatami-block-inner {
      writing-mode: vertical-rl;
    }
  }
</style>
