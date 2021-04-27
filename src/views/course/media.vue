<template>
    <el-container>
      <!-- 头部 -->
      <el-header>
          <el-row  class="row-bg">
            <el-col :span="6">
              <el-button 
                class="grid-content" 
                icon="el-icon-edit" 
                type="primary"
                @click="mediacreact"
                >
                新增图文
              </el-button>
            </el-col>
            <el-col :span="2" :offset="11">
              <el-select v-model="value" placeholder="商品状态" class="">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                    >
                  </el-option>
                </el-select>
            </el-col>
            <el-col :span="3">
              <span class="grid-content">
                <el-input v-model="input" placeholder="标题"></el-input>
              </span>
            </el-col>
            <el-col :span="2">
              <el-button class="grid-content" icon="el-icon-search" type="primary">
                搜索
              </el-button>
            </el-col>
          </el-row>
      </el-header>
<!-- 主体 -->
      <el-main>
         <el-table
         :data="tableData"
          border
          style="width: 100%">
          <el-table-column
            align="center"
            label="ID"
            sortable
            width="80">
             <template slot-scope="scope">{{scope.row.id}}</template>
          </el-table-column>
          <el-table-column
            
            label="图文内容"
            width="370">
            <template style="height:70px;" slot-scope="scope">
              <div class="content_img">
                <img :src="scope.row.cover" alt="">
              </div>
              <div style="display:float;float:left;">
                <p style="margin:0;">{{scope.row.title}}</p>
                <p style="margin:0;">￥{{scope.row.price}}</p>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            label="订阅量"
            width="110">
            <template slot-scope="scope">{{scope.row.sub_count}}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="状态"
            width="100">
            <template slot-scope="scope">
              <el-tag :type="scope.row.status ? 'success' : 'danger'">
                {{scope.row.status | statusFilter}}
              </el-tag>
              <!-- {{scope.row.status= 1 ? <el-button size="mini">已上架</el-button>: <el-button size="mini">已下架</el-button>}} -->
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            label="创建时间"
            width="150">
            <template slot-scope="scope">{{scope.row.created_time}}</template>
          </el-table-column>
          <el-table-column
            align="center"
            label="操作"
            width="230">
            <template slot-scope="scope">
              <el-button size="mini">编辑</el-button>
              <el-button size="mini" @click="changestatus(scope.row,scope.status)" :type="scope.row.status ? 'success' : '' ">{{scope.row.status ? "上架" : "下架"}}</el-button>
              <el-button size="mini">删除</el-button>
            </template>
            
          </el-table-column>
        </el-table>
      </el-main>
      <!-- 底部 -->
      <el-footer>
        <div class="block">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :page-sizes="[10, 20, 30, 50]"
            :page-size="10"
            layout="total, sizes, prev, pager, next, jumper"
            :total="100">
          </el-pagination>
        </div>
      </el-footer>
    </el-container>
    
</template>
<script>
import {
  fetchList,
  creactMedia,
  updateMedia,
  deleteMedia
} from "@/api/media"
import MediaAdd from "./components/MediaAdd"

        
      
export default {
  components:{
      MediaAdd
  },
  
  data() {
    return {
      tableData:[],
      title:undefined,
      status:undefined,
      page:0,
      limit:20,
      sort:"+id",
      listLoading:false,
      input: '',
      options: [{
        value: '1',
        label: '已下架'
      }, {
        value: '2',
        label: '已上架'
      }],
      value: '',
    }
  },
  created(){
    this.getList();
  },
  methods: {
    // 获取数据
     getList(){
      this.listLoading = true;
      fetchList(this.status,this.title,this.page,this.limit,this.sort).then(response=>{
          // console.log(response)
        if(response.code === 20000){
          this.listLoading = false;
          this.tableData = response.data.items;
        }
          
          // console.log(this.tableData)
        
      })
    },
    // 上架下架
    changestatus(row,status){
      status = !status;
      this.$message({
        success:"操作success",
        type:"success"
      })
    },
    handleSizeChange(val) {
      // console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
    },
    mediacreact(){
      this.$router.push('/course/media/create')
    }
  },
  filters:{
    statusFilter(value){
      if(value == 1){
        return "已上架"
      }else{
        return "已下架"
      }
    }
  }
 


}
</script>

<style scoped>
 .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
 .row-bg {
    padding: 10px 0;

  }
  .content_img{
    float: left;
    
  }
  .content_img img{
    width: 100px;
    height: 50px;
  }
</style>

