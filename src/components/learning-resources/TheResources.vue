<template>
  <base-card>
    <!-- the click function fall into the child by default in this cause the button -->
    <base-button
      @click="setSelectedTab('store-resources')"
      :mode="storedResButtonMode"
      >Store Resource</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
      >Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoreResources from './StoreResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoreResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'store-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.com',
        },
      ],
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'store-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource, 
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, link) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: link,
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'store-resources';
    },
    removeResource(resId) {
      // this will not work beacuse the override 
      // of the array via provide and inject 
      // does not update to the rest of the places

      // this.storedResources = this.storedResources.filter(
      //   (res) => res.id !== resId
      // );

      const resIndex = this.storedResources.findIndex(res => res.id === resId);
      this.storedResources.splice(resIndex, 1);
    },
  },
};
</script>
