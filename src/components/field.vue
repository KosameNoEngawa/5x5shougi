<template>
<div id='field-wrapper'>
  <div id='bord'>
    <div v-for='x in 5' v-bind:key='x' class='row'>
      <div v-for='y in 5' v-bind:key='y' class='clumn'>
        <div class='mass' 
        v-on:click="select_place(x,y)" 
        v-bind:style="next_place(x,y)" 
        v-bind:id='`${x}-$[y]`'>
          <button v-on:click="select_king(x,y)" v-show='king_position(x,y)'>
            王
          </button>
          <button v-bind:class='status' 
          v-on:click="enemy_select_king(x,y)" 
          v-show='enemy_king_position(x,y)'>
            玉
          </button>
          <button v-on:click="select_knight(x,y)" v-show='knight_position(x,y)'>
            騎
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      place: {
        king_x: 5,
        king_y: 3,
        knight_x: 5,
        knight_y: 4,
        enemy_king_x: 1,
        enemy_king_y: 3,
      },
      //コマが次に動けるマスを表示するかしないか
      //コマを押した時に値がtrueに
      //コマを動かし終わった時にfalseに
      select_king_status: false,
      select_knight_status: false,

      select_enemy_king_status: false,
      //コマを一番最初に押した時に値を変える
      king_piece_wait: false,
      knight_piece_wait: false,

      enemy_king_piece_wait: false,

      next_place_now: false,

      //敵のコマのクラスをenemyにする
      status: 'opponent',

      turn: 'supporter_turn', //敵の時はopponent_turn
    }
  },
  computed: {
  },
  methods: {
    //王の初期位置
    king_position: function(x,y) {
        if(x == this.place.king_x && y == this.place.king_y){
          this.king_piece_wait = false
          return true
        }
    },
    knight_position: function(x,y) {
        if(x == this.place.knight_x && y == this.place.knight_y){
          this.knight_piece_wait = false
          return true
        }
    },
    enemy_king_position: function(x,y) {
        if(x == this.place.enemy_king_x && y == this.place.enemy_king_y){
          this.enemy_king_piece_wait = false
          return true
        }
    },
    next_place: function(x,y) {
      if(this.select_king_status == true){
        if(x == this.place.king_x - 1 && y == this.place.king_y - 1 ||
          x == this.place.king_x - 1 && y == this.place.king_y ||
          x == this.place.king_x - 1 && y == this.place.king_y + 1 ||
          x == this.place.king_x && y == this.place.king_y - 1 ||
          x == this.place.king_x && y == this.place.king_y + 1 ||
          x == this.place.king_x + 1 && y == this.place.king_y - 1 ||
          x == this.place.king_x + 1 && y == this.place.king_y ||
          x == this.place.king_x + 1 && y == this.place.king_y + 1
        ) {
          return {backgroundColor: 'aqua'}
        }
      }
      if(this.select_knight_status == true){
        if(x == this.place.knight_x - 1 && y == this.place.knight_y - 1 ||
            x == this.place.knight_x - 1 && y == this.place.knight_y ||
            x == this.place.knight_x - 1 && y == this.place.knight_y + 1 ||
            x == this.place.knight_x + 1 && y == this.place.knight_y - 1 ||
            x == this.place.knight_x + 1 && y == this.place.knight_y + 1
          ) {
            return {backgroundColor: 'aqua'}
        }
      }
      if(this.select_enemy_king_status == true){
        if(x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y - 1 ||
          x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y ||
          x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y + 1 ||
          x == this.place.enemy_king_x && y == this.place.enemy_king_y - 1 ||
          x == this.place.enemy_king_x && y == this.place.enemy_king_y + 1 ||
          x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y - 1 ||
          x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y ||
          x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y + 1
        ) {
          return {backgroundColor: 'aqua'}
        }
      }
    },
    select_king: function(x,y) {
      this.select_knight_status = false
      this.select_enemy_king_status = false
      this.next_place(x,y)
      this.select_king_status = true
      return
    },
    select_knight: function(x,y) {
      this.select_king_status = false
      this.select_enemy_king_status = false
      this.next_place(x,y)
      this.select_knight_status = true
      return
    },
    enemy_select_king: function(x,y) {
      this.select_knight_status = false
      this.select_king_status = false
      this.next_place(x,y)
      this.select_enemy_king_status = true
      return
    },
    select_place: function(x,y) {
      if(x == this.place.king_x && y == this.place.king_y) {
        console.log('select_king');
        this.king_piece_wait = true
        return
      }
      if(x == this.place.knight_x && y == this.place.knight_y) {
        console.log('select_kingt');
        this.knight_piece_wait = true
        return
      }
      if(x == this.place.enemy_king_x && y == this.place.enemy_king_y) {
        console.log('select_enemy_king');
        this.enemy_king_piece_wait = true
        return
      }
      if(this.turn == 'supporter_turn') {
        if(this.king_piece_wait == true) {
          if(x == this.place.king_x - 1 && y == this.place.king_y - 1 ||
            x == this.place.king_x - 1 && y == this.place.king_y ||
            x == this.place.king_x - 1 && y == this.place.king_y + 1 ||
            x == this.place.king_x && y == this.place.king_y - 1 ||
            x == this.place.king_x && y == this.place.king_y + 1 ||
            x == this.place.king_x + 1 && y == this.place.king_y - 1 ||
            x == this.place.king_x + 1 && y == this.place.king_y ||
            x == this.place.king_x + 1 && y == this.place.king_y + 1
          ) {
            if(this.select_knight_status == false) {
              console.log('next_king');
              this.place.king_x = x
              this.place.king_y = y
              this.king_position(x,y)
              this.turn = 'opponent_turn'
            }
          }
        }
        if(this.knight_piece_wait == true) {
          if(x == this.place.knight_x - 1 && y == this.place.knight_y - 1 ||
            x == this.place.knight_x - 1 && y == this.place.knight_y ||
            x == this.place.knight_x - 1 && y == this.place.knight_y + 1 ||
            x == this.place.knight_x + 1 && y == this.place.knight_y - 1 ||
            x == this.place.knight_x + 1 && y == this.place.knight_y + 1
          ) {
            if(this.select_king_status == false) {
              console.log('next_knight');
              this.place.knight_x = x
              this.place.knight_y = y
              this.knight_position(x,y)
              this.turn = 'opponent_turn'
            }
          }
        }
      }
      if(this.turn == 'opponent_turn') {
        if(this.enemy_king_piece_wait == true) {
          if(x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y - 1 ||
            x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y ||
            x == this.place.enemy_king_x - 1 && y == this.place.enemy_king_y + 1 ||
            x == this.place.enemy_king_x && y == this.place.enemy_king_y - 1 ||
            x == this.place.enemy_king_x && y == this.place.enemy_king_y + 1 ||
            x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y - 1 ||
            x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y ||
            x == this.place.enemy_king_x + 1 && y == this.place.enemy_king_y + 1
          ) {
            if(this.select_knight_status == false) {
              console.log('next_enemy_king');
              this.place.enemy_king_x = x
              this.place.enemy_king_y = y
              this.enemy_king_position(x,y)
              this.turn = 'supporter_turn'
            }
          }
        }
      }

      this.select_king_status = false
      this.select_knight_status = false
      this.select_enemy_king_status = false
    },
  }
}
</script>
<style>
#bord {
  margin: 0 auto;
  width: 510px;
  height: 510px;
  border: 1px solid #333333;
}
.opponent {
  transform:rotate(180deg);    /* 中心を基準点に時計回りに回転する */
}
.row {
  display: flex;
}
.mass {
  width: 100px;
  height: 100px;
  border: 1px solid #333333;
  font-size: 0px;
}
#king {
  font-size: 30px;
}
</style>