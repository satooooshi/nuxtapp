<!-- pages/index.vue -->
<template>
<v-app>
  <section class="container">
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
<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'
import commerce from "~/common/commerce";

interface Task {
  content: string
  created: string
  state: string
}

export default defineComponent({
  setup() {

    const context = useContext()
    // state
    const state = reactive({
      content: '' as string,
      display_todos: [
        {content: 'テスト', created: '2020-04-30 17:00', state: '作業前'}, 
        {content: 'コーディング', created: '2020-04-30 16:00', state: '作業中'},
        {content: '環境構築', created: '2020-04-30 15:30', state: '完了'}
      ] as Task[],
      find_state:'' as string,
      find_flag:false as boolean,
      msg:''
    })

    // mounted
    onMounted(() => {
      console.log('Component is mounted!')
      context.$axios.$get('http://httpbin.org/uuid')
        .then(res =>{ console.log(res);})

      // https://cloudpack.media/57935
      interface Obj {
        name: string;
        age: number;
      }
      const myObj: Obj = {
        name: "Hanako",
      age: 18,
      };
      console.log(myObj)

      type Location = string;
      let location: Location = "Saitama";

      const sampleFunc = (arg: string): string => {
        return arg;
      };
      console.log(sampleFunc("hello world@!"))

      interface posOptions {
        xPos?: number; // xPos : number | undefined
        yPos?: number; // xPos : number | undefined
      }
      const getPosition = (opts: posOptions) => {
      // xPost と yPos が undefined になる可能性も考えた処理を書く必要がある
      let xPos = opts.xPos === undefined ? 0 : opts.xPos;
      let yPos = opts.yPos === undefined ? 0 : opts.yPos;
      console.log("(${xPos}, ${yPos})")
      console.log(xPos)
      };
      getPosition({}); // 引数がなくても関数を呼び出せる
      getPosition({ xPos: 1 });
      getPosition({ xPos: 1, yPos: 1 });

      console.log(process.env.NUXT_ENV_CHEC_PUBLIC_API_KEY)
      commerce.products.list().then((product) => console.log(product));

    })

    


    const insert = function() {
      if(state.content != ''){
          var d = new Date();
          var fmt = d.getFullYear()
                  + '-' + ('00' + (d.getMonth() + 1)).slice(-2)
                  + '-' + ('00' + d.getDate()).slice(-2)
                  + ' ' + ('00' + d.getHours()).slice(-2)
                  + ':' + ('00' + d.getMinutes()).slice(-2);
          state.display_todos.push({
            content: state.content,
            created: fmt.toString(),
            state: '作業前'
          })
        state.content = '';
      }
    }
    const remove = (todo:Task) => {
            for(let i = 0; i < state.display_todos.length; i++) {
              const ob = state.display_todos[i];
              if(ob.content == todo.content && ob.created == todo.created) {
                if(confirm('"' + ob.content + '"を削除しますか？')){
                  state.display_todos.splice(i, 1);
                  //console.log(state.display_todos.splice(i, 1))
                  return;
                }
              }
            }
    }

    const changeState = (todo:Task) => {

    }

    const flag_reset = () => {

    }
    const find = () => {

    }

    return {
        ...toRefs(state),
        insert,
        remove,
        changeState,
        flag_reset,
        find,
    }
  },
})
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