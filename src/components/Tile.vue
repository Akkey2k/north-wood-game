<template>
  <div class="tile" :style="tileStyle"></div>
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
  background-image: url("~@/assets/tileGrass1.png");
  background-repeat: no-repeat;
  background-size: 64px;
  transition: .2s;
}
.tile:hover {
  outline: 3px #2b2b2b solid;
  outline-offset: -6px;
}
</style>
