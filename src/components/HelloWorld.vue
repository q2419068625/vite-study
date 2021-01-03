<template>
  <p>{{counter}}</p>
  <p>{{douleCounter}}</p>
  <p ref="desc"></p>
  <!-- ModelOpen -->
  <ModelButton></ModelButton>
  <Emits @click="onClick"></Emits>
  <comp></comp>
</template>

<script>
import { computed, reactive,onMounted, onUnmounted, ref, toRefs, watch } from 'vue'
import ModelButton from './ModelButton.vue'
import Emits from './Emits.vue'
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
 methods:{
    onClick(){
      console.log('click me');
    }
 },
  components:{
    ModelButton,
    Emits
  }

}

function useCounter(){
  const data = reactive({
     counter:1,
     douleCounter:computed(()=>data.counter*2)
   })

    // let timer
    // onMounted(() => {
    //   timer = setInterval(()=>{
    //     data.counter++;
    //   },1000)
    // })
    // onUnmounted(()=>{
    //   clearInterval(timer)
    // })
    return toRefs(data)
}

</script>
