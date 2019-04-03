<template>
  <div id="app" class='container'>  
    <h1>Todolist(VUE)</h1>
    <input  type="text" class='content' v-model="itemNew" v-on:keyup.enter="addNew"/>
    <ul class='task-list'>
    	<li class='list-item 'v-for="(item,index) in items" >
    	 <span class="item-content">{{item.content}}</span>
       <span class="fr">
       <span class="action" v-on:click='listDetail(item,index)'>详情</span>
       <span class="action" v-on:click='listDelete(index)'>删除</span>
       </span>
    	</li>
    </ul>
    <div class="task-detail" v-if='isShowDetail'>
      <input type="text" class="detail-content" v-model="showItem.content"/>
      <span style="font-size: 16px;color:white;"> 备注</span>
      <textarea class="desc" v-model="showItem.desc"></textarea>
      <span style="font-size: 16px;color:white;"> 设置闹钟</span>
      <input type="text" class="datetime" v-model="showItem.datetime"/>
      <button v-on:click="changeSubmit">提交</button>
    </div>
  </div>
</template>

<script>
//使用ES6特性引入 localstorage 储存脚本，命名为 Storage
import Storage from './localstorage'
//ES6语法，相当于 
//new Vue({})
export default {
  name: 'app',//name属性作为组件名称，全局 ID 自动作为组件的 name
  data () {
  	return {
  		
      isShowDetail: false,
      showItem: {},
      detailIndex: 0,
  		items:Storage.fetch(),//获取存在 storage 里面的键值对
  		itemNew:''
  	}
  },
  components:{
  	//在#app元素内，注册组件
  	//Hello
  },
  methods:{
  	addNew:function () {
  		this.items.push({
  			content : this.itemNew
  		});
  		//清空文本栏
  		this.itemNew = null;
  	},
    changeItemf:function() {
      this.items[this.detailIndex]={
          content: changeItem.desc ,
          desc: changeItem.desc
      }
    },
  
    listDetail: function(item,index){
    
       this.detailIndex=index;
       this.showItem.content=item.content;
       this.showItem.desc=item.desc;
       this.isShowDetail= true;
       console.log(this.showItem)

    },
    listDelete: function(index){
       this.items.splice(index,1)
       this.isShowDetail= false;
      
    },
    changeSubmit:function(){
    //将数据存入数组
      this.items[this.detailIndex].content = this.showItem.content;
      this.items[this.detailIndex].desc = this.showItem.desc;
      this.items[this.detailIndex].datepick = '2011'
      console.log(this.items)
    //关闭弹窗
      this.isShowDetail= false;
    }
  },
  watch:{
  	items:{
	  	 handler: function (items) {
	      Storage.save(items);//监控li变化，将新增的值存入 sessionStorage 里
	    },										//sessionStorage 里的数据将在页面关闭后删除
	  	deep:true							//深度监视，只要 items 有一点改变就会触发回调函数handler
  	}
  }
}
</script>

<style>
.isStudent {
	color: red;
}
* {
      margin: 0;
      padding: 0;
      -webkit-box-sizing: border-box;
      -moz-box-sizing: border-box;
      box-sizing: border-box;
      outline: 0;
    }
    body {
      background-color: #00334B;
      color: white;
    }
    h1 {
       width: 80%;
    
      padding: 3px;
      text-align: center;
    }
    .container {
      max-width: 700px;
      margin: 0 auto;
      
    }
  
   .content {
      width: 80%;
      border-radius: 4px;
      padding: 3px;

     
    }
  
    /*********************************************/
    .task-list {
      width: 100%;
      color: black;
      margin-top: 10px;


    }
    .task-list .list-item {
           background-color: white;
           padding: 10px;
           margin-bottom: 10px;
           border-radius: 10px;
    }
    .task-list .list-item:hover {
      background-color: #ccc;
    }
    .task-list .item-cotent {

    }
    .task-list .fr {
      float: right;
      font-size: 20px;

    }
    .fr .action {
            cursor: pointer;
            color: #888;
       }
    .fr .action:hover {
            color: black;
    }

   /*********************************************/
    .task-detail{
        width: 300px;
        background-color: #524141;
        border-radius: 6px;
        color: white;
        padding: 20px;
        position: fixed;
        right: calc(50vw - 150px);
        top: 100px;
    }

    .detail-content,.datetime,.desc{
      background-color: #ddd;
      width: 100%;      
      margin-bottom: 20px;
      padding: 3px;
    }
    .detail-content,.datetime {
      max-height: 100px;
    }
    .desc{
        height: 100px;
      
    }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
