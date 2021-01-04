<template>
  <p>{{ counter }}</p>
  <p>{{ douleCounter }}</p>
  <p ref="desc"></p>
  <!-- ModelOpen -->
  <ModelButton></ModelButton>
  <Emits @click="onClick"></Emits>
  <comp></comp>
  <!-- v-model使用 -->
  <VmodelTest v-model:counter="counter"></VmodelTest>
  <!-- 等效于 -->
  <!-- <VmodelTest :counter="counter" @update:counter="counter=$event"></VmodelTest> -->
  <!-- render api的变化 -->
  <RenderTest v-model:counter="counter">
    <template v-slot:default>xxxxx</template>
    <template v-slot:counter>counter.....</template>
  </RenderTest>
  <Functional level="3">这是一个动态的H元素</Functional>
</template>

<script>
import {
  computed,
  reactive,
  onMounted,
  onUnmounted,
  ref,
  toRefs,
  watch,
  h,
} from "vue";
import ModelButton from "./ModelButton.vue";
import VmodelTest from "./VmodelTest.vue";
import Emits from "./Emits.vue";
import Functional from "./Functional.vue";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      counter: 1,
    };
  },
  setup() {
    //counter
    const { counter, douleCounter } = useCounter();
    const msg = ref("some message");

    //使用元素引用
    const desc = ref(null);

    //侦听器
    watch(counter, (val, oldVal) => {
      const p = desc.value;
      p.textContent = `counter change from ${oldVal} to ${val}`;
    });

    return { counter, douleCounter, msg, desc };
  },
  methods: {
    onClick() {
      console.log("click me");
    },
  },
  components: {
    ModelButton,
    Emits,
    VmodelTest,
    Functional,
    RenderTest: {
      props: {
        counter: {
          type: Number,
          default: 0,
        }
      },
      render() {
        return h("div", [
          h("div", { onClick: this.onClick }, [
            `i am RenderTest${this.counter}`,
            this.$slots.counter(),
            this.$slots.default(),
          ])
        ]);
      },
      methods: {
        onClick() {
          this.$emit("update:counter", this.counter + 1);
        }
      }
    }
  }
};

function useCounter() {
  const data = reactive({
    counter: 1,
    douleCounter: computed(() => data.counter * 2),
  });

  // let timer
  // onMounted(() => {
  //   timer = setInterval(()=>{
  //     data.counter++;
  //   },1000)
  // })
  // onUnmounted(()=>{
  //   clearInterval(timer)
  // })
  return toRefs(data);
}
</script>
