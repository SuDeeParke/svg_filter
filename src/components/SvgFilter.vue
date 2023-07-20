<template>
  <svg width="0px" height="0px" xmlns="http://www.w3.org/2000/svg" version="1.1">
    <defs>
      <filter id="f1" x="0%" y="0%" width="100%" height="100%">
        <feColorMatrix
          result="original"
          id="c1"
          type="matrix"
          :values="(matrixResult as unknown as string)" />
      </filter>
    </defs>
  </svg>
  <main class="container">
    <h1>SvgFilter</h1>
    <section class="display">
      <img :src="demoPic" alt="">
    </section>
    <section class="controler">
      <div class="group" v-for="(array) in matrix">
        <div class="item" v-for="(item) in array">
          <label>{{item.name}}</label>
          <el-slider v-model="item.value" show-input size="small" />
        </div>
      </div>

    </section>
    <section class="result">
      {{ matrixResult }}
    </section>
  </main>
</template>

<script lang="ts" setup>
import demoPic from '@/assets/images/demo.png';
import { ref, computed } from 'vue';


// 颜色矩阵
/**
 *      R G B A offset
 *
 *  r   0 0 0 0 0
 *  g   0 0 0 0 0
 *  b   0 0 0 0 0
 *  a   0 0 0 0 0
 */
const matrix = ref([
  [{ id: 'Rr', name: 'Red to red', value: 1 }, { id: 'Gr', name: 'Green to red', value: 0 }, { id: 'Br', name: 'Blue to red', value: 0 }, { id: 'Ar', name: 'Alpha to red', value: 0 }, { id: 'Or', name: 'red offset', value: 0 }],
  [{ id: 'Rg', name: 'Red to green', value: 0 }, { id: 'Gg', name: 'Green to green', value: 1 }, { id: 'Bg', name: 'Blue to green', value: 0 }, { id: 'Ag', name: 'Alpha to green', value: 0 }, { id: 'Og', name: 'green offset', value: 0 }],
  [{ id: 'Rb', name: 'Red to blue', value: 0 }, { id: 'Gb', name: 'Green to blue', value: 0 }, { id: 'Bb', name: 'Blue to blue', value: 1 }, { id: 'Ab', name: 'Alpha to blue', value: 0 }, { id: 'Ob', name: 'blue offset', value: 0 }],
  [{ id: 'Ra', name: 'Red to alpha', value: 0 }, { id: 'Ga', name: 'Green to alpha', value: 0 }, { id: 'Ba', name: 'Blue to alpha', value: 0 }, { id: 'Aa', name: 'Alpha to alpha', value: 1 }, { id: 'Oa', name: 'alpha offset', value: 0 }],
]);

const matrixResult = computed(() => {
  return `
    ${matrix.value[0][0].value} ${matrix.value[0][1].value} ${matrix.value[0][2].value} ${matrix.value[0][3].value} ${matrix.value[0][4].value}
    ${matrix.value[1][0].value} ${matrix.value[1][1].value} ${matrix.value[1][2].value} ${matrix.value[1][3].value} ${matrix.value[1][4].value} 
    ${matrix.value[2][0].value} ${matrix.value[2][1].value} ${matrix.value[2][2].value} ${matrix.value[2][3].value} ${matrix.value[2][4].value}
    ${matrix.value[3][0].value} ${matrix.value[3][1].value} ${matrix.value[3][2].value} ${matrix.value[3][3].value} ${matrix.value[3][4].value}
  `;
})
</script>

<style lang="scss" scoped>
.container {
  padding: 0px 50px;

  h1 {
    margin-bottom: 20px;
  }

  .display {
    display: flex;
    align-items: center;
    justify-content: start;
    border: 1px solid #eee;
    margin-bottom: 30px;
    img {
      width: 20%;
      filter: url('#f1');
    }
  }

  .controler {
    .group {
      width: 100%;
      display: flex;
      justify-content: space-between;
      .item {
        display: flex;

        label {
          width: 90px;
          padding: 0 20px 0 0;
        }
      }
    }

  }

}
</style>
