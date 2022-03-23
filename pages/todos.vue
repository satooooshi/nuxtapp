<!-- pages/index.vue -->
<template>
<v-app>
  <section class="container">
    <a class="btn btn--orange" @click="getMsg" >PUSH {{msg}}</a>
    <button href="" class="btn btn--orange" @click="getMsg" >PUSH {{msg}}</button>
    <h1>Todoリスト</h1>
    <div class="addArea">
      <input type="text" name="addName" v-model="content" placeholder="タスクを入力してください">
      <button class="button button--green" @click="insert">追加</button>
    </div>
    <div class="Filter">
      <button class="button button--gray" v-bind:class="{'is-active':(!find_flg)}" @click="flag_reset">全て</button>
      <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '作業前')}" @click="find('作業前')">作業前</button>
      <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '作業中')}" @click="find('作業中')">作業中</button>
      <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '完了')}" @click="find('完了')">完了</button>
    </div>
    <table class="Lists">
      <thead>
        <tr>
          <th>タスク</th>
          <th>登録日時</th>
          <th>状態</th>
          <th> . </th>
        </tr>
      </thead>
      <tbody>
<tr v-for="(item, index) in display_todos" :key="index">
  <td>{{ item.content }}</td>
  <td>{{ item.created }}</td>
  <td>
  <button class="button"
          :class="{
            'button--yet':item.state == '作業前',
            'button--progress':item.state == '作業中',
            'button--done':item.state == '完了'}"
          @click="changeState(item)">
    {{ item.state }}
  </button>
  </td>
  <!-- pages/index.vue -->
  <td><button class="button button--red" @click="remove(item)">削除</button></td>
</tr>
      </tbody>
    </table>
  </section>
</v-app>
</template>
<script>
import {mapState} from 'vuex';

export default {
  //
  data: function() {
    return {
      content:'', // v-model 
      find_state:'',
      find_flg:false,
      // axios test
      msg:'axios',
    }
  },
  //
  computed: {
    //  vuexからmapStateをインポートすることで、
    // dataにストアのstateの中身を入れ、
    // computedでtodosの中身が変わるたびに描写させるようにしています。
    ...mapState(['todos']), 

    // display filtered todos
    display_todos:function(){
      // find_flgがtrueならば、find_stateとstateが一致するタスクだけ表示し、
      // そうでなければ全て表示します。
      if(this.find_flg){
        let arr = [];
        // data is immutable now
        let data = this.todos;
        data.forEach(element =>{
          if(element.state == this.find_state){
            arr.push(element);
          }
        });
        return arr;
      }else{
        return this.todos;
      }
    }
  },
  //
  methods: {
    insert: function() {
      if(this.content != ''){
        this.$store.commit('insert', {content: this.content});
        this.content = '';
      }
    },
    remove: function(todo) {
      this.$store.commit('remove', todo)
    },
    changeState: function(todo){
      this.$store.commit('changeState',todo)
    },
    find: function(findState){
      this.find_state = findState;
      this.find_flg = true;
    },
    flag_reset: function(){
      this.find_flg = false;
    },
    // axios test
    getMsg () {
      this.$axios.$get('http://httpbin.org/uuid')
        .then(res =>{ console.log(res); this.msg=res.uuid;})
    },
    // routing
    movePage(){
      this.$router.push({ path: `user/one` });
    }
  },
  //
}
</script>
<style>
.button--yet {
    color: #0000FF;
    background-color: white;
}
.btn--orange,
a.btn--orange {
  color: #fff;
  background-color: #eb6100;
}
.btn--orange:hover,
a.btn--orange:hover {
  color: #fff;
  background: #f56500;
}
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

button {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}
button:hover {
  color: #fff;
  background-color: #35495e;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}
.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}
.button--red {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #a8172f;
  color: #b23030;
  text-decoration: none;
  padding: 10px 30px;
}
.button--red:hover {
  color: rgb(241, 238, 238);
  background-color: #dd428f;
}
.button--gray {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}
.button--gray:hover {
  color: #fff;
  background-color: #35495e;
}
</style>