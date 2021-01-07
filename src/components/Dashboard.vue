<template>
    <div>
        <ul>
            <li>
                <!-- <router-link to="/">dashbord</router-link> -->
                <NavLink to="/"></NavLink>
            </li>
            <li>
                <!-- <router-link to="/todos">todos</router-link> -->
                <NavLink to="/todos"></NavLink>
            </li>
             <li>
                <!-- <router-link to="/about">about</router-link> -->
                <NavLink to="/about"></NavLink>
            </li>
        </ul>
        <router-view></router-view>
    </div>
</template>

<script>
import NavLink from './NavLink.vue'

export default {
    setup () {
        

        return {}
    },
    components: {
        NavLink
    }
}
</script>

<style  scoped>

</style>