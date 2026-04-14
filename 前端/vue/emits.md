子：
```js
<script setup> 

const emit = defineEmits(['send-data']) //  定义组件可以触发的事件 

const sendDataToParent = () => { 
	const data = { message: '你好，来自子组件！' }
	emit('send-data', data)  // 触发 'send-data' 事件
} 
</script> 

<template> 
<button @click="sendDataToParent"> 发送数据给父组件 </button>
</template>
```
---
父：
```js
<script setup> 
import { ref } from 'vue' import ChildComponent from './ChildComponent.vue' 

const messageFromChild = ref('') // 定义事件处理函数，接收子组件传来的数据 

const handleData = (data) => { messageFromChild.value = data.message } 
</script>

<template> 
<ChildComponent @send-data="handleData" /> // 监听子组件的 'send-data' 事件 

<p>父组件接收到的数据：{{ messageFromChild }}</p> 
</template>
```
**props+emits例**
父
```js
//App.vue
<template>
  <CounterButton :currentValue="count" :step="3" @request-increment="add"></CounterButton>
</template>

<script setup>
import { ref } from 'vue';
import CounterButton from './CounterButton.vue';
const count = ref(0);
const add = (stp) => {
  count.value += stp;
}
</script>
```
---
子
```js
// CounterButton.vue
<script setup>

const props = defineProps({

  currentValue: Number,

  step: Number

});

  

const emit = defineEmits(['request-increment']);

  

function handleClick() {

  emit('request-increment', props.step);

}

</script>

  

<template>

  <div class="box">

    <p>子组件显示当前值: {{ currentValue }}</p>

    <button @click="handleClick">点我增加 {{ step }}</button>

  </div>

</template>
```