<template>
    <view class="chart-wrapper" v-if="data.length">
        <ff-canvas 
            class="chart" 
            id="chart-pub" 
            canvas-id="chart-pub" 
            :opts="opts"
            :onInit="initChart">
        </ff-canvas>
    </view>
</template>
 
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import ffCanvas from '@/components/f2-canvas/index.vue'
import F2 from '@/components/f2-canvas/lib/f2.min.js'

let that: any = null

@Component({
    name: 'demo',
    components: {
        ffCanvas
    }
})
export default class extends Vue {
    private data: any[] = []
    private opts = {}

    private created() {
        that = this
    }

    private async onLoad() {
        this.data = await this.getData()
    }

    private getData() {
        return Promise.resolve([
            { genre: 'Sports', sold: 275 },
            { genre: 'Strategy', sold: 115 },
            { genre: 'Action', sold: 120 },
            { genre: 'Shooter', sold: 350 },
            { genre: 'Other', sold: 150 }
        ])
    }

    private initChart(config: any) {
        // Step 1: 创建 Chart 对象
        const chart = new F2.Chart(config)

        // Step 2: 载入数据源
        chart.source(that.data)

        // Step 3：创建图形语法，绘制柱状图，由 genre 和 sold 两个属性决定图形位置，genre 映射至 x 轴，sold 映射至 y 轴
        chart.interval()
        .position('genre*sold')
        .color('genre')

        // Step 4: 渲染图表
        chart.render()

        return chart
    }
}
</script>
 
<style scoped lang="scss">
    .chart-wrapper {
        width: 100%;
        height: 250px;
    }
</style>