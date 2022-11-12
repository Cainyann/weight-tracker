<script>
export default {
    name: "WeightHistory",
    props: { 
        weights: Array,
        deleteWeight:Function
     },
    setup(props) {
        let weights = props.weights.sort((a,b)=>a.date-b.date)
        return {
            weights
        }
    }
}
</script>

<template>
    <div class="weight-history">
        <ul>
            <li v-for="item in weights">
                <span>{{ new Date(item.date).toLocaleDateString() }}</span>
                <span>{{ item.weight }} kg</span>

                <!-- <span @click="deleteWeight(item.id)">x</span>
                <span>reset</span> -->


            </li>
        </ul>
    </div>
</template>


<style>
.weight-history ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.weight-history ul li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem;
    cursor: pointer;
}

.weight-history ul li:nth-child(even) {
    background-color: #dfdfdf;
}

/* .weight-history ul li:hover {
	background-color: #f8f8f8;
} */

/* .weight-history ul li:last-of-type {
	border-bottom: none;
} */

.weight-history ul li span {
    display: block;
    font-size: 1rem;
    font-weight: 400;
    margin-right: 1rem;
    color: #888;
    font-style: italic;
}
</style>