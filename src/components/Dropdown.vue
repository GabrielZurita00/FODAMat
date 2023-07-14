<template>
    <div class="dropdown">
        <select v-model="selectedOption" @change="emitSelectedOption">
            <option v-for="option in options" :value="option" :key="option"> {{ option }} </option>
        </select>
    </div>
</template>

<script>
import '@fontsource/poppins';
export default {
    props: {
        options: {
            type: Array,
            required: true
        },
        matrixValue: {
            type: Number,
            required: true
        }
    },
    data() {
        return{
            selectedOption: 0
        }
    },
    watch:{
        matrixValue(newValue){
            this.selectedOption=newValue
        }
    },
    methods: {
        emitSelectedOption(){
            this.$emit('option-selected',this.selectedOption)
        }
    },
    mounted (){
        this.selectedOption=this.matrixValue
    }
}
</script>

<style>
select{
    position: relative;
    font-family: 'Poppins', sans-serif;
    font-size: 15px;
    border: 2px solid;
    border-radius: 5px;
}

</style>