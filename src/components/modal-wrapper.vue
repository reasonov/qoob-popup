<template>

  <div>
    <div :class="['modal', parentClass]">
      <slot></slot>
    </div>

    <div class="modal-close" :style="'height:' + windowHeight + 'px;'" @click="closeModal"></div>
  </div>

</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "modal-wrapper",
  props: {
    parentClass: {
      type: String,
      required: false
    }
  },
  emits: ['closeModal'],
  setup(props, { emit }) {
    const windowHeight = ref(0);

    function closeModal() {
      emit('closeModal');
    }

    onMounted(() => {
      windowHeight.value = document.body.clientHeight;
    })

    return { windowHeight, closeModal }
  }
}
</script>

<style scoped>
.modal {
  position: absolute;
  top: 100px;
  left: 50%;
  z-index: 5;

  width: 890px;
  box-sizing: border-box;
  padding: 60px;
  box-shadow: 0px 24px 120px -30px rgba(0, 0, 0, 0.15);
  border-radius: 30px;
  background-color: #fff;

  transform: translateX(-50%);
}

.new__modal {
  max-width: 650px;
}

.modal-close {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 4;

  width: 100%;
  height: 100%;
}
</style>