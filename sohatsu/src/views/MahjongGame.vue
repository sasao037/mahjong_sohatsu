<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <h2>Mahjong Game 「索發」</h2>
        </v-row>
        <v-row>
        <v-col cols="4">
          <v-btn @click="startGame">Game Start</v-btn>
        </v-col>
        <v-col cols="4">
          <v-btn>Reset</v-btn>
        </v-col>
        <v-col cols="4">
          <v-btn>History</v-btn>
        </v-col>
        </v-row>

        <v-row>
          <v-sheet
            color="success"
            elevation="20"
            height="400"
            width="1000"
          >
            <v-row>
              <v-col cols="1">
                <img :src="ka1" v-if="kawa1 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka2" v-if="kawa2 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka3" v-if="kawa3 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka4" v-if="kawa4 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka5" v-if="kawa5 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka6" v-if="kawa6 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka7" v-if="kawa7 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka8" v-if="kawa8 == true">
              </v-col>
              <v-col cols="1">
                <img :src="ka9" v-if="kawa9 == true">
              </v-col>
            </v-row>

            <v-row class="justify-center">
              <v-col cols="1">
                <img class="display1" :src="te1" v-if="display1 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display2" :src="te2" v-if="display2 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display3" :src="te3" v-if="display3 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display4" :src="te4" v-if="display4 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display5" :src="te5" v-if="display5 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display6" :src="te6" v-if="display6 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display7" :src="te7" v-if="display7 == true" @click="replace($event)">
              </v-col>
              <v-col cols="1">
                <img class="display8" :src="te8" v-if="display8 == true" @click="replace($event)">
              </v-col>
            </v-row>
          </v-sheet>
        </v-row>

        <v-row>
          <v-col cols="4">
            <v-btn @click="finish">ツモ</v-btn>
          </v-col>
          <v-col cols="4">
            <v-btn>ロン</v-btn>
          </v-col>
          <v-col cols="4">
            <v-btn>ポン</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { ref } from 'vue'

