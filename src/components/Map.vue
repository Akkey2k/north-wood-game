<template>
  <div class="map-grid"
    :style="mapStyle"
    @mousedown="handleDnD($event, 'start')"
    @mousemove="handleDrag($event)"
    @mouseup="handleDnD($event, 'stop')"
    @wheel="handleZoom($event)"
    >
    <Tile v-for="(item, index) in gridCfg" :key="index" :cfg=item :tileSize=tileSize />
  </div>
</template>

<script>
import Tile from './Tile.vue';
import { createNoise2D } from 'simplex-noise';

export default {
  name: 'Map',
  data() {
    return {
      mapWidth: 50,
      mapHeight: 50,
      mapStyle: {
        height: 0,
        width: 0,
        left: 0,
        top: 0,
        scale: 1
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
      const gen = new createNoise2D();
      function noise(nx, ny) {
        // Rescale from -1.0:+1.0 to 0.0:1.0
        return gen(nx, ny) / 2 + 0.5;
      }

      const noiseArray = [];

      for (let y = 0; y < this.mapHeight; y++) {
        noiseArray[y] = [];
        for (let x = 0; x < this.mapWidth; x++) {
          let nx = x / this.mapWidth - 0.5,
              ny = y / this.mapHeight - 0.5;

          noiseArray[y][x] = noise(nx, ny);
        }
      }
      console.table(noiseArray);

      const cfg = [];

      function getBiome(e) {
        // if (e < 0.1) return "water";
        // else if (e < 0.2) return "sand";
        // else if (e < 0.3) return "forest";
        // else if (e < 0.5) return "jungle";
        // else if (e < 0.7) return "savannah";
        // else if (e < 0.9) return "desert";
        // else return "show";

        if (e < 0.3) return "sand";
        else if (e < 0.6) return "forest";
        else return "water";
      }

      for (let i = 0; i < this.mapWidth; i += 1) {
        for (let j = 0; j < this.mapHeight; j += 1) {
          cfg.push({
            type: getBiome(noiseArray[i][j]),
            x: i,
            y: j,
          });
        }
      }

      this.gridCfg = cfg;
      console.table(this.gridCfg);
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
    handleZoom(e) {
      e = e || window.event;
      e.preventDefault ? e.preventDefault() : (e.returnValue = false);

      var delta = e.deltaY || e.detail || e.wheelDelta;
      
      if (delta < 0) {
        if(this.mapStyle.scale <= 2.5) {
          this.mapStyle.scale += 0.1;
        }
      } else {
        if(this.mapStyle.scale >= 1) {
          this.mapStyle.scale -= 0.1;
        }
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
