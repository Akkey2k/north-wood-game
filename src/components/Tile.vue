<template>
  <span class="tile" :style="tileStyle" :class="cfg.type"></span>
</template>

<script>
export default {
  name: 'Tile',
  props: {
    cfg: Object,
    tileSize: Number,
  },
  data() {
    return {
    };
  },
  computed: {
    tileStyle() {
      const position = {
        left: `${(this.cfg.x * this.tileSize)}px`,
        top: `${(this.cfg.y * this.tileSize)}px`,
        width: `${this.tileSize}px`,
        height: `${this.tileSize}px`,
      };

      return position;
    },
  },
};
</script>

<style>
.tile {
  position: absolute;
  background-color: #2b2b2b;
  transition: .2s;
  outline-offset: -1px;
  background-repeat: no-repeat;
  background-size: 64px;
  overflow: hidden;
}
.tile.water{
  background-color: #00a6ff;
}
.tile.sand{
  background-image: url("~@/assets/tileSand1.png");
}
.tile.forest{
  background-image: url("~@/assets/tileGrass1.png");
}
.tile:hover {
  outline: 3px #2b2b2b solid;
  outline-offset: -6px;
}
</style>
