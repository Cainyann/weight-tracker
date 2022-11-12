<template>
  <div class="main">
  <h1>Weight Tracker</h1>

  <div class="current">
    current weight<span>{{ currentWeight.weight }}</span>(kg)
  </div>

  <InputForm :addWeight="addWeight" />

  <div v-if="weights.length!==0"><!-- 初始状态 weights为[]时不显示-->
    <h2>Last 7 days</h2>
    <WeightChart :weights="weights" /> <!-- 把weights传递WeightChart -->
    <h2>Weight History</h2>
    <WeightHistory :weights="weights" :deleteWeight="deleteWeight"/>
  </div>
</div>
</template>



<script>
import { reactive, ref, computed } from 'vue';
import WeightChart from './components/WeightChart.vue'
import WeightHistory from './components/WeightHistory.vue';
import InputForm from './components/InputForm.vue';
export default {
  name: 'App',
  components: { WeightChart, WeightHistory, InputForm },
  setup() {
    let weights = reactive([]); //全部体重

    //上方最近一次的体重
    let currentWeight = computed(() => {
      return weights.length > 0 ? weights.sort((a, b) => b.date - a.date)[0] : { weight: 0 }
    })

    //用于添加当前体重
    //传给子组件InputForm进行回调
    function addWeight(inputNumber) {
      let newWeight = {
        weight: inputNumber,
        date: new Date().getTime(),
        id:new Date()
      }
      weights.push(newWeight)
    }

    //用于删除单个weight项目
    function deleteWeight(id){
      console.log(id);
      // return weights = computed(() => {
      //   return weights.filter(weight=>weight.id !== id)
      // })
      weights = weights.filter(weight=>weight.id !== id)
      console.log(weights);
    }

    

    return {
      currentWeight, weights,
      addWeight,deleteWeight
    }
  }

}
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
}

body {
  background-color: #eee;
}

.main {
  padding: 1.5rem;
  margin-top: 10px;

}
h1 {
  color: rgb(181, 70, 237);
  margin-bottom: 1rem;
}
h2 {
  margin-bottom: 1rem;
  color: #888;
  font-weight: 400;
}

.current {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: 200px;
  height: 200px;

  text-align: center;
  background-color: white;
  border-radius: 999px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border: 5px solid rgb(165, 60, 217);

  margin: 0 auto 2rem;
}

.current span {
  display: block;
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 0.5rem;
}


</style>
