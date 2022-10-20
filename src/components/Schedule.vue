<template>
  <div class="container">
    <v-row dense justify="space-between" class="mb-2">
      <v-col>
          <ScheduleButton :icon="'mdi-chevron-left'"></ScheduleButton>
          <ScheduleButton :icon="'mdi-chevron-right'"></ScheduleButton>     
          <span class="week-range">2022/10/16~2022/10/22</span>
      </v-col>
      <v-col class="time-zone-description">* 時間以 台北 (GMT+08:00) 顯示</v-col>
    </v-row>
    <v-row dense justify="center" class="week-title">
      <v-col class="day">{{ $t("sun") }}</v-col>
      <v-col class="day">一</v-col>
      <v-col class="day">二</v-col>
      <v-col class="day">三</v-col>
      <v-col class="day">四</v-col>
      <v-col class="day">五</v-col>
      <v-col class="day">六</v-col>
    </v-row>
    <v-row dense justify="center" >
      <v-col class="day">16</v-col>
      <v-col class="day">17</v-col>
      <v-col class="day">18</v-col>
      <v-col class="day">19</v-col>
      <v-col class="day">20</v-col>
      <v-col class="day">21</v-col>
      <v-col class="day">22</v-col>
    </v-row>
    <v-row dense justify="center" >
      <v-col class="time"></v-col>
      <v-col class="time"></v-col>
      <v-col class="time"></v-col>
      <v-col class="time">19:00</v-col>
      <v-col class="time"></v-col>
      <v-col class="time"></v-col>
      <v-col class="time"></v-col>
    </v-row>
  </div>
</template>

<script lang="ts" setup>
import ScheduleButton from './Schedule-Button.vue'

</script>
<script lang="ts" >
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

.btn-prev,
.btn-next{
//  border: 1px solid #e6e6e6;
//  border-radius: 5px;
//  color: #02cab9;
//  width: 60px;
 font-size: 16px;
//  padding: 5px;
//  cursor: pointer;
//   &:hover {
//     background-color: #02cab9;
//     color: #FFF;
//   }
}

.week-range{
  margin-left: 10px;
}
.time-zone-description{
  text-align: end;
  font-size: 12px;
}
.time{
  color: #02cab9;
  text-align: center;  
}
</style>
