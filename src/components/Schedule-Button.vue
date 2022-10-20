<template>
            <v-hover v-slot="{ hover }">
              <v-btn 
              depressed 
              outlined 
              color="#f5f5f5"
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
  import { defineProps } from 'vue'

  defineProps({
    icon: {
        type: String,
        required: true
    }
  })
  </script>
  

  
  