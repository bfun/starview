<template>
  <lay-layout class="example">
    <lay-header>
      <lay-form class="form-row">
        <lay-form-item>
          <lay-select id="portSelect" v-model="port" @search="portSearch" allow-clear :show-search="true" placeholder="端口">
            <lay-select-option v-for="item in ports" :key="item" :value="item">
              {{ item }}
            </lay-select-option>
          </lay-select>
        </lay-form-item>
        <lay-form-item>
          <lay-select id="dtaSelect" v-model="dta" @search="dtaSearch" allow-clear :show-search="true" placeholder="DTA">
            <lay-select-option v-for="item in dtas" :key="item" :value="item">
              {{ item }}
            </lay-select-option>
          </lay-select>
        </lay-form-item>
        <lay-form-item>
          <lay-select id="codeSelect" v-model="code" @search="codeSearch" allow-clear :show-search="true" placeholder="交易码">
            <lay-select-option v-for="item in codes" :key="item" :value="item">
              {{ item }}
            </lay-select-option>
          </lay-select>
        </lay-form-item>
      </lay-form>
    </lay-header>
    <lay-body></lay-body>
    <lay-footer></lay-footer>
  </lay-layout>
</template>

<script setup>
import { ref, reactive,onMounted, fetch } from 'vue';
import axios from 'axios';

const data = () => ({
  portSelectedValue: '',
  ports: ["5500", "5501", "5502", "6600", "65535"],
  dtas: ['ABCS_SVR','POBS_SVR','NPOBS_SVR'],
  codes: ['1001001100','1001001111','2003400114','02005700'],
});
let ports = data().ports;
let dtas = data().dtas;
let codes = data().codes;
const port = ref('');
const dta = ref('');
const code = ref('');
// const model = reactive({});
const portSearch = (val) => {
  console.log(val,data().ports)
  if (val.trim() !== '') {
    ports = data().ports.filter(v => v.includes(val))
  }else{
    ports = data().ports
  }
};
const dtaSearch = (val) => {
  console.log(val,data().dtas)
  if (val.trim() !== '') {
    dtas = data().dtas.filter(v => v.includes(val.toUpperCase()))
  }else{
    dtas = data().dtas
  }
};
const codeSearch = (val) => {
  if (val.trim() !== '') {
    codes = data().codes.filter(v => v.includes(val))
  }else{
    codes = data().codes
  }
};
onMounted(async () => {
  await axios.get('http://locahost:8080/svrs').then((response) => {
    data().ports = []
    data().dtas = []
    data().codes = []
    response.data.forEach((item) => {
      data().ports.push(item.Port)
      data().dtas.push(item.Name)
    })
  })
  .catch(error => {
    console.error('Error fetching data: ', error);
  })
});
</script>
<style>
.form-row {
  display: flex; /* 使用 Flexbox 布局 */
  flex-wrap: wrap; /* 允许换行 */
  justify-content: space-evenly;
  /* space-between; 项目之间的间距平均分布 */
  /* 或者使用 justify-content: flex-start; 让项目紧挨着左边排列 */
}
@media screen and (min-width: 768px) {
  .form-row {
    width: 750px;
  }
}
@media screen and (min-width: 992px) {
  .form-row {
    width: 970px;
  }
}
</style>