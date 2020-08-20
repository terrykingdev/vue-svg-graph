<template>
  <v-app>
    <v-row>
      <v-col cols=12 sm=8 md=6 lg=4>
        <v-card elevation="15">
          <v-responsive :aspect-ratio="16/9">
            <v-card-text>
              <svg-graph :raw="points" :labels="{x:'Age in seconds',y:'Reads per interval',xstart:'0',xend:'59'}"></svg-graph>
            </v-card-text>
          </v-responsive>      
        </v-card>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import SvgGraph from './components/SvgGraph'

export default {
  name: 'App',
  components: {
    SvgGraph
  },
  data () {
    return {
      points: []
    }
  },
  created() {
    for(let i=0;i<100;i++){
      this.points.push(Math.ceil(Math.random()*100))
    }
  },
  mounted(){
    setInterval(() => {
      this.points.pop()
      this.points.unshift(Math.ceil(Math.random()*1000))
    },1000)
  } 
}
</script>


<style>
</style>
