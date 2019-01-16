<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart();">Add to Cart</button>
    <div class="top-row">
      <div class="top part">
        <div class="robot-name">
          {{ selectedRobot.head.title }}
          <span v-if="selectedRobot.head.onSale" class="sale">Sale!</span>
        </div>
        <img :src="selectedRobot.head.src" title="head"/>
        <button @click="selectPrevHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead();" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobot.leftArm.src" title="left arm"/>
        <button @click="selectPrevLeftArm();" class="prev-selector">&#9650;</button>
        <button @click="selectNextLeftArm();" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobot.torso.src" title="center part"/>
        <button @click="selectPrevTorso();" class="prev-selector">&#9668;</button>
        <button @click="selectNextTorso();" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobot.rightArm.src" title="right arm"/>
        <button @click="selectPrevRightArm();" class="prev-selector">&#9650;</button>
        <button @click="selectNextRightArm();" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobot.base.src" title="left arm"/>
        <button @click="selectPrevBase();" class="prev-selector">&#9668;</button>
        <button @click="selectNextBase();" class="next-selector">&#9658;</button>
      </div>
    </div>
  </div>
</template>

<script>
  import availableParts from '../../data/parts.js';

  function getPreviousValidIndex(index, length) {
    const deprecatedIndex = index - 1;
    return deprecatedIndex < 0 ? length - 1 : deprecatedIndex;
  }

  function getNextValidIndex(index, length) {
    const deprecatedIndex = index + 1;
    return deprecatedIndex > length - 1 ? 0 : deprecatedIndex;
  }

  export default {
    data() {
      return {
        availableParts,
        cart: [],
        selectHeadIndex: 0,
        selectLeftArmIndex: 0,
        selectRightArmIndex: 0,
        selectTorsoIndex: 0,
        selectBaseIndex: 0
      }
    },
    computed: {
      selectedRobot() {
        return {
          head: availableParts.heads[this.selectHeadIndex],
          leftArm: availableParts.arms[this.selectLeftArmIndex],
          rightArm: availableParts.arms[this.selectRightArmIndex],
          torso: availableParts.torsos[this.selectTorsoIndex],
          base: availableParts.bases[this.selectBaseIndex],
        }
      }
    },
    methods: {
      addToCart() {
        console.log('Cart before', this.cart);
        const robot = this.selectedRobot;
        const cost = robot.head.cost + robot.leftArm.cost + robot.rightArm.cost + robot.torso.cost + robot.base.cost;
        this.cart.push(Object.assign({}, robot, { cost }));
        console.log('Cart after', this.cart);
      },
      selectNextHead() {
        this.selectHeadIndex = getNextValidIndex(this.selectHeadIndex, this.availableParts.heads.length);
      },
      selectPrevHead() {
        this.selectHeadIndex = getPreviousValidIndex(this.selectHeadIndex, this.availableParts.heads.length);
      },
      selectNextLeftArm() {
        this.selectLeftArmIndex = getPreviousValidIndex(this.selectLeftArmIndex, this.availableParts.arms.length);
      },
      selectPrevLeftArm() {
        this.selectLeftArmIndex = getPreviousValidIndex(this.selectLeftArmIndex, this.availableParts.arms.length);
      },
      selectNextRightArm() {
        this.selectRightArmIndex = getPreviousValidIndex(this.selectRightArmIndex, this.availableParts.arms.length);
      },
      selectPrevRightArm() {
        this.selectRightArmIndex = getPreviousValidIndex(this.selectRightArmIndex, this.availableParts.arms.length);
      },
      selectNextTorso() {
        this.selectTorsoIndex = getPreviousValidIndex(this.selectTorsoIndex, this.availableParts.torsos.length);
      },
      selectPrevTorso() {
        this.selectTorsoIndex = getPreviousValidIndex(this.selectTorsoIndex, this.availableParts.torsos.length);
      },
      selectNextBase() {
        this.selectBaseIndex = getPreviousValidIndex(this.selectBaseIndex, this.availableParts.bases.length);
      }
    },
  }
</script>

<style scoped>
  .part {
  position: relative;
  width:165px;
  height:165px;
  border: 3px solid #aaa;
}
.part img {
  width:165px;
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
.robot-name {
  position: absolute;
  width: 100%;
  top: -25px;
  text-align: center;
}
.sale {
  color: red;
}
.content {
  position: relative;
}
.add-to-cart {
  position: absolute;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}
</style>
