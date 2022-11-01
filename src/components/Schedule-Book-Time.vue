<template>
  <div>
    <v-row dense justify="center" >
      <v-col class="time" v-for="(week, index) in data" :key="`week${index}`">
        <template v-for="(time, index ) in scheduleTime(week)">
          <div :key="`time${index}`">
            <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <div
                  class="mt-2 part-time"
                  :class="time.booked ? 'booked': ''"
                  v-bind="attrs"
                  v-on="on">
                  {{ time.time }}
                </div>
              </template>
              <span>{{ $t("bookCourse") }}</span>
            </v-tooltip>
          </div>
        </template>
      </v-col>
    </v-row>
  </div>
</template>

<script lang="ts" setup>
import { defineProps, PropType } from 'vue'
import dayjs from 'dayjs'

type Time = {
  start: string
  end: string
  booked: boolean
}

defineProps({
  data: {
      type: [] as PropType<Array<Array<Time>>>,
      required: true
  }
})

//==============
//
// methods
//

const partTime = ({start, end, booked}: Time) => {
  const startTime  = dayjs(start)
  const endTime = dayjs(end)
  const diff = endTime.diff(startTime, 'minute')
  let i = 0
  let arr = []
  
  while(i < diff){
    arr.push({
      time: startTime.add(i, 'minute').format('HH:mm'),
      booked
    })
    i += 30
  }
  return arr
}

const scheduleTime = (times: Array<Time>) =>{
  let arr = times.map(i => {
    return partTime(i)
  })
  return arr.flat()
}
</script>
<style scoped lang="scss">
.time{
  color: #02cab9;
  text-align: center;  
}
.booked{
  color: grey;
}
.part-time:hover{
  color: #02cab9;
  border: #02cab9 1px solid;
  cursor: pointer;
}
</style>
