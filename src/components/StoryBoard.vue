<template>
  <div id="story-chapters-container">
    <div id="chapters">
      <div v-for="chapter in mapStory.chapters" id="features" :key="chapter.id">
        <section
          :id="chapter.id"
          :class="chapter.class"
          @click="moveToLocation(chapter.flyToCommands, chapter.id), toggleLayerVisibility(chapter.layersToHide, chapter.hiddenLayersToShow)"
          @mouseover="moveToLocation(chapter.flyToCommands, chapter.id), toggleLayerVisibility(chapter.layersToHide, chapter.hiddenLayersToShow)"
        >
          <h3>{{ chapter.title }}</h3>
          <p>{{ chapter.content }}</p>
        </section>
      </div>
    </div>
  </div>
</template>
<script>
import mapStory from "../assets/mapStory/mapStory";

export default {
  name: "StoryBoard",
  data() {
    return {
      mapStory: mapStory,
      layersToUnhide: [],
      layersToUnshow: []
    };
  },
  methods: {
    moveToLocation(flyToCommands, elementId) {
      let allActiveSectionElements = document.querySelectorAll(
        "section.active"
      );
      [...allActiveSectionElements].forEach(selection => {
        selection.setAttribute("class", "");
      });
      document.getElementById(elementId).setAttribute("class", "active");
      this.$store.map.flyTo(flyToCommands);
    },
    toggleLayerVisibility(layersToHide, layersToShow) {
      console.log("layers ", this.$store.map.getStyle().layers);

      let self = this;
      let map = this.$store.map;
      let layersList = self.$store.map.getStyle().layers;
      // Reset all layer visibility to the way it was when the page was first loaded.
      layersList.forEach(function(layer) {
        console.log("current layer ", layer);
        if (self.layersToUnhide.includes(layer.id)) {
          let visibility = map.getLayoutProperty(layer.id, "visibility");
          map.setLayoutProperty(layer.id, "visibility", "visible");
        }
        if (self.layersToUnshow.includes(layer.id)) {
          let visibility = map.getLayoutProperty(layer.id, "visibility");
          map.setLayoutProperty(layer.id, "visibility", "none");
        }
      });

      // Show or hide layer based on what is listed in the 'mapStory.js'
      layersList.forEach(function(layer) {
        if (layersToShow.includes(layer.id)) {
          let visibility = map.getLayoutProperty(layer.id, "visibility");
          map.setLayoutProperty(layer.id, "visibility", "visible");
        }
        if (layersToHide.includes(layer.id)) {
          let visibility = map.getLayoutProperty(layer.id, "visibility");
          map.setLayoutProperty(layer.id, "visibility", "none");
        }
      });

      // add the layers we changed to the component data, so that the next time the toggle is run we can reset them
      this.layersToUnhide = layersToHide;
      this.layersToUnshow = layersToShow;
    }
  }
};
</script>
<style scoped lang="scss">
#story-chapters-container{
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  #chapters{
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    overflow: auto;
    /* for Firefox */
    min-height: 0;
  }
#features {
  font-family: sans-serif;
  flex-grow: 1;
}
section {
  padding: 25px 50px;
  line-height: 25px;
  border-bottom: 1px solid #ddd;
  opacity: 0.25;
  font-size: 13px;
}
section.active {
  opacity: 1;
}
section:last-child {
  border-bottom: none;
}
</style>
