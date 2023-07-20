<template>
  <svg width="600px" height="400px" xmlns="http://www.w3.org/2000/svg" version="1.1">
    <defs>
      <filter id="f1" x="0%" y="0%" width="100%" height="100%">
        <feColorMatrix
          result="original"
          id="c1"
          type="matrix"
          values="
          1 0 0 0 0
          0 1 0 0 0
          0 0 1 0 0
          0 0 0 1 0"
        />
      </filter>
    </defs>
  </svg>
  <main class="container">
    <h1>svg_filter</h1>
    <section class="display">
      <img :src="demoPic" alt="">
    </section>
    <section class="controler">
      <div class="group">
        <label for="Rr"></label>
        <el-slider id="Rr" v-model="matrix.Rr" show-input size="small" />
        <el-slider v-model="matrix.Gr" show-input size="small" />
        <el-slider v-model="matrix.Br" show-input size="small" />
        <el-slider v-model="matrix.Ar" show-input size="small" />
        <el-slider v-model="matrix.Or" show-input size="small" />
      </div>
    </section>
  </main>
</template>

<script lang="ts" setup>
import demoPic from '@/assets/images/demo.png';
import { ref, onMounted } from 'vue';


// 颜色矩阵
/**
 *      R G B A offset
 *
 *  r   0 0 0 0 0
 *  g   0 0 0 0 0
 *  b   0 0 0 0 0
 *  a   0 0 0 0 0
 */
const matrix = ref({
  Rr: 0, Gr: 0, Br: 0, Ar: 0, Or: 0,
  Rg: 0, Gg: 0, Bg: 0, Ag: 0, Og: 0,
  Rb: 0, Gb: 0, Bb: 0, Ab: 0, Ob: 0,
  Ra: 0, Ga: 0, Ba: 0, Aa: 0, Oa: 0,
});

onMounted(() => {
 
});
</script>

<style lang="scss" scoped>
.container{

}
</style>
