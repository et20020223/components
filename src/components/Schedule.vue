<template>
  <div class="container">
    <v-row dense justify="space-between" class="mb-2">
      <v-col>
          <ScheduleButton 
            :button-type="'prev'" 
            :icon="'mdi-chevron-left'"
            :disabled="disableButton"
            @click="changeWeek">
          </ScheduleButton>
          <ScheduleButton 
            :button-type="'next'" 
            :icon="'mdi-chevron-right'"
            @click="changeWeek">
          </ScheduleButton>     
          <span class="week-range">{{ weekend }}</span>
      </v-col>
      <v-col class="time-zone-description">* {{ $t('timeZone', { timeZone: timeZone, zone: zone})}}</v-col>
    </v-row>
    <v-row dense justify="center" class="week-title">
      <v-col class="day">{{ $t("sun") }}</v-col>
      <v-col class="day">{{ $t("mon") }}</v-col>
      <v-col class="day">{{ $t("tue") }}</v-col>
      <v-col class="day">{{ $t("wed") }}</v-col>
      <v-col class="day">{{ $t("thu") }}</v-col>
      <v-col class="day">{{ $t("fri") }}</v-col>
      <v-col class="day">{{ $t("sat") }}</v-col>
    </v-row>
    <v-row dense justify="center" >
      <v-col v-for="num in 7" class="day" :key="num">{{ showDay(num -1 + day) }}</v-col>
    </v-row>
    <BookTime :data="scheduleData"></BookTime>
  </div>
</template>

<script lang="ts" setup>
import ScheduleButton from './Schedule-Button.vue'
import BookTime from './Schedule-Book-Time.vue'
import dayjs from 'dayjs'
import weekday from 'dayjs/plugin/weekday'
import { computed, onMounted, ref } from 'vue'

type Time = {
  start: string
  end: string
  booked: boolean
}

dayjs.extend(weekday)
const apiData = {"available":[{"start":"2022-10-31T14:00:00Z","end":"2022-10-31T15:00:00Z"},{"start":"2022-10-31T21:00:00Z","end":"2022-10-31T23:00:00Z"},{"start":"2022-11-01T12:00:00Z","end":"2022-11-01T12:30:00Z"},{"start":"2022-11-01T13:30:00Z","end":"2022-11-01T16:00:00Z"},{"start":"2022-11-01T21:00:00Z","end":"2022-11-02T00:30:00Z"},{"start":"2022-11-02T01:00:00Z","end":"2022-11-02T07:00:00Z"},{"start":"2022-11-02T15:00:00Z","end":"2022-11-02T16:00:00Z"},{"start":"2022-11-02T21:00:00Z","end":"2022-11-03T06:00:00Z"},{"start":"2022-11-03T07:00:00Z","end":"2022-11-03T07:30:00Z"},{"start":"2022-11-03T08:30:00Z","end":"2022-11-03T11:30:00Z"},{"start":"2022-11-03T12:00:00Z","end":"2022-11-03T12:30:00Z"},{"start":"2022-11-03T13:30:00Z","end":"2022-11-03T16:00:00Z"},{"start":"2022-11-03T21:00:00Z","end":"2022-11-04T01:00:00Z"},{"start":"2022-11-04T04:00:00Z","end":"2022-11-04T06:00:00Z"},{"start":"2022-11-04T08:00:00Z","end":"2022-11-04T14:00:00Z"},{"start":"2022-11-04T15:00:00Z","end":"2022-11-04T16:00:00Z"},{"start":"2022-11-04T21:00:00Z","end":"2022-11-04T22:30:00Z"},{"start":"2022-11-04T23:00:00Z","end":"2022-11-05T00:30:00Z"},{"start":"2022-11-05T03:30:00Z","end":"2022-11-05T11:30:00Z"},{"start":"2022-11-05T12:00:00Z","end":"2022-11-05T12:30:00Z"},{"start":"2022-11-05T14:00:00Z","end":"2022-11-05T16:00:00Z"}],"booked":[{"start":"2022-10-31T15:00:00Z","end":"2022-10-31T16:00:00Z"},{"start":"2022-11-01T12:30:00Z","end":"2022-11-01T13:30:00Z"},{"start":"2022-11-02T00:30:00Z","end":"2022-11-02T01:00:00Z"},{"start":"2022-11-03T06:00:00Z","end":"2022-11-03T07:00:00Z"},{"start":"2022-11-03T07:30:00Z","end":"2022-11-03T08:30:00Z"},{"start":"2022-11-03T11:30:00Z","end":"2022-11-03T12:00:00Z"},{"start":"2022-11-03T12:30:00Z","end":"2022-11-03T13:30:00Z"},{"start":"2022-11-04T06:00:00Z","end":"2022-11-04T08:00:00Z"},{"start":"2022-11-04T14:00:00Z","end":"2022-11-04T15:00:00Z"},{"start":"2022-11-04T22:30:00Z","end":"2022-11-04T23:00:00Z"},{"start":"2022-11-05T00:30:00Z","end":"2022-11-05T03:30:00Z"},{"start":"2022-11-05T11:30:00Z","end":"2022-11-05T12:00:00Z"},{"start":"2022-11-05T12:30:00Z","end":"2022-11-05T14:00:00Z"}]}
let scheduleData = ref([] as Array<Array<Time>>)
let day = ref(0)

