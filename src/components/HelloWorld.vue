<template>
  <p>{{counter}}</p>
  <p>{{douleCounter}}</p>
  <p ref="desc"></p>
  <!-- ModelOpen -->
  <ModelOpen></ModelOpen>
</template>

<script>
import { computed, reactive,onMounted, onUnmounted, ref, toRefs, watch } from 'vue'
import ModelOpen from './ModelButton.vue'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
 setup(){
    //counter
    const {counter,douleCounter} = useCounter()
    const msg = ref('some message')

    //使用元素引用
    const desc = ref(null)

    //侦听器
    watch(counter,(val,oldVal)=>{
      const p = desc.value
      p.textContent = `counter change from ${oldVal} to ${val}`
    })  

   return {counter,douleCounter,msg,desc}
 },
  components:{
    ModelOpen
  }
}

function useCounter(){
  const data = reactive({
     counter:1,
     douleCounter:computed(()=>data.counter*2)
   })

    let timer
    onMounted(() => {
      timer = setInterval(()=>{
        data.counter++;
      },1000)
    })
    onUnmounted(()=>{
      clearInterval(timer)
    })
    return toRefs(data)
}

</script>
