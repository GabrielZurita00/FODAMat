<template>
    <div class="dropdown">
        <select v-model="selectedOption" @change="emitSelectedOption">
            <option v-for="option in options" :value="option" :key="option" :title="hint[option]"> {{ option }} </option>
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
            selectedOption: 0,
            hint: ["Sin relación","1","Relación muy débil","3","Relación débil","5","Relación media","7","Relación fuerte","9","Relación muy fuerte"]
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