//==============
//
// computed
//

const disableButton = computed(() =>{
  return dayjs().weekday(0).diff(dayjs().weekday(0 + day.value), 'day') === 0
})

const zone = computed(() =>{
  return '台北'
})

const timeZone = computed(() =>{
  return '+08:00'
})

const weekend = computed(() =>{
  return `${dayjs().weekday(0 + day.value).format('YYYY/MM/DD')}~${dayjs().weekday(6 + day.value).format('YYYY/MM/DD')}`
})

//==============
//
// mounted
//

onMounted(()=>{

  let data = concatData(apiData)

  data = data.sort( (a, b) => {
    return a.start > b.start ? 1 : -1
  })

  scheduleData.value = differentialteDate(data)
})

//==============
//
// methods
//

const changeWeek = (_day: number) => {
  day.value += _day
}

const differentialteDate = (data: Array<Time>) => {
  let scheduleData = []
  for(let i = 0 ; i < 7 ; i++){
    const targetDay = dayjs().weekday(i).format('YYYY-MM-DD')
    const temp = data.filter(i => i.start.indexOf(targetDay) > -1)
    scheduleData.push(temp)
  }

  return scheduleData
}

const concatData = (data: Record<string, Array<Omit<Time, 'booked'>>>) => {
   const available = data.available.map((i)=>{
    return {
      start: dayjs(i.start).format('YYYY-MM-DD HH:mm'),
      end: dayjs(i.end).format('YYYY-MM-DD HH:mm'),
      booked: false
    }
  })

  const booked = data.booked.map((i)=>{
    return {
      start: dayjs(i.start).format('YYYY-MM-DD HH:mm'),
      end: dayjs(i.end).format('YYYY-MM-DD HH:mm'),
      booked: true
    }
  })
  
  return available.concat(booked)
}

const showDay = (weekday: number) =>{
  return dayjs().weekday(weekday).format('DD')
}

</script>
<script lang="ts">
 export default {
    name: 'main-schedule'
}
</script>
<style scoped lang="scss">
.container{
  width: 50%;
}
.week-title{
   .day{
    &::before{
      content: '';
      display: block;
      margin-bottom: 6px;
      width: 100%;
      height: 4px;
      background-color: #02cab9;
    }
   }
}
.day{
  text-align: center;  
}
.week-range{
  margin-left: 10px;
}
.time-zone-description{
  text-align: end;
  font-size: 12px;
}
</style>
