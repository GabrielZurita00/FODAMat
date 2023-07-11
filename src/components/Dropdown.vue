<template>
    <div>
        <select v-model="selectedOption" @change="emitSelectedOption">
            <option v-for="option in options" :value="option" :key="option"> {{ option }} </option>
        </select>
    </div>
</template>

<script>
export default {
    props: {
        options: {
            type: Array,
            required: true
        }
    },
    data() {
        return{
            selectedOption: 0
        }
    },
    methods: {
        emitSelectedOption(){
            this.$emit('option-selected',this.selectedOption)
        }
    }
}
</script>