export default {
  name: "MahjongGame",
  components: {
  },
  setup() {
    const img1 = ref()
    const img2 = ref()
    const img3 = ref()
    const img4 = ref()
    const img5 = ref()
    const img5a = ref()
    const img6 = ref()
    const img7 = ref()
    const img8 = ref()
    const img9 = ref()
    const imgh = ref()

    img1.value = require("@/assets/img/so1.gif")
    img2.value = require("@/assets/img/so2.gif")
    img3.value = require("@/assets/img/so3.gif")
    img4.value = require("@/assets/img/so4.gif")
    img5.value = require("@/assets/img/so5.gif")
    img5a.value = require("@/assets/img/so5a.gif")
    img6.value = require("@/assets/img/so6.gif")
    img7.value = require("@/assets/img/so7.gif")
    img8.value = require("@/assets/img/so8.gif")
    img9.value = require("@/assets/img/so9.gif")
    imgh.value = require("@/assets/img/h.gif")

    const dic = { 1:img1, 2:img2, 3:img3, 4:img4, 5:img5,
                  6:img6, 7:img7, 8:img8, 9:img9, "h":imgh }
    return {
        dic
    }
  },

  data() {
    return {
      display1: true,
      display2: true,
      display3: true,
      display4: true,
      display5: true,
      display6: true,
      display7: true,
      display8: true,

      kawa1: false,
      kawa2: false,
      kawa3: false,
      kawa4: false,
      kawa5: false,
      kawa6: false,
      kawa7: false,
      kawa8: false,
      kawa9: false,

      test: [],
      te1: null,
      te2: null,
      te3: null,
      te4: null,
      te5: null,
      te6: null,
      te7: null,
      te8: null,

      ka1: null,
      ka2: null,
      ka3: null,
      ka4: null,
      ka5: null,
      ka6: null,
      ka7: null,
      ka8: null,
      ka9: null,

      turn: 1,

      yama: [],

      han: 0,
      points: 0,
      yaku: [],
      yakuman: [],

    }
  },

  mounted() {
  },

  methods: {
    startGame() {
      // 牌をランダムにする
      this.yama = []
      var species = [1, 2, 3, 4, 5, 6, 7, 8, 9, "h"]
      for (var i=0; i<2; i++) {
        species = species.concat(species)
      }

      while (species.length > 0) {
        var n = species.length
        var k = Math.floor(Math.random() * n)
        this.yama.push(species[k])
        species.splice(k, 1)
      }

      // 手牌を並び替える
      this.test = this.yama.splice(0, 8)
      this.test = this.test.filter(item => item != "h")
      this.test.sort((first, second) => first - second)
      var count = 8 - this.test.length
      for (var i=0; i<count; i++) {
        this.test.push("h")
      }

      this.te1 = this.dic[this.test[0]]
      this.te2 = this.dic[this.test[1]]
      this.te3 = this.dic[this.test[2]]
      this.te4 = this.dic[this.test[3]]
      this.te5 = this.dic[this.test[4]]
      this.te6 = this.dic[this.test[5]]
      this.te7 = this.dic[this.test[6]]
      this.te8 = this.dic[this.test[7]]
    },

    replace(event) {
      // 選択した牌を非表示にする
      this[event.target.className] = false

      // 河に選択した牌を表示する
      const position = event.target.className.replace("display", "te")
      var ka = "ka" + this.turn
      var kawa = "kawa" + this.turn
      this[ka] = this[position]
      this[kawa]= true
      this.turn += 1

      // 牌を並び替える
      this.test.splice(Number(position.replace("te", ""))-1, 1)
      var te = this.test
      this.test = te
      this.test = this.test.filter(item => item != "h")
      this.test.sort((first, second) => first - second)
      var count = 7 - this.test.length
      for (var i=0; i<count; i++) {
        this.test.push("h")
      }
      this[event.target.className] = true
      this.display8 = false

      this.te1 = this.dic[this.test[0]]
      this.te2 = this.dic[this.test[1]]
      this.te3 = this.dic[this.test[2]]
      this.te4 = this.dic[this.test[3]]
      this.te5 = this.dic[this.test[4]]
      this.te6 = this.dic[this.test[5]]
      this.te7 = this.dic[this.test[6]]

      // 山から牌を引く
      var draw = this.yama.shift()
      this.te8 = this.dic[draw]
      this.display8 = true
      this.test.push(draw)
    },

    finish() {
      const before = this.test
      const fin = this.test[7]
      // 手牌を並び替える
      this.test = this.test.filter(item => item != "h")
      this.test.sort((first, second) => first - second)
      var count = 8 - this.test.length
      for (var i=0; i<count; i++) {
        this.test.push("h")
      }
      // console.log(before)
      // console.log(fin)
      console.log(`手牌: [${this.test}]`)


      // 各牌を数える
      var species = [1, 2, 3, 4, 5, 6, 7, 8, 9, "h"]
      var counts = []

      for (var i=0; i<species.length; i++) {
        var count = this.test.filter(item => item == species[i]).length
        counts.push(count)
      }
      console.log(`各牌の枚数: [${counts}]`)

      // 發
      if (counts[9] == 3) {
        this.yaku.push("發")
        this.han += 1
      }

      // 断么
      if (counts[0] + counts[8] + counts[9] == 0) {
        this.yaku.push("断么")
        this.han += 1
      }

      // 全帯么九系
      if (counts[0] + counts[8] + counts[9] == 8 ) {
        this.yaku.push("混老頭")
        this.han += 2
      }

      // 緑一色
      if (counts[0] + counts[4] + counts[6] + counts[8] == 0) {
        this.yakuman.push("緑一色")
        this.han += 13
      }

      // 紅孔雀
      if (counts[1] + counts[2] + counts[3] + counts[5] + counts[7] + counts[9] == 0) {
        this.yakuman.push("紅孔雀")
        this.han += 13
      }

      // 対子、刻子を数える
      var nums = []

      for (var i = 0; i < 5; i++) {
        var num = counts.filter(item => item == i).length
        nums.push(num)
      }
      console.log(`各枚数の牌種: [${nums}]`)

      // 暗刻系
      if(nums[3] == 2 && nums[2] == 1) {
        this.yaku.push("対々和")
        this.han += 2
        if (species[counts.indexOf(2)] == fin) {
          this.yakuman.push("二暗刻単騎")
          this.han += 13
        }
      }

      // 四対子
      if (nums[2] * 2 + nums[4] * 4 == 8) {
        this.yaku.push("四対子")
        this.han += 2


        if (
          (this.test[0] + 1 == this.test[2] && this.test[2] == this.test[4] - 1) ||
          (this.test[2] + 1 == this.test[4] && this.test[4] == this.test[6] - 1) ) {
          this.yaku.push("一盃口")
          this.han += 1
        }
        if (nums[4] == 1) {
          if (this.test[0] + 1 == this.test[2] && this.test[4] == this.test[6] - 1 ) {
            this.yaku.push("一盃口")
            this.han += 1
          }
        }
      }

      // 平和系
      if (counts[9] == 0) {
        if (nums[1] == 6 && nums[2] == 1) {
          var removeHand = this.test.filter(item => item != species[counts.indexOf(2)])
          var b1 = removeHand.splice(0, 3)
          var b2 = removeHand
          if (
            (b1[0] + 1 == b1[1] && b1[1] == b1[2] - 1) &&
            (b2[0] + 1 == b2[1] && b2[1] == b2[2] - 1) ) {
            if (
              (species[counts.indexOf(2)] != fin) &&
              (b1[1] != fin || b2[1] != fin)  &&
              ((b1[2] != 3 && fin == 3) || (b2[0] != 7 && fin == 7)) ) {
              this.yaku.push("平和")
              this.han += 1
            }
          }
        }
      }
      if (nums[1] == 4 && nums[2] == 2) {

      }

      if (this.han >= 13 && this.han <= 25) {
        this.han = 13
      } else if (this.han <= 38) {
        this.han = 26
      }
      console.log(`翻数: ${this.han}翻`)

      if (this.han >= 13) {
        console.log(`${this.yakuman}`)
      } else {
      console.log(`${this.yaku}`)
      }

      if (this.han == 1) {
        this.points = 1000;
      } else if (this.han == 2) {
        this.points = 2000;
      } else if (this.han == 3) {
        this.points = 4000;
      } else if (this.han <= 5) {
        this.points = 8000;
        console.log("満貫")
      } else if (this.han <= 7) {
        this.points = 12000;
        console.log("跳満")
      } else if (this.han <= 10) {
        this.points = 18000;
        console.log("倍満")
      } else if (this.han <= 12) {
        this.points = 24000;
        console.log("三倍満")
      } else if (this.han <= 25) {
        this.points = 32000;
        console.log("役満")
      } else if (this.han <= 38) {
        this.points = 64000;
        console.log("二倍役満")
      }
      console.log(`点数: ${this.points}点`)
    },
  },
};
</script>

<style scoped>
::v-deep .v-application--wrap {
  min-height: 0px !important;
}
</style>
