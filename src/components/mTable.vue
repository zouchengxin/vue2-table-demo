<template>
  <div class="table">
    <!-- 过滤器 -->
    <form class="filter">
      <select v-model="filterColum">
        <option v-for="(col,index) in columns" :key="index" :value="col.key">{{col.name}}</option>
      </select>
      <input type="text" v-model="filterVal">
      <input type="submit" value="搜索" @click="submitFilter($event)">
    </form>

    <!-- 表格 -->
    <table border="1">
      <tr class="header">
        <th v-for="(col,index) in columns" :key="index">
          <!-- 排序图标 -->
          <span class="sort-icon" v-if="col.sortAble">
            <i class="sort-up" :class="{active: sortColumn==col.key&&sortOrder==1}" @click="sortChange(col.key,1)"></i>
            <i class="sort-down" :class="{active: sortColumn==col.key&&sortOrder==0}" @click="sortChange(col.key,0)"></i>
          </span>
          <!-- 表头名称 -->
          <span>{{col.name}}</span>
        </th>
      </tr>
      <tr class="body" v-for="(row,i1) in tableData" :key="i1">
        <td v-for="(col,i2) in columns" :key="i2">{{row[col.key]}}</td>
      </tr>
    </table>

    <!-- 分页 -->
    <m-pagination :page="page" @change="pageChange"></m-pagination>
  </div>
</template>

<script>
import MPagination from './mPagination.vue'
export default {
  name: 'mTable',
  components:{
    MPagination
  },
  props: {
    data:{//数据源
      type:Array,
      default:function(){
        return [
          {
            name:'小李',
            age:12,
            sex:0,
          },
          {
            name:'小红',
            age:15,
            sex:1,
          },
          {
            name:'小明',
            age:13,
            sex:0
          },
          {
            name:'小张',
            age:19,
            sex:1
          },
          {
            name:'小王',
            age:13,
            sex:0
          },
          {
            name:'小马',
            age:19,
            sex:1
          }
        ]
      }
    },
    columns:{//表头
      type:Array,
      default:function(){
        return [
          {
            key:'name',
            name:'姓名',
            sortAble:true,//启用排序
          },
          {
            key:'age',
            name:'年龄',
            sortAble:true,
          },
          {
            key:'sex',
            name:'性别'
          }
        ]
      }
    },
    pageSize:{//每页大小
      type:Number,
      default:2
    },
    pageNum:{//当前页码
      type:Number,
      default:1
    },

  },
  data(){
    return {
      page:{
        pageSize:this.pageSize,
        pageNum:this.pageNum,
        pageTotal:this.data.length
      },
      tableData:this.data,//显示的表格数据
      sortColumn:'',//排序字段
      sortOrder:-1,//排序方式
      filterColum:'',//过滤字段
      filterVal:'',//过滤值
    }
  },
  created(){
    this.pageChange(this.pageNum,this.pageSize);
  },
  methods:{
    pageChange(curr,size){//页码发生改变
      this.page.pageSize=size;
      this.page.pageNum=curr;
      this.filterTable();
      this.$emit('pageChange',curr,size);
    },

    sortChange(column,order){//点击排序(1:升序,0:降序)
      this.sortColumn=column;
      this.sortOrder=order;
      this.filterTable();
      this.tableData.sort((a,b) =>{
        if(a[column]>b[column]){
          return order==1?1:-1;
        }else if(a[column]<b[column]){
          return order==1?1:-1;
        }else{
          return 0
        }
      });
      this.$emit('sortChange',column,order);
    },

    submitFilter(event){//提交过滤
      event.preventDefault();
      this.page.pageNum=1;
      this.filterTable();
    },

    filterTable(){//过滤数据
      const start=(this.page.pageNum-1)*this.page.pageSize,end=start+this.page.pageSize;
      if(this.filterColum&&this.filterVal){
        this.tableData=this.data.filter((row) =>{
          return row[this.filterColum].toString().includes(this.filterVal);
        });
        this.page.pageTotal=this.tableData.length;
        this.tableData=this.tableData.slice(start,end);
      }else{
        this.page.pageTotal=this.data.length;
        this.tableData=this.data.slice(start,end);
      }
      
      
    }
  }
}
</script>

<style scoped>
.sort-up,.sort-down{
  width: 0px;
  height:0px;
  border: solid 4px transparent;
  cursor: pointer;
}
.sort-up{
  border-bottom-color: gray;
}
.sort-up.active{
  border-bottom-color: #1890ff;
}
.sort-down{
  border-top-color: gray;
}
.sort-down.active{
  border-top-color: #1890ff;
}
.sort-icon{
  display: inline-flex;
  flex-flow: column nowrap;
  height: 18px;
  justify-content: space-between;
  vertical-align: middle;
}
.filter{
  text-align: left;
}
</style>