<template>
  <div>
    <div id="screen" :class="state" @click="onClickScreen">
      {{message}}
    </div>
    <div v-show="result.length">
      <div>평균시간 : {{average}}ms</div>
      <button @click="onReset">리셋</button>
    </div>
  </div>  
</template>

<script>

let startTime = 0;
let endTime = 0;
let timeout = null;

export default {
  data(){
    return{
      result: [],
      state: 'waiting',
      message: '클릭해서 시작하세요.'
    }
  },
  computed: { //캐싱???
    /* 성능에 영향을 줌!!!!!!!!!!! */
    /* data가 가공되었을 때 */
    /* result에 변화가 있을 때 average 실행 */
    average(){
      return this.result.reduce((a,c) => a + c, 0)/this.result.length || 0;
    }

  },
  methods: {
    onReset(){
      this.result = [];
    },
    onClickScreen(){
      if(this.state === 'waiting'){
        this.state = 'ready';
        this.message = '초록색이 되면 클릭!!';
        timeout = setTimeout(()=>{
          this.state = 'now';
          this.message ='지금 클릭!';
          startTime = new Date();
        }, Math.floor(Math.random()*1000) + 2000);
      }else if(this.state === 'ready'){
        clearTimeout(timeout);
        this.state = 'waiting';
        this.message = '너무 성급하시군요! 초록색이 된 후 클릭하세요.';
      }else if(this.state === 'now'){
        endTime = new Date();
        this.state = 'waiting';
        this.message = '클릭해서 시작하세요.';
        this.result.push(endTime - startTime);
      }
    }
  }
}
</script>

<style>
  #screen{
    width: 300px;
    height: 200px;
    text-align: center;
    user-select: none;
  }
  #screen.waiting{
    background-color: aqua;
  }
  #screen.ready{
    background-color: red;
    color: white;
  }
  #screen.now{
    background-color: greenyellow;
  }
</style>
