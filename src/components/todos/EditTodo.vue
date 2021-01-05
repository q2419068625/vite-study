<template>
     <input type="text" 
        :value="todoTitle"
        v-bind="$attrs"
        @input="onInputChange"
        />
</template>

<script>
import { reactive, toRefs } from 'vue'

export default {
    props: {
        todoTitle: {
            type: String,
            default: ''
        }
    },
    methods: {
        onInputChange(e) {
            this.$emit('update:todoTitle',e.target.value)
        }
    },
}
</script>

<style  scoped>

</style>