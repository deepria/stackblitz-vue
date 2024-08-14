<script setup>
import { ref, onMounted, nextTick,computed,reactive} from 'vue'

// 반응적인 상태의 속성
const count = ref(0)
let rawHtml = ref("이것은 빨간색이어야 합니다.");
const dynamicId = ref("deepria");
let isButtonDisabled = ref(false);
 const obj = ref({
  nested: { count: 0 },
  arr: []
})

let awesome = ref(false);

const now = computed(() => Date.now())

const isActive = ref(true)
const error = ref(null)

const classObject = computed(() => ({
  active: isActive.value && !error.value,
  'text-danger': error.value && error.value.type === 'fatal'
}))

async function mutateDeeply() {
  // 예상대로 작동합니다
  obj.value.nested.count++
  obj.value.arr.push(obj.value.nested.count)
  await nextTick()
  // 이제 DOM이 업데이트되었습니다.
}
const elements = computed(() => {
  const str = obj.value.arr.concat().join(" ")
  return str
})

function reset(){
  obj.value.nested.count = 0
  obj.value.arr = []
}

// 속성 값을 변경하고 업데이트 할 수 있는 함수.
function increment() {
  count.value++
}

function toggleTxt() {
  isButtonDisabled ? false : true;
}

 // 생명 주기 훅
onMounted(() => {
  console.log(`숫자 세기의 초기값은 ${ count.value } 입니다.`)
  
})

const colorful = reactive({
  color: '#ff0099',
  fontSize: '30px'
})

function makeitdarker(){

  const colorNow = parseInt(colorful.color.slice(3)) + 1;
  colorful.color = '#'+ colorful.color.slice(1,4) + colorNow;
 }

</script>

<template>
   <button @click="increment">숫자 세기: {{ count }}</button>
  <p>텍스트 보간법 사용: {{ rawHtml }}</p>
<p>v-html 디렉티브 사용: <span v-html="rawHtml"></span></p>

<div v-bind:id="dynamicId">
  <button @click="makeitdarker">makeitdarker</button>
  <div>{{colorful.color}}</div>
  <div :style="colorful"> {{now}}</div>
 

</div>
 
  <button :disabled="isButtonDisabled">버튼</button>
  <button @click="toggleTxt">토글</button>
  <span v-if="isButtonDisabled">아에이오우</span>
  {{isButtonDisabled ? "트루":"폴스"}}
  <div>
    <button @click="mutateDeeply">btn</button>
    <button @click="reset">reset</button>
    <br>
    {{obj.nested.count}}
    <br>
<span>{{elements}}</span>
    
      </div>

<div :class="classObject">
<button @click="awesome = !awesome">전환</button>

<h1 v-if="awesome">Vue는 정말 멋지죠!</h1>
<h1 v-else>아닌가요? 😢</h1>

</div>

   
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
