<style>    
    #app {  
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
    }
    .v-box{
        width: 600px;
        margin: 0 auto;
    }
    .v-box:after{
        content: '';
        display: block;
        clear: both;
    }
    .head{
        width: 100%;
        height: 50px;
        line-height: 50px;
        background: rgba(47,47,47,0.98);
    }
    .todo-in{
        float: right;    
        width: 40%;
    }
    .todo-in input{
        width: 100%;
        height: 30px;
        margin-top: 8px;
        text-indent: 10px;
        border-radius: 5px;
        box-shadow: 0 1px 0 rgba(255,255,255,0.24), 0 1px 6px rgba(0,0,0,0.45) inset;
        border: none;
        outline: none;
    }
    h1{
        float: left;
        color: #DDD;
        font-size: 24px;
        cursor: pointer;
        font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
    }  
    ol{
        list-style: none;
    }
    .finished{
/*        color: red;*/
        opacity: 0.5;
        text-decoration: line-through;
    }
    .list{
        display: block;
        margin:12px 0;
        padding:10px 20px;
        background: #fff;
        border-radius: 3px;
        overflow: hidden;
        cursor: pointer;
        position: relative;
    }
    .list:before{
        content: '';
        display: block;
        width: 5px;
        height: 100%;
        background: #629A9C;
        position: absolute;
        top: 0;
        left: 0;
    }
    .empty{
        text-align: center;
        padding: 100px 50px;
        
    }
    .hide{
        display: none;
    }
</style>
 <template>
  <div id="app">
     <div class="head">
        <div class="v-box">
             <h1 v-html='title'></h1>   
             <div class='todo-in'>
                 <input type="text" v-model='newItem' @keyup.enter = 'addNew' placeholder="添加Todo">
             </div>    
        </div>         
     </div>
     <div class="v-box">        
        <ol>
             <li v-for='(item,i) in items' v-bind:class="[liclass,{finished:item.isFinished}]" v-on:click='doThings(item)'>
                <span>
                    {{i+1}} {{item.text}} 
                </span>                
             </li>
         </ol>
         <div class="empty" v-if="!items">
            暂无数据 ... 
         </div>
         <a href="javascript:" v-if="items" @click='clearData'>
             清空数据
         </a>     
     </div> 
     <div class="v-box hide">
         <csb msgfromfather='youdie' v-on:child-tell-me="listenMySon"></csb> 
             绑定自定义的事件：child-tell-me
         <p>我的儿子说：{{ childWords }}</p>
     </div>   
       
  </div>
</template>

<script>
    import Store from './store';
    import Csb from './components/csb';
    
    export default {
        watch:{
            items:{
                handler:function(items){
                    Store.save(items);   
                },
                deep:true
            }    
        },
        components:{Csb},
        data(){
            return {
                title: '<span>ToDoList<span>',
                items:Store.fetch(),
                liclass:'list',
                newItem:'',
                childWords:''
            }
        },
        methods:{  
            clearData:function(){
                Store.clear();
                this.items = '';
            },
            listenMySon:function(words){  
                this.childWords = words;
            },
            doThings:function(i){
                i.isFinished = !i.isFinished;
            },
            addNew:function(){
                var newItem ={};
                if(this.newItem.trim() == ''){
                    alert('请输入要做的事');
                    return;
                }
                newItem.text = this.newItem;
                newItem.isFinished = false;
                
                if(this.items == null || this.items == ''){
                    this.items = []; 
                }
                this.items.push(newItem);  
                this.newItem = '';
            }
        }
    }
</script>

