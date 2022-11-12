<template>
    <form @submit.prevent="handleAdd">
        <input v-model="inputRef" type="number"/>
        <input type="submit" value="Add weight">
    </form>

</template>

<script>
import {ref} from 'vue'
export default {
    name:"InputForm",
    props:{
        addWeight:Function
    },
    setup(props){
        let {addWeight} = props
        let inputRef = ref(null)  //表单input使用v-model实现双向数据绑定
        //向父组件传递inputRef.value
        function handleAdd(){
            if(inputRef.value==null){
                return alert('please input your weight')
            }
        
            addWeight(inputRef.value)
            inputRef.value = null //清空input框
        }
        return{
            inputRef,addWeight,handleAdd
        }
    }
}
</script>

<style>
form {
	display: flex;
	margin-bottom: 2rem;
	border: 1px solid #AAA;
	border-radius: 0.5rem;
	overflow: hidden;
	transition: 200ms linear;
}
form:hover {
	border-color:rgb(225, 129, 237);
	border-width: 2px;
}
form input[type="number"] {
	/* appearance: none; */
	outline: none;
	border: none;
	background-color: white;

	flex: 1;
	padding: 1rem 1.5rem;
	font-size: 1.25rem;
}

form input[type="submit"] {
    width: 140px;
    text-align: center;
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	background-color: rgb(182, 84, 231);


	color: white;
	font-size: 1.25rem;
	font-weight: 700;
	transition: 200ms linear;
	border-left: 3px solid transparent;
}
</style>
