<!-- eslint-disable vue/multi-word-component-names -->
<script>
import { computed } from 'vue'

export default {
  props: {
    matched: {
      type: Boolean,
      default: false
    },
    position: {
      type: Number,
      required: true
    },
    value: {
      type: String,
      required: true
    },
    visible: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const flippedStyle = computed(() => {
      if (props.visible) {
        return 'is-flipped'
      }
      return ''
    })

    const selectCard = () => {
      context.emit('select-card', {
        position: props.position,
        faceValue: props.value
      })
    }

    return {
      flippedStyle,
      selectCard
    }
  }
}
</script>

<template>
  <div
    class="card"
    :class="flippedStyle"
    @click="selectCard"
  >
    <div class="card-face is-front">
      <img
        :src="`/images/${value}.png`"
        :alt="value"
      />
      <img
        v-if="matched"
        src="/images/checkmark.svg"
        class="icon-checkmark"
      />
    </div>

    <div class="card-face is-back"></div>
  </div>
</template>

<style>
.card {
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  backface-visibility: hidden;
}
.card-face.is-front {
  background-image: url('/images/card-bg.png');
  color: white;
  transform: rotateY(180deg);
}

.card-face.is-back {
  background-image: url('/images/card-bg-empty.png');
  color: white;
}

.icon-checkmark {
  position: absolute;
  right: 5px;
  bottom: 5px;
}
</style>
