<template>
  <v-hover v-slot="{ hover }">
    <v-btn 
      depressed 
      color="#f5f5f5"
      @click="changeWeek()"
      :outlined="!disabled"
      :disabled="disabled"
      :style="{ 'background-color': hover ? '#02cab9' : '#ffffff' }">
        <v-icon 
          :color="hover ? '#ffffff':'#02cab9'" 
          small>
          {{ icon }}
        </v-icon>
    </v-btn>
  </v-hover>
</template>
  
<script lang="ts" setup>
import { defineProps, defineEmits, PropType } from 'vue'

const props = defineProps({
  icon: {
      type: String,
      required: true
  },
  disabled: {
      type: Boolean,
      required: false,
      default: false
  },
  buttonType: {
      type: String as PropType<'prev' | 'next'>,
      required: true
  }
})

const emit = defineEmits({
  click: (_day: number) => true
})

//==============
//
// methods
//

const changeWeek = () => {
    if(props.buttonType === 'next'){
      emit('click', 7)
    } else {
      emit('click', -7)
    }
}
</script>

  
  