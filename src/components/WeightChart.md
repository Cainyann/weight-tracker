
<template>
    <div class="chartContainer">
        <canvas ref="canvas"></canvas>
    </div>
</template>

<script>
import Chart from 'chart.js/auto';
import { ref, nextTick, onMounted, watch } from 'vue';
export default {
name: "WeightChart",
props: { weights: Array },
setup(props) {
    // console.log(props.weights);
    //在canvas挂载之后获得对它的直接引用
    let canvas = ref(null)
    onMounted(() => {
        canvas.value.focus()
    })
    let wChart = ref(null)

    //监听父组件的weights，deep:true对对象进行深层监听
watch(props.weights,

(newWeights) => {
    console.log(newWeights);
    let weightsData = [...newWeights]
    





    // 非初次载入数据：已经由new Chart()创建过wChart，只需要更新
    if (wChart.value) {
        wChart.value.data.labels = weightsData
            .sort((a, b) => a.date - b.date)
            .map(weight => new Date(weight.date).toLocaleDateString())
            .slice(-7)

        wChart.value.data.datasets[0].data = weightsData
            .sort((a, b) => a.date - b.date)
            .map(weight => weight.weight)
            .slice(-7)

        wChart.value.update()
        return
    }


    nextTick(() => {
    // if(wChart){
    //    wChart.value.destroy()
    // }
    wChart.value = new Chart(canvas.value.getContext("2d"), {
        type: 'line',
        // 数据集
        data: {
            labels: weightsData.sort((a, b) => a.date - b.date)
                .map(weight => new Date(weight.date).toLocaleDateString())
                .slice(-7),
            datasets: [{
                label: "weight",
                backgroundColor: 'rgb(255, 99, 132)',
                // borderColor: 'rgb(255, 99, 132)',
                borderColor: 'rgba(255, 105, 180, 1)',
                borderWidth: 1,
                data: weightsData.sort((a, b) => a.date - b.date)
                    .map(weight => weight.weight)
                    .slice(-7),
            }]
        },
        // 配置选项
        options: {
            responsive: true,
            maintainAspectRatio: false,
            fill:true
        }});
    })

}, { deep: true })
return {wChart, canvas}
}
}
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


