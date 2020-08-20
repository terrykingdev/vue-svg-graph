<template>
  <div>
    <svg width="100%" height="100%" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 1600 900" xml:space="preserve" :style="getResponsiveStyle">
    <text text-anchor="middle" x="800" y="870" class="labels" v-text="labels.x"></text>
    <text text-anchor="middle" x="100" y="840" class="sublabels" v-text="labels.xstart"></text>
    <text text-anchor="middle" x="1500" y="840" class="sublabels" v-text="labels.xend"></text>

    <text v-for="(label,index) in yTicks" text-anchor="end" :key="index" x="90" :y="label.y" class="sublabels" v-text="label.text"></text>
    <text text-anchor="middle" x="30" y="450" transform="rotate(-90,30,450)" class="labels" v-text="labels.y"></text>

    <line x1="100" y1="800" x2="1500" y2="800" stroke="black" stroke-width="2"></line>
    <line x1="100" y1="800" x2="100" y2="100" stroke="black" stroke-width="2"></line>
    <circle v-for="(point,index) in points" :key="'c'+index" r=10 :cx="point.x" :cy="point.y"></circle>
    <line v-for="(line,index) in lines" :key="'l'+index" :x1="line.x1" :y1="line.y1" :x2="line.x2" :y2="line.y2" stroke="black" stroke-width="3"></line>
    </svg>
  </div>
</template>
 
<script>
 
export default {
    data: () => ({
      getResponsiveStyle: '',
      lines: [],
      points: [],
      yTicks: [],
    }),
    props: [
      'raw',
      'labels'
    ],
    mounted(){
      this.calc()
    },
    watch:{
      raw: function(){
        // Reprocess everything when the data changes
        this.calc()
      }
    },
    methods: {
      calc: function(){
        let maxValue = Math.max(...this.raw)
        maxValue = Math.ceil(maxValue/5)*5
        let yLabelScale = 700/5
        let ys=[]
        for(let i=0;i<6;i++){
          ys.push({y: 800-(i*yLabelScale), text:(maxValue*i/5).toString()})
        }
        this.yTicks=ys
        let yScale=700/maxValue
        let xScale=1400/this.raw.length
        let p = []
        let x=0
        for(let r in this.raw){
          p.push( { x: 100+(x*xScale), y: 800-(this.raw[r]*yScale) })
          x++
        }
        this.points = p

        let l = []
        for(let point=0;point<this.points.length-1;point++){
          l.push( { x1: this.points[point].x, y1: this.points[point].y,
                    x2: this.points[point+1].x, y2: this.points[point+1].y} )
        }
        this.lines = l
      }
    }
}
</script>

<style lang="css" scoped>
.labels{
  font: 40px sans-serif;
}
.sublabels{
  font: 30px sans-serif;
}
.circle {
  fill:solid;
  stroke:#aaaaaa;
  stroke-width: .25px;
  stroke-miterlimit:10;
}
.line {
  fill:solid;
  stroke:#3333ff;
  stroke-width: .25px;
  stroke-miterlimit:10;
}
</style>