<template>
    <transition name="fade" appear>
        <div>

            <div class="vis-tabs">
              <div v-for="tab in Array.from(tabs.keys())" :key="tab">
                <div @click="toggleTab(tab)" :class="(tab === currentTab) ? 'tab current-tab' : 'tab'">
                  <div>Tab {{tab}}</div>
                  <div @click="closeTab(tab)"><vue-icon className="tab-icon" icon="cross" iconSize="13"></vue-icon></div>
                </div>
              </div>
              <button v-if="tabs.size < 11" @click="newTab" class='btn new-tab-btn'><vue-icon icon="plus" className="vue-icon"></vue-icon></button>
            </div>

            <keep-alive>
                  <component :is="tabs.get(currentTab)"></component>
            </keep-alive>

        </div>
    </transition>
</template>

<style scoped>

  .vis-tabs {
    position: absolute;
    bottom: 2.3%;
    z-index: 1;
    width: 100%;
    display: flex;
    align-items: center;
  }

  .tab {
    background-color: var(--gray-4);
    width: 120px;
    height: 30px;
    border: var(--container-darkest-border);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    justify-content: space-between;
    padding: var(--container-padding);
  }

  .current-tab {
    background-color: var(--gray-1);
    border-top: none;
  }

</style>

<script>

import Vue from 'vue';
import VisTab from './VisTab.vue';

export default {
  name: 'VisualiserContent',
  components: { VisTab },
  data() {
    return {
      currentTab: undefined,
      tabs: new Map(),
      visTab: 'VisTab',
    };
  },
  created() {
    const Ciao = Vue.extend(VisTab);
    // const lol = new Ciao({ propsData: { tabId: 0 }, store: this.$store });
    debugger;
    this.tabs.set(0, Ciao);
    const Ciao1 = Vue.extend(VisTab);

    this.tabs.set(1, Ciao1);
    this.currentTab = 0;
  },
  methods: {
    toggleTab(tab) {
      this.currentTab = tab;
    },
    newTab() {
      const newTabId = Math.max(...Array.from(this.tabs.values())) + 1;
      this.tabs.add(newTabId);
      this.currentTab = newTabId;
    },
    closeTab(tab) {
      if (this.currentTab === tab) this.currentTab = Array.from(this.tabs.values())[0];
      this.tabs.delete(tab);
      this.tabs = new Set(Array.from(this.tabs.values()));
    },
  },
};
</script>
