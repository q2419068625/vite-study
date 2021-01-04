<script>
//vue3中函数式组件只能用函数式方式声明
import {h} from 'vue'
function Heading(props,context) {
    console.log(props,context);
    return h(`h${props.level}`,context.attrs,context.slots)
}
Heading.props = ['level']
export default Heading
</script>