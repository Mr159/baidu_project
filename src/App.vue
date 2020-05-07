<template>
  <div id="app" class="search-widget">
    <router-view/>
    <div class="search-box">
      <input type="text" id="searchText" v-model="title" @keydown.down="changedown" @keydown.up="changeup" @input="inputSearch" @keydown.enter="enterTap" placeholder="请输入内容">
      <input type="submit" value="搜索" @click="search()">
    </div>
    <ul v-show="list.length!=0">
      <li v-for="(item,index) in list" :key="index" :class="{active:index===nowIndex}"
      @mouseover="hover(index)" @click="skip(index)">
          {{item}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      title:"",
      list:[],
      nowIndex:-1,
    }
  },
  methods:{
    //获取搜索数据
    getData(word){
      this.$http.jsonp("https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su",{
        params:{
          wd:word,
        },
        jsonp:"cb"
      }).then((res)=>{
        console.log(res)
        this.list=res.body.s;
      })
    },
    //keydown.down事件
    changedown(){
      if(this.nowIndex<this.list.length-1){
        this.nowIndex++;
      }else{
        this.nowIndex=0
      }
      this.title=this.list[this.nowIndex]
    },
    //keydown.up事件
    changeup(){
      if(this.nowIndex>0){
        this.nowIndex--;
        
      }else{
        this.nowIndex=this.list.length-1
      }
      this.title=this.list[this.nowIndex]
    },
    //监听input
    inputSearch(){
      this.nowIndex=-1;
      if(this.title!= ""){
        this.getData(this.title)
      }else{
        this.list=''
      }
    },
    //enter事件
    enterTap(){
      window.open('https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd='+this.title)
      this.title = '';
			this.list = [];
    },
    //点击跳转事件
    skip(index){
      console.log(index)
      location.href='https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd='+this.list[index]
    },
    //鼠标移入移除事件
    hover(index){
      this.nowIndex=index
    },
    //点击搜索事件
    search(){
      window.open('https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd='+this.title)
      this.title = '';
			this.list = [];
    }
    
  },
  computed:{

  },
  watch:{

  }


}
</script>

<style>
.active{
  background: #eee;
}
.search-widget {
  font-size: 13px;
  width: 550px;
  margin: 10px 10px;
}
.search-widget * {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.search-widget .search-box {
  width: 100%;
  background: pink;
}
.search-widget .search-box:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
.search-widget .search-box input {
  outline: none;
  float: left;
  height: 35px;
}
.search-widget .search-box input[type="text"] {
  width: 80%;
  border: 1px solid #3a99ff;
  padding: 0 10px;
}
.search-widget .search-box input[type="submit"] {
  width: 20%;
  background: #3a99ff;
  color: white;
  border: none;
  cursor: pointer;
}
.search-widget ul {
  display: block;
  width: 80%;
  border: 1px solid #e6e6e6;
  border-top: none;
}
.search-widget ul li {
  display: block;
  padding: 0 10px;
  color: #666;
  line-height: 26px;
  cursor: pointer;
}
.search-widget p {
  color: #999;
  font-size: 15px;
  margin-top: 10px;
}

</style>
