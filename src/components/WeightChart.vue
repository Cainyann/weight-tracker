
<template>
    <div class="chartContainer">
        <canvas ref="canvas"></canvas>
    </div>
</template>

<script>
import Chart from 'chart.js/auto';
import { ref, nextTick, onMounted, watch,shallowRef} from 'vue';
export default {
name: "WeightChart",
props: { weights: Array },
setup(props) {
    // console.log('setup'); //只有第一次输入数据时执行

    //父组件设置了v-if，在第一次输入之后该组件挂载
    //在组件挂载之后获得对canvas的直接引用
    let canvas = ref(null)
    //注意chart要用shallowRef,否则Maximum call stack size exceeded
    //https://stackoverflow.com/questions/68602389/maximum-call-stack-error-when-attempting-to-update-chart-in-vue-js
    let wChart = shallowRef(null)
    onMounted(() => {
        canvas.value.focus()
        // console.log(canvas.value); //<canvas>

    //初始化新建chart
    if(wChart.value==null){
        wChart.value = new Chart(canvas.value.getContext("2d"), {
        type: 'line',
        // 数据集
        data: {
            labels: props.weights.sort((a, b) => a.date - b.date)
            .map(weight => new Date(weight.date).toLocaleDateString())
            .slice(-7),
            datasets: [{
                label: "weight",
                backgroundColor: ' rgba(182, 84, 231,0.3)',
                borderColor: ' rgb(182, 84, 231,1)',
                borderWidth: 1,
                data: props.weights.sort((a, b) => a.date - b.date)
                    .map(weight => weight.weight)
                    .slice(-7),
            }]
        },
        // 配置选项
        options: {
            responsive: true,
            maintainAspectRatio: false,
            fill:true
        },
    })}

    })



    
// 监听weights更新wCart
watch(props.weights,(newWeights)=>{
    // console.log('newweights');
    // console.log( wChart.value.data.labels);
    let weightsData = [...newWeights]

    //更新chart
    wChart.value.data.labels = weightsData
            .sort((a, b) => a.date - b.date)
            .map(weight => new Date(weight.date).toLocaleDateString())
            .slice(-7)

    wChart.value.data.datasets[0].data = weightsData
            .sort((a, b) => a.date - b.date)
            .map(weight => weight.weight)
            .slice(-7)
    wChart.value.update()
 })

    return{
        canvas
    }
}}
    

 


</script>

<style>
.chartContainer {
	width: 100%;
	max-width: 720px;
	background-color: white;
	padding: 1rem;
	border-radius: 0.5rem;
	box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
	margin-bottom: 2rem;
}

</style>


