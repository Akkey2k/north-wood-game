<template>
  <div class="map-grid"
    :style="mapStyle"
    @mousedown="handleDnD($event, 'start')"
    @mousemove="handleDrag($event)"
    @mouseup="handleDnD($event, 'stop')"
  >
    <Tile
      v-for="(item, index) in gridCfg" :key="index"
      :cfg = item
      :tileSize = tileSize
    />
  </div>
</template>

<script>
import Tile from './Tile.vue';

export default {
  name: 'Map',
  data() {
    return {
      mapWidth: 20,
      mapHeight: 20,
      mapStyle: {
        height: 0,
        width: 0,
        left: 0,
        top: 0,
      },
      gridCfg: [],
      tileSize: 64,
      isEnabledDnD: false,
      positionDnD: {
        x: 0,
        y: 0,
        offsetX: 0,
        offsetY: 0,
      },
    };
  },
  components: {
    Tile,
  },
  methods: {
    calcSize() {
      this.mapStyle.width = `${(this.mapWidth * this.tileSize)}px`;
      this.mapStyle.height = `${(this.mapHeight * this.tileSize)}px`;
    },
    generateGrid() {
      const cfg = [];

      for (let i = 0; i < this.mapWidth; i += 1) {
        for (let j = 0; j < this.mapHeight; j += 1) {
          cfg.push({
            type: 'herb',
            x: i,
            y: j,
          });
        }
      }

      this.gridCfg = cfg;
    },
    handleDnD(e, state) {
      const isEnabled = state === 'start';
      this.isEnabledDnD = isEnabled;

      if (isEnabled) {
        this.positionDnD.x = e.clientX;
        this.positionDnD.y = e.clientY;
      } else {
        this.positionDnD.offsetX += e.clientX - this.positionDnD.x;
        this.positionDnD.offsetY += e.clientY - this.positionDnD.y;
      }
    },
    handleDrag(e) {
      if (this.isEnabledDnD) {
        this.mapStyle.left = `${this.positionDnD.offsetX + (e.clientX - this.positionDnD.x)}px`;
        this.mapStyle.top = `${this.positionDnD.offsetY + (e.clientY - this.positionDnD.y)}px`;
      }
    },
  },
  created() {
    this.calcSize();
    this.generateGrid();
  },
};
</script>

<style>
.map-grid {
  position: absolute;
  transform: rotateX(65deg) rotateZ(-55deg);
}
</style>
