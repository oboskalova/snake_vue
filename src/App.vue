<template>
<div >
 <div class="wall">
  <snake v-for="e in arr"   :x="e[0]" :y="e[1]" />
 <food :xF="xF" :yF="yF" />
</div>
<table class="table" >
    <tr >
        <td colspan="3" >  </td> 
        <td class="direction" id="up" @click="moveUp" >  up  </td>
        <td></td>
    </tr>
    <tr>
        <td > <div id="result" > {{speed}} </div> </td>
        <td >
            <div class="direction" id="plus" @click="faster">+</div>

            <div class="direction" id="minus" @click="slower" >-</div>
        </td>
        <td class="direction" id="left" @click="moveLeft" @keyup.left="moveLeft" > left </td>
        <td > </td>
        <td class="direction" id="right" @click="moveRight" > right </td>
    </tr>
    <tr>
        <td id="count"> {{count}} </td>
        <td colspan="2" > </td>
        <td class="direction" id="down" @click="moveDown"> down </td>
    </tr>
</table>
<div class="direction" id="start" @click="starter">
   <b>start</b>
 </div>
</div>
</template>

<script>
import snake from "./components/snake"
import food from "./components/food"
export default {
  components: {
    snake,
    food
  },
  data () {
    return {
      variable: 3,
      x: 200,
      y: 200,
      xF: 300,
      yF: 300,
      speed: 1,
      timeID: null,
      started: false,
      count: 0,
      arr: [[200, 200]],
      eat : false,
      
    }
  },
  mounted(){
     document.onkeyup = this.OnKeyUpMy;

     
  },
  methods: {
    OnKeyUpMy(e){
    if(this.started){
      if(e.which===38){
        this.moveUp()
      }
      if(e.which===39){
         this.moveRight()
      }
      if(e.which===40){
         this.moveDown()
      }
      if(e.which===37){
         this.moveLeft()
      }
    }
    },
    randomiseFood () {
      this.xF = Math.floor(Math.random() * 395)
      this.xF+=5-this.xF%5;
      this.yF =Math.floor( Math.random() * 395)
      this.yF+=5-this.yF%5;
      console.log(this.x, this.y)
    },
    isEaten(){

      if(this.x==this.xF&&this.y==this.yF){
      this.count+=1
      this.arr.unshift([this.xF, this.yF])
      console.log(this.arr)
      this.randomiseFood()
      this.eat=true
      }
    },
    moveUp () {
      clearTimeout(this.timerId)
      if(this.started){
      this.timerId = setInterval(() => { this.moveUp() }, 
      1000-(this.speed-1)*100);
      this.y -= 5
      this.isEaten()
       this.movingTogether()
       
      if(this.y<0){
       this.death()
      }  
        console.log(this.x, this.y)
      }
    },
    moveDown () {
      clearTimeout(this.timerId)
      
      if(this.started){
      this.timerId = setInterval(() => { this.moveDown() }, 
      1000-(this.speed-1)*100)
      this.y += 5
      this.isEaten()
      this.movingTogether()
      if(this.y>=400){
        this.death()
      }  
        console.log(this.x, this.y)
      }
    },
    moveRight () {
      clearTimeout(this.timerId)
      if(this.started){
      this.timerId = setInterval(() => { this.moveRight() }, 
      1000-(this.speed-1)*100);
      this.x += 5
      this.isEaten()
      this.movingTogether()
      
      if(this.x>=400){
      this.death()
      }  
        console.log(this.arr[0])
        console.log(this.x, this.y)
      }
    },
    moveLeft () {
      clearTimeout(this.timerId)
      
      if(this.started){
      this.timerId = setInterval(() => { this.moveLeft() }, 
      1000-(this.speed-1)*100);
      this.x -= 5
      this.isEaten()
      this.movingTogether()
     
      if(this.x<0){
      this.death()
      }  
        console.log(this.x, this.y)
      }
    },
    
    faster () {
      if(this.speed<=9 && this.started)
         this.speed+=1
        console.log(this.x, this.y)
    },
    slower () {
      if(this.speed>0 && this.started)
        this.speed-=1
        console.log(this.x, this.y)
    }, 
    starter(){
      
      this.count = 0
      this.started = true
      this.randomiseFood()
      this.x=200
      this.y=200
      this.speed = 1
      this.moveRight()
      console.log(this.speed)
      this.eat=false
    },
    movingTogether(){
      if(!this.eat){
      for(var k=this.arr.length - 1; k>1;--k)
      {
     
        this.arr.splice(k,1,this.arr[k-1])
     
      }
      this.arr.splice(0,1, [this.x, this.y])
      
      }
      this.eat=false
      console.log('arr', this.arr.length, this.arr)

    },
    death(){
      
      this.started = false
       clearTimeout(this.timerId)
       alert("выход за границу, конец игры")
    }

  }
}



</script>

<style>
.table{
  border: 1px solid black;

}
#food{
  position: absolute;
  width: 5px;
  height: 5px;
  background: darkred;
  padding: 0px;
  left: 300px;
  top: 300px;
}
.wall {
  width: 400px;
  height: 400px;
  background: lightcyan no-repeat;
  position: relative;
  border: 3px solid black;
}
#snake {
  position: absolute;
  width: 5px;
  height: 5px;
  background: #000000;
  padding: 0px;
  left: 200px;
  top: 200px;
  z-index: 10;
}
#plus{
  height: 22px;
  width: 20px;
  
}
#minus{
  height: 22px;
  width: 20px;
 
}
.direction
{
  width: 40px;
  height: 42px;
  background-color: chartreuse;
  text-align: center;
  border: 1px solid black;
}

#result {
  border: 1px solid black;
  width: 40px;
  height: 30px;
  text-align: center;
  vertical-align: center;
  padding-top: 15px;
  background-color: aquamarine;
}
#count {
  border: 1px solid black;
  width: 38px;
  height: 30px;
  text-align: center;
  vertical-align: center;
  padding-top: 10px;
  background-color: aquamarine;
}
#start{
 width: 212px;
 height: 40px;
 font-size:20px;
 text-align: center;
 background-color: red;
 padding-top: 5px;
}
</style>
