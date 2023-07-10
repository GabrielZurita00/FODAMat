<template>
    <div>
        <select v-model="selectedOption">
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
    }
}
</script>