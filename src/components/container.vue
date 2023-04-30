<template>
  <div
    class="container"
    :style="{ left: `${containerX}px`, top: `${containerY}px` }"
    @mousedown="startContainerDrag"
    @mousemove="containerDrag"
    @mouseup="endContainerDrag"
  >
  <span class="label">Container Box</span>

    <div
      class="draggable"
      :style="{ left: `${x}px`, top: `${y}px` }"
      @mousedown="startDrag"
      @mousemove="drag"
      @mouseup="endDrag"
    >
    <div class="content">
      <span class="content-label">Content Box</span>
   </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';

export default {
  name: 'DraggableElement',
  setup() {
    const dragging = ref(false);
    const startX = ref(0);
    const startY = ref(0);
    const x = ref(0);
    const y = ref(0);

    const startDrag = (event: MouseEvent) => {
      event.preventDefault();
      dragging.value = true;
      startX.value = event.clientX - x.value;
      startY.value = event.clientY - y.value;
    };

    const drag = (event: MouseEvent) => {
      event.preventDefault();
      containerDragging.value = false
      if(x.value < 0 || y.value < 0 || x.value > 225 || y.value > 210){
        dragging.value = false
        x.value = 2
        y.value = 2
      }
      if (dragging.value) {
        x.value = event.clientX - startX.value;
        y.value = event.clientY - startY.value;
      }
    };

    const endDrag = () => {
      dragging.value = false;
    };

    const containerDragging = ref(false);
    const containerStartX = ref(0);
    const containerStartY = ref(0);
    const containerX = ref(0);
    const containerY = ref(0);

    const startContainerDrag = (event: MouseEvent) => {
      event.preventDefault();
      containerDragging.value = true;
      containerStartX.value = event.clientX - containerX.value;
      containerStartY.value = event.clientY - containerY.value;
    };

    const containerDrag = (event: MouseEvent) => {
      event.preventDefault();
      if (containerDragging.value) {
        containerX.value = event.clientX - containerStartX.value;
        containerY.value = event.clientY - containerStartY.value;
      }
    };

    const endContainerDrag = () => {
      containerDragging.value = false;
    };

    return {
      dragging,
      startX,
      startY,
      x,
      y,
      startDrag,
      drag,
      endDrag,
      containerDragging,
      containerStartX,
      containerStartY,
      containerX,
      containerY,
      startContainerDrag,
      containerDrag,
      endContainerDrag,
    };
  },
};
</script>

<style  scoped>

.content  {
      @apply bg-black h-24 w-36 relative;
   }
 .content-label {
       @apply absolute -top-3 px-3 left-0 bg-black text-white text-xs
    }

.draggable {
  position: absolute;
  top: 50%;
  width: 100px;
  height: 100px;
  color: #fff;
  text-align: center;
  line-height: 100px;
  cursor: move;
}

.container {
        @apply w-96 h-80 bg-gray-300 flex items-center justify-center relative;
    }
    .label {
       @apply absolute -top-3 px-3 left-0 bg-gray-300 text-black text-xs;
    }

</style>
