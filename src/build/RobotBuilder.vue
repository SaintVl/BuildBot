<template>
  <div class="content">
    <div class="preview">
      <CollapsibleSection>
      <div class="preview-content">
        <div class="top-row">
            <img :src="selectedRobot.head.src" >
        </div>
        <div class="middle-row">
          <img :src="selectedRobot.leftArm.src" class="rotate-left">
          <img :src="selectedRobot.torso.src">
          <img :src="selectedRobot.rightArm.src" class="rotate-right">
        </div>
        <div class="bottom-row">
          <img :src="selectedRobot.base.src">
        </div>
        <button class="add-to-cart" @click="addToCart()">В КОРЗИНУ!</button>
      </div>
    </CollapsibleSection>
    </div>
  </div>
<div>
    <div class="top-row">
            <PartSelector 
                :parts="availableParts.heads" 
                position="top"
                @partSelected="(part) => selectedRobot.head = part"/>
          </div> 
        </div>
    <div class="middle-row">
        <div class="left part">
          <PartSelector
              :parts="availableParts.arms"
              position="left"
              @partSelected="(part) => selectedRobot.leftArm = part"/>
        </div>
        <div class="center part">
          <PartSelector
              :parts="availableParts.torsos"
              position="center"
              @partSelected="(part) => selectedRobot.torso = part"/>
        </div>
        <div class="right part">
          <PartSelector
              :parts="availableParts.arms"
              position="right"
              @partSelected="(part) => selectedRobot.rightArm = part"/>
        </div>
    </div>
    <div class="bottom-row">
        <div class="bottom part">
          <PartSelector
              :parts="availableParts.bases"
              position="bottom"
              @partSelected="(part) => selectedRobot.base = part"/>
        </div>
    </div>
    <div>
      <h3>КОРЗИНА</h3>
      <table>
        <thead>
          <tr>
            <th>РОБОТ</th>
            <th class="cost">ЦЕНА</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(robot, index) in cart" :key="index">
            <td>{{ robot.head.title }}</td>
            <td class="cost">{{ robot.cost }}</td>
          </tr>
        </tbody>
      </table>
    </div>



</template>

<script>
   import availableParts from '../data/parts'
   import PartSelector from './PartSelector.vue';
   import CollapsibleSection from './CollapsibleSection.vue';

   function getPreviosValidIndex(index, length){
      const decIndex = index -1;
      return decIndex < 0 ? length -1 : decIndex;
   }

   function getNextValidIndex(index, length){
      const incIndex = index +1;
      return incIndex > length -1 ? 0 : incIndex;
   }
    export default{
        name: 'RobotBuilder',
        components: {PartSelector, CollapsibleSection },
        data(){
            return{
                availableParts,
                cart: [],
                selectedRobot: {
                  head: {},
                  leftArm: {},
                  torso:{},
                  rightArm: {},
                  base: {},
            }
          }
        },
        computed:{
        },
        methods:{
          addToCart(){
            const robot = this.selectedRobot;
            const cost = robot.head.cost +
                         robot.leftArm.cost +
                         robot.torso.cost +
                         robot.rightArm.cost +
                         robot.base.cost;
                         this.cart.push(Object.assign({},robot,{cost}))
                         
          },
          selectNextHead(){
            this.selectedHeadIndex = getNextValidIndex (this.selectedHeadIndex, availableParts.heads.length)
          },
          selectPreviosHead(){
            this.selectedHeadIndex = getPreviosValidIndex (this.selectedHeadIndex, availableParts.heads.length)
          },
          selectNextBase(){
            this.selectedBaseIndex = getNextValidIndex(this.selectedBaseIndex, availableParts.bases.length);
          },
          selectPreviosBase(){
            this.selectedBaseIndex = getPreviosValidIndex(this.selectedBaseIndex, availableParts.bases.length);
          },
          selectNextTorsos(){
            this.selectedTorsoIndex = getNextValidIndex(this.selectedTorsoIndex, availableParts.torsos.length);
          },
          selectPreviosTorsos(){
            this.selectedTorsoIndex = getPreviosValidIndex(this.selectedTorsoIndex, availableParts.torsos.length);
          },
          selectNextLeftArm(){
            this.selectedLeftArmIndex = getNextValidIndex(this.selectedLeftArmIndex, availableParts.arms.length);
          },
          selectPreviosLeftArm(){
            this.selectedLeftArmIndex = getPreviosValidIndex(this.selectedLeftArmIndex, availableParts.arms.length);
          },
          selectNextRightArm(){
            this.selectedRightArmIndex = getNextValidIndex(this.selectedRightArmIndex, availableParts.arms.length);
          },
          selectPreviosRightArm(){
            this.selectedRightArmIndex = getPreviosValidIndex(this.selectedRightArmIndex, availableParts.arms.length);
          },
        }

    }
</script>

<style>
.part {
  position: relative;
  width:165px;
  height:165px;
  border: 3px solid #aaa;
}
.part {
  img {
    width:165px;
  }
}
.top-row {
  display:flex;
  justify-content: space-around;
}
.middle-row {
  display:flex;
  justify-content: center;
}
.bottom-row {
  display:flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector, .center .next-selector {
  opacity:0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}
.robot-name{
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}  
.sale{
  color: red;
}
.add-to-cart{
  position: absolute;
  right: 6px;
  width: 200px;
  padding: 3px;
  font-size: 16px;
}

td, th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}
.cost{
  text-align: right;
}
.preview{
  position: absolute;
  top: 80px;
  right: 350px;
  width: 210px;
  height: 210px;
  padding: 5px;
}
.preview-content{
  border: 1px solid #999;
}
.preview img{
  width: 50px;
  height: 50px;
}
.rotate-right{
  transform: rotate(90deg);
}
.rotate-left{
  transform: rotate(-90deg);
}
</style>
