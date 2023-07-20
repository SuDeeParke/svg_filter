<template>
  <svg width="0px" height="0px" xmlns="http://www.w3.org/2000/svg" version="1.1">
    <defs>
      <filter id="f1" x="0%" y="0%" width="100%" height="100%">
        <feColorMatrix
          result="original"
          id="c1"
          type="matrix"
          :values="(matrixResult as unknown as string)" />

        <feColorMatrix id="saturate" type="saturate" :values="`${(saturate)}`" />
        <feColorMatrix type="hueRotate" :values="`${(hueRotate)}`"/>
      </filter>
    </defs>
  </svg>
  <main class="container">
    <h1>SvgFilter</h1>
    <section class="display">
      <img :src="demoPic" alt="">
    </section>
    <section class="controler">
      <el-collapse v-model="activeNames" >
        <el-collapse-item title="颜色矩阵" name="1">
          <div class="group" v-for="(array) in matrix">
            <div class="item" v-for="(item) in array">
              <label>{{ item.id }}:</label>
              <div class="result">
                {{ item.value }}
                <div class="pop">
                  <el-slider v-model="item.value" show-input size="small" :step="0.01" :min="-1" :max="1" />
                </div>
              </div>
            </div>
          </div>
           <div class="btn">
             <el-button plain @click="reset()">重设</el-button>
            </div>
        </el-collapse-item>
        
        <el-collapse-item title="高级项" name="2">
          <div class="slide-item">
            <label>饱和度</label>
            <el-slider v-model="saturate" show-input size="small" :step="0.01" :min="0" :max="1" />
          </div>
          <div class="slide-item">
              <label>色相</label>
              <el-slider v-model="hueRotate" show-input size="small" :min="0" :max="360" />
          </div>
          <div class="btn">
            <el-button plain @click="resetAdvance()">重设</el-button>
          </div>

        </el-collapse-item>
      </el-collapse>

    </section>
    <section class="output">
      <div> 颜色矩阵结果：{{ matrixResult }}</div>
      <div> 饱和度： {{ saturate }}</div>
      <div> 色相： {{ hueRotate }}</div>
   
    </section>
  </main>
</template>

<script lang="ts" setup>
import demoPic from '@/assets/images/demo.png';
import { ref, computed } from 'vue';
import {cloneDeep } from 'lodash';

// 颜色矩阵
/**
 *      R G B A offset
 *
 *  r   0 0 0 0 0
 *  g   0 0 0 0 0
 *  b   0 0 0 0 0
 *  a   0 0 0 0 0
 */ 
const defVal = [
  [{ id: 'Rr', name: 'Red to red', value: 1 }, { id: 'Gr', name: 'Green to red', value: 0 }, { id: 'Br', name: 'Blue to red', value: 0 }, { id: 'Ar', name: 'Alpha to red', value: 0 }, { id: 'Or', name: 'red offset', value: 0 }],
  [{ id: 'Rg', name: 'Red to green', value: 0 }, { id: 'Gg', name: 'Green to green', value: 1 }, { id: 'Bg', name: 'Blue to green', value: 0 }, { id: 'Ag', name: 'Alpha to green', value: 0 }, { id: 'Og', name: 'green offset', value: 0 }],
  [{ id: 'Rb', name: 'Red to blue', value: 0 }, { id: 'Gb', name: 'Green to blue', value: 0 }, { id: 'Bb', name: 'Blue to blue', value: 1 }, { id: 'Ab', name: 'Alpha to blue', value: 0 }, { id: 'Ob', name: 'blue offset', value: 0 }],
  [{ id: 'Ra', name: 'Red to alpha', value: 0 }, { id: 'Ga', name: 'Green to alpha', value: 0 }, { id: 'Ba', name: 'Blue to alpha', value: 0 }, { id: 'Aa', name: 'Alpha to alpha', value: 1 }, { id: 'Oa', name: 'alpha offset', value: 0 }],
]

const matrix = ref(cloneDeep(defVal));


// 饱和度
const saturate = ref<number>(1);

// hueRotate
const hueRotate = ref<number>(0);

const matrixResult = computed(() => {
  return `
    ${matrix.value[0][0].value} ${matrix.value[0][1].value} ${matrix.value[0][2].value} ${matrix.value[0][3].value} ${matrix.value[0][4].value}
    ${matrix.value[1][0].value} ${matrix.value[1][1].value} ${matrix.value[1][2].value} ${matrix.value[1][3].value} ${matrix.value[1][4].value} 
    ${matrix.value[2][0].value} ${matrix.value[2][1].value} ${matrix.value[2][2].value} ${matrix.value[2][3].value} ${matrix.value[2][4].value}
    ${matrix.value[3][0].value} ${matrix.value[3][1].value} ${matrix.value[3][2].value} ${matrix.value[3][3].value} ${matrix.value[3][4].value}
  `;
})


const activeNames = ref(['1'])

const reset = () => {
  matrix.value = cloneDeep(defVal);
}

const resetAdvance = () => {
  saturate.value = 1;
  hueRotate.value = 0;
}
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
    margin-bottom: 30px;
    img {
      width: 20%;
      filter: url('#f1');
    }
  }

  .controler {
    margin-bottom: 20px;
    .group {
      width: 60%;
      display: flex;
      justify-content: space-between;
      margin: 20px 0;
      .item {
        display: flex;
        label {
          margin-right: 10px;
        }
        .result {
          position: relative;
          width: 80px;
          border: 1px solid #eee;
          text-align: center;
          margin-left: 20px;
          margin-right: 20px;

          cursor: pointer;
          .pop {
            display: none;
            position: absolute;
            left: 80%;
            top: -20px;
            width: 500px;
            height: 50px;
            background-color: #fff;
            z-index: 10;
            padding-top: 20px;
          }
          &:hover{
            .pop {
              display: flex;
            }
          }
        }
        
      
      }
    }

    .slide-item{
      max-width: 500px;
      min-width: 300px;
      padding: 0 20px;
    }

  }

  .output{
    width: 100%;
    height: 100%;
    background-color: #333;
    color: #fff;
    padding: 20px;
    box-sizing: border-box;
    border-radius: 5px;
  }

}

.btn{
  margin: 20px 0;
}

</style>
