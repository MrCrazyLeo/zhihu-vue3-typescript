<template>
  <teleport to="#modal">
    <div v-if="isOpen"
         class="modal-container">
      <h2>
        <slot>this is a modal</slot>
      </h2>
      <button @click="buttonClick">Close</button>
    </div>
  </teleport>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
  props: {
    isOpen: Boolean,
  },
  emits: {
    'close-modal': null,
  },
  setup(props, context) {
    const buttonClick = () => {
      console.log('close！！！');
      context.emit('close-modal');
    };
    return {
      buttonClick,
    };
  },
});
</script>
<style >
#modal .modal-container {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: white;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
}
#modal .modal-container h2 {
  display: block;
  text-align: center;
}
#modal .modal-container button {
  display: block;
  margin: 0 auto;
}
</style>
