<template>
    <div :class="{active: isActive}" @click="navigate">
        {{route.name}}
    </div>
</template>

<script>
import {RouterLink, useLink} from 'vue-router'

export default {
    props: {
        ...RouterLink.props,
        inactiveClass: String
    },
    setup (props) {
        //获取RouterLink 内部的属性和方法
        const {route, href, isActive, isExactActive, navigate} = useLink(props)
        // console.log(route, href, isActive, isExactActive, navigate);
        return {route, isActive, navigate}
    }
}
</script>

<style  scoped>
.active{
    background-color: #3fb983;
}
</style>