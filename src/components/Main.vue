<template>
  <div>
    <div class="nav justify-content-center mt-3">
      <h1 class="pl-4 mb-0 align-self-center" style="font-size: 24px;">Freecell</h1>
      <div class="d-flex mx-auto ">
        <p>0</p>
        <img class="align-self-center px-4" src="/img/Diamond.svg" alt="" style="height: 25px;">
        <p>0:10</p>
      </div>
      <div class="menu">
        <img class="m-3" src="/img/Menu.svg" alt="" style="width:24px;">
      </div>
    </div>
    <div class="container">
      <div class="topBlock d-flex">
        <div class="row">
          <div class="col-2 leftTopBlock" v-for="(item, index) in leftTopBlock" :key="index">
            <div class="topBlockCard"></div>
          </div>
          <div class="col-2 rightTopBlock" v-for="(items, index) in rightTopBlock" :key="index + 4">
            <div class="topBlockCard rightTopBlockText"></div>
          </div>
        </div>
      </div>
      <div class="belowBlock">
        <div class="row">
          <div class="col-2" v-for="(item, index) in belowBlock" :key="index" @dragstart='dragstart(index)' @dragenter='dragenter(index)' @dragend='dragend'>
            <div v-for="(item, index) in item" :key="index" draggable="true">
              <img class="poker" :src="distinguish(item)" alt="" :value='item'>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      leftTopBlock: Array.from(new Array(4)),
      rightTopBlock: [[], [], [], []],
      belowBlock: [
        [], // 7 張
        [], // 7 張
        [], // 7 張
        [], // 7 張
        [], // 6 張
        [], // 6 張
        [], // 6 張
        [] // 6 張
      ],
      timeMachine: [],
      poker: {},
      dragBlockNumbar: null,
      dropBlockNumbar: null,
      dragNunbar: null,
      // mobile: {
      //   from: {
      //     blockNumbar: this.dragBlockNumbar
      //   },
      //   to: {
      //     blockNumbar: this.dropBlockNumbar
      //   }
      // }
    }
  },
  methods: {
    init () {
      const vm = this
      vm.leftTopBlock = Array.from(new Array(4))
      vm.rightTopBlock = Array.from(new Array(4)).map((item) => {
        return new Array(0)
      })
      vm.poker = Array.from(new Array(52)).map((_, index) => {
        return index + 1
      })
      vm.shuffle(vm.poker)
      this.distribute()
    },
    shuffle (array) {
      for (let i = array.length - 1; i >= 0; i--) {
        let j = Math.floor(Math.random() * (i + 1))
        let temp = array[i]
        array[i] = array[j]
        array[j] = temp
      }
    },
    distribute () {
      const vm = this
      vm.belowBlock = Array.from(new Array(8)).map((item, index) => {
        if (index < 4) {
          let temp = []
          for (let i = 0; i < 7; i++) {
            temp.push(vm.poker.shift())
          }
          return temp
        } else {
          let temp = []
          for (let i = 0; i < 6; i++) {
            temp.push(vm.poker.shift())
          }
          return temp
        }
      })
    },
    distinguish (item) {
      let number = (item + 1) % 13
      if (number === 0) { number = 13 }
      let suit = ''
      if (item < 13) {
        suit = 'Spades'
      } else if (item > 12 && item < 26) {
        suit = 'Hearts'
      } else if (item > 25 && item < 39) {
        suit = 'Diamonds'
      } else if (item > 38) {
        suit = 'Clubs'
      }
      // console.log(number)
      return `/img/${number}_${suit}.svg`
    },
    dragstart (index) {
      const vm = this
      
      vm.dragBlockNumbar = index
    },
    dragenter (index) {
      const vm = this
      console.log(e)
      vm.dropBlockNumbar = index
    },
    dragend (e) {
      const vm = this
      console.log(e)
      vm.dragNunbar = vm.belowBlock[vm.dragBlockNumbar].pop()
      vm.belowBlock[vm.dropBlockNumbar].push(vm.dragNunbar)
    }
  },
  computed: {
  },
  created () {
    this.init()
    // console.log(this.belowBlock)
  }
}
</script>
