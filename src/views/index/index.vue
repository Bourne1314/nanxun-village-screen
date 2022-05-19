<template>
  <div id="index">
    <div class="bg" >
      <dv-loading v-if="loading">Loading...</dv-loading>
      <div v-else  class="host-body">
        <div class="headline">
          <div></div>
          <div class="title-body">
            {{countryName}}村数字管理平台
          </div>
          <div class="title-body2">
            <div class="box">
              <p class="boxDay">{{ dateWeek }}  {{ dateDay }}  {{ dateYear }}</p>
              <!--              <p class="boxTime"></p>-->
              <!--              <p class="boxDate"></p>-->
            </div>
          </div>

        </div>
        <div class="body-box">
          <!-- 第三行数据 -->
          <div class="content-box">
            <div>
              <!--              <dv-border-box-12>-->
              <centerLeft1 />
              <!--              </dv-border-box-12>-->
            </div>
            <div>
              <!--              <dv-border-box-12>-->
              <centerLeft2 />
              <!--              </dv-border-box-12>-->
            </div>
            <!-- 中间 -->
            <!--            <div>-->
            <!--              <center />-->
            <!--            </div>-->
            <!-- 中间 -->
            <div>
              <centerRight2 />
            </div>
            <!--            <div>-->
            <!--              <dv-border-box-13>-->
            <!--                <centerRight1 />-->
            <!--              </dv-border-box-13>-->
            <!--            </div>-->
          </div>

          <!-- 第四行数据 -->
          <div class="bottom-box">
            <!--            <dv-border-box-13>-->
            <bottom1/>
            <!--            </dv-border-box-13>-->
            <!--            <dv-border-box-12>-->
            <bottom2 />
            <bottom3 />
            <bottom4 />
            <!--            </dv-border-box-12>-->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  reactive,
  onMounted,
  onUnmounted,
} from 'vue'
import { formatTime } from '@/utils/index'
import { WEEK } from '@/constant/index'
import useDraw from '@/utils/useDraw'
import { title, subtitle, moduleInfo } from '@/constant/index'
import CenterLeft1 from '../centerLeft1/index.vue'
import CenterLeft2 from '../centerLeft2/index.vue'
import Center from '../center/index.vue'
import CenterRight1 from '../centerRight1/index.vue'
import CenterRight2 from '../centerRight2/index.vue'
import Bottom1 from '../bottom1/index.vue'
import Bottom2 from '../bottom2/index.vue'
import Bottom3 from '../bottom3/index.vue'
import Bottom4 from '../bottom4/index.vue'

export default defineComponent({
  components: {
    CenterLeft1,
    CenterLeft2,
    Center,
    CenterRight1,
    CenterRight2,
    Bottom1,
    Bottom2,
    Bottom3,
    Bottom4,
  },
  setup() {
    // * 颜色
    const decorationColors = ['#568aea', '#000000']
    // * 加载标识
    const loading = ref<boolean>(true)
    // * 时间内容
    const timeInfo = reactive({
      setInterval: 0,
      dateDay: '',
      dateYear: '',
      dateWeek: ''
    })
    // * 适配处理
    const { appRef, calcRate, windowDraw, unWindowDraw } = useDraw()
    // 生命周期
    onMounted(() => {
      cancelLoading()
      handleTime()
      // todo 屏幕适应
      windowDraw()
      calcRate()
      // timeFn()
    })

    onUnmounted(() => {
      unWindowDraw()
      clearInterval(timeInfo.setInterval)
    })

    // methods
    // todo 处理 loading 展示
    const cancelLoading = () => {
      setTimeout(() => {
        loading.value = false
      }, 500)
    }
    // function timeFn (){
    //     this.timing = setInterval(() => {
    //     this.dateDay = formatTime(new Date(), 'HH: mm: ss')
    //     this.dateYear = formatTime(new Date(), 'yyyy-MM-dd')
    //     this.dateWeek = this.weekday[new Date().getDay()]
    //   }, 1000)
    // }

    // todo 处理时间监听
    const handleTime = () => {
      timeInfo.setInterval = setInterval(() => {
        const date = new Date()
        timeInfo.dateDay = formatTime(date, 'HH: mm: ss')
        timeInfo.dateYear = formatTime(date, 'yyyy-MM-dd')
        timeInfo.dateWeek = WEEK[date.getDay()]
      }, 1000)
    }

    // return
    return {

      dateDay: null,
      dateYear: null,
      dateWeek: null,
      weekday: ['周日', '周一', '周二', '周三', '周四', '周五', '周六'],
      countryName: '安吉某',
      loading,
      timeInfo,
      appRef,
      title,
      subtitle,
      moduleInfo
    }
  }
})
</script>

<style lang="scss">
@import '@/assets/scss/index.scss';
</style>
