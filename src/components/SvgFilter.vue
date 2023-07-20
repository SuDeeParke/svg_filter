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
        <feColorMatrix type="hueRotate" :values="`${(hueRotate)}`" />
      </filter>
    </defs>
  </svg>
  <main class="container">
    <h1>SvgFilter</h1>
    <section class="display">
      <div class="img-wrap">
        <img :src="imageUrl" alt="">
       
      </div>
       <div class="upload" >
            <el-upload
              drag
              ref="upload"
              action=""
              :show-file-list="false"  
              :limit="1"
              :on-exceed="handleExceed"
              :auto-upload="false"
              :on-change="handleChange"
              >
              <div class="el-upload__text">
                Drop file here or <em>click to upload</em>
              </div>
            </el-upload>
          </div>
    </section>
    <section class="controler">
      <el-collapse v-model="activeNames">
        <el-collapse-item title="颜色矩阵" name="1">
          <div class="group">
            <span style="color:#fff">&&nbsp;</span>
            <div class="item" v-for="(item) in vals">
              <p class="title">{{ item }}</p>
            </div>
          </div>
          <div class="group" v-for="(array, index) in matrix">
            {{ vals[index] }}
            <div class="item" v-for="(item) in array">
              <div class="result">
                {{ item.value }}
                <div class="pop">
                  <el-slider v-model="item.value" show-input size="small" :step="0.01" :min="-1" :max="1" />
                </div>
              </div>
            </div>
          </div>
          <div class="btn">
            <el-button plain @click="reset()" :size="'small'">重设</el-button>
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
            <el-button plain @click="resetAdvance()" :size="'small'">重设</el-button>
          </div>

        </el-collapse-item>
      </el-collapse>

    </section>
    <h5>结果</h5>
    <section class="output">
      <div> 颜色矩阵：{{ matrixResult }}</div>
      <div> 饱和度： {{ saturate }}</div>
      <div> 色相： {{ hueRotate }}</div>

    </section>
  </main>
</template>

<script lang="ts" setup>
import demoPic from '@/assets/images/demo.png';
import { ref, computed } from 'vue';
import { cloneDeep } from 'lodash';
import { genFileId } from 'element-plus'

const vals = ['R', 'G', 'B', 'A', 'offset']

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
  [{ id: 'RR', name: 'Red to red', value: 1 }, { id: 'GR', name: 'Green to red', value: 0 }, { id: 'BR', name: 'Blue to red', value: 0 }, { id: 'AR', name: 'Alpha to red', value: 0 }, { id: 'OR', name: 'red offset', value: 0 }],
  [{ id: 'RG', name: 'Red to green', value: 0 }, { id: 'GG', name: 'Green to green', value: 1 }, { id: 'BG', name: 'Blue to green', value: 0 }, { id: 'AG', name: 'Alpha to green', value: 0 }, { id: 'OG', name: 'green offset', value: 0 }],
  [{ id: 'RB', name: 'Red to blue', value: 0 }, { id: 'GB', name: 'Green to blue', value: 0 }, { id: 'BB', name: 'Blue to blue', value: 1 }, { id: 'AB', name: 'Alpha to blue', value: 0 }, { id: 'OB', name: 'blue offset', value: 0 }],
  [{ id: 'RA', name: 'Red to alpha', value: 0 }, { id: 'GA', name: 'Green to alpha', value: 0 }, { id: 'BA', name: 'Blue to alpha', value: 0 }, { id: 'AA', name: 'Alpha to alpha', value: 1 }, { id: 'OA', name: 'alpha offset', value: 0 }],
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

const upload = ref()
const handleExceed = (files: any) => {
  upload.value!.clearFiles()
  const file = files[0]
  file.uid = genFileId()
  upload.value!.handleStart(file)
}

const imageUrl = ref(demoPic)
const handleChange= (file:any, fileList:any) => {//选中文件触发的change事件
  const isJPG = file.raw.type === 'image/jpeg'
  const isPNG = file.raw.type === 'image/png'
  if (!isPNG && !isJPG) {
    return false
  } else {
    imageUrl.value = URL.createObjectURL(file.raw);//赋值图片的url，用于图片回显功能
  }
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
    align-items: flex-start;
    justify-content: start;
    margin-bottom: 30px;
    width: 100%;
    .img-wrap{
      position: relative;
      img {
        width: 400px;
        filter: url('#f1');
      }
    }
    .upload{
      display: block;
      align-items: center;
      justify-content: center;
      margin-left: 20px;
    }
    
  }

  .controler {
    margin-bottom: 20px;

    .group {
      min-width: 30%;
      max-width: 40%;
      display: flex;
      justify-content: space-between;
      margin: 20px 0;
      .item {
        display: flex;
        
        .title{
          width: 80px;
          text-align: center;
        }
        .result {
          position: relative;
          width: 80px;
          border: 1px solid #eee;
          text-align: center;

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

          &:hover {
            .pop {
              display: flex;
            }
          }
        }


      }
    }

    .slide-item {
      max-width: 500px;
      min-width: 300px;
      padding: 0 20px;
    }

  }

  .output {
    width: 100%;
    height: 100%;
    background-color: #333;
    color: #fff;
    padding: 20px;
    box-sizing: border-box;
    border-radius: 5px;
    margin-top: 21px;
  }

}

.btn {
  margin: 20px 0;
}
</style>
