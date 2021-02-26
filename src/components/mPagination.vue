<template>
  <ul class="pagination">
    <!-- 分页 -->
    <li class="total-text">总共{{page.pageTotal}}页</li>
    <li class="prev-btn" @click="clickPrev"><a> &lt; </a></li>
    <li class="page-num" @click="clickNum(item)" :class="{active: currentPage==item}" v-for="(item,index) in numbers" :key=index><a>{{item}}</a></li>
    <li class="next-btn" @click="clickNext"><a> &gt; </a></li>
  </ul>
</template>

<script>
export default {
  name: 'mPagination',
  props: {
    page:{
      type:Object,
      default:function(){
        return {
          pageSize:5,
          pageNum:1,
          pageTotal:10
        }
      }
    }

  },
  data(){
    return {
      currentPage:1,//当前页
      pageSize:this.page.pageSize,//每页条数
      numbers:[]
    }
  },
  watch:{
    page:{
      handler(n,o){
        this.init();
      },
      deep:true
    }
  },
  created(){
    this.init();
  },
  methods:{
    init(){//初始化
      const len=Math.ceil(this.page.pageTotal/this.page.pageSize);
      const arr=new Array(len>0?len:0);
      this.numbers=[...arr.keys()].map((v) =>++v);
      this.currentPage=this.page.pageNum;
    },

    clickPrev(){//点击上一页
      if(this.currentPage<=1) return;
      --this.currentPage;
      this.$emit('change',this.currentPage,this.pageSize);
    },

    clickNext(){//点击下一页
      if(this.currentPage>=this.numbers.length) return;
      ++this.currentPage;
      this.$emit('change',this.currentPage,this.pageSize);
    },

    clickNum(num){//点击数字
      this.currentPage=num;
      this.$emit('change',this.currentPage,this.pageSize);
    }
  }
}
</script>

<style scoped>
.pagination{
  list-style: none;
  text-align: left;
  padding-left: 5px;
}
.pagination li{
  display: inline-block;
  padding-right: 5px;
}
.page-num,.prev-btn,.next-btn{
  background-color: #fff;
  border: 1px solid #d9d9d9;
  outline: 0;
  cursor: pointer;
  padding: 4px;
  margin: 4px;
}
.page-num.active{
  border-color: #1890ff;
  color: #1890ff;
}
</style>