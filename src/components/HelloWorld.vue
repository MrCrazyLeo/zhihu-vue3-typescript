<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <h1>{{count}}</h1>
    <h1>{{double}}</h1>
    <h1>{{greetings}}</h1>
    <h1>X: {{x}},Y: {{y}}</h1>
    <p>{{error}}</p>
    <Suspense>
      <template #default>
        <div>
          <h1>Promise的数字：</h1>
          <async-show />
          <h1>async/await的狗狗：</h1>
          <dog-show />
        </div>
      </template>
      <template #fallback>
        <h1>Suspense Loading...</h1>
      </template>
    </Suspense>
    <h1 v-if="loading">Loading!...</h1>
    <img v-if="loaded"
         :src="result[0].url">
    <br />
    <button @click="openModal">Open Modal</button><br />
    <modal :isOpen="modalIsOpen"
           @close-modal="onModalClose"> My Modal !!!!</modal>
    <button @click="increase">👍+1</button><br />
    <button @click="updateGreeting">Update Title</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch, onErrorCaptured } from 'vue';
import useMousePosition from '../hooks/useMousePosition';
import useURLLoader from '../hooks/useURLLoader';
import Modal from './Modal.vue';
import AsyncShow from './AsyncShow.vue';
import DogShow from './DogShow.vue';
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
}
interface DogType {
  message: string;
  status: string;
}
interface CatResult {
  id: string;
  url: string;
  width: number;
  height: number;
}
export default {
  name: 'App',
  components: {
    Modal,
    AsyncShow,
    DogShow,
  },
  setup() {
    const error = ref(null);
    onErrorCaptured((e: any) => {
      error.value = e;
      return true;
    });
    const data: DataProps = reactive({
      count: 0,
      increase: () => {
        data.count++;
      },
      double: computed(() => data.count * 2),
    });
    const greetings = ref('');
    const updateGreeting = () => {
      greetings.value += 'Hello! ';
    };
    const { x, y } = useMousePosition();
    const { result, loading, loaded } = useURLLoader<CatResult[]>(
      'https://api.thecatapi.com/v1/images/search?limit=1'
    );

    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const onModalClose = () => {
      modalIsOpen.value = false;
    };

    watch(result, () => {
      if (result.value) console.log(333333, result.value[0].id);
    });
    const refData = toRefs(data);
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      loaded,
      openModal,
      onModalClose,
      modalIsOpen,
      error,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1 {
  font-size: 2rem;
}
button {
  font-size: 2rem;
}
</style>
