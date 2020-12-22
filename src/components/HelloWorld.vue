<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <h1>{{count}}</h1>
    <h1>{{double}}</h1>
    <h1>{{greetings}}</h1>
    <h1>X: {{x}},Y: {{y}}</h1>
    <h1 v-if="loading">Loading!...</h1>
    <img v-if="loaded"
         :src="result.message">
    <br />
    <button @click="increase">👍+1</button><br />
    <button @click="updateGreeting">Update Title</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch } from 'vue';
import useMousePosition from '../hooks/useMousePosition';
import useURLLoader from '../hooks/useURLLoader';
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
}
interface DogType {
  message: string;
  status: string;
}
export default {
  name: 'App',
  components: {},
  setup() {
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
    const { result, loading, loaded } = useURLLoader<DogType>(
      'https://dog.ceo/api/breeds/image/random'
    );
    watch(result, () => {
      if (result.value) console.log(333333, result.value.message);
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
