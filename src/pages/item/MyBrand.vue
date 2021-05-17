<template>
  <div>
    <v-layout class="px-3 pb-3">
      <v-flex xs2>
        <v-btn color="info">新增品牌</v-btn>
      </v-flex>
      <v-spacer></v-spacer>
      <v-flex xs6 sm6 md3>
        <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
      </v-flex>
    </v-layout>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <!--图片比较特殊，遍历地址而非图片-->
        <td class="text-xs-center"><img :src="props.item.image" ></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-btn flat icon color="info">
            <v-icon>edit</v-icon>
          </v-btn>
          <v-btn flat icon color="purple">
            <v-icon>delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  // axois
  //import axois from 'axios'
    export default {
        name: "MyBrand",
        data() {
          return{
            headers: [
              { text: '品牌id', align: 'center', sortable: true, value: 'id'},
              { text: '品牌名称', align: 'center', sortable: false, value: 'name'},
              { text: '品牌logo', align: 'center', sortable: false, value: 'image'},
              { text: '品牌首字母', align: 'center', sortable: true, value: 'letter'},
              { text: '操作', align: 'center', sortable: false, value: 'execute'}
            ],
            //这里加载数据，页面一加载，后台就要给数据
            brands: [],
            // v-model 搜索默认空串
            key: "",
            pagination: {},
            totalBrands: 0,
            //开启进度条，默认不显示，等数据加载完成改变
            loading: false
          }
        },
      created() {
          //Vue http.js
          /*this.$http.get("/brand/page",{
            params:{
              //分页查询
              page: 1,
            }
          })*/
          //ajax 这里是相对路径 axois 配置baseURL:
          /*axois.get("/brand/page",{
          }).then();*/
          //假数据
          /*this.brands = [
            {id: 2032, name: "欧博（OPPO）", image: "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg", letter: "O"},
            {id: 2033, name: "飞利浦（PHILIPS）", image: "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg", letter: "F"},
            {id: 2034, name: "华为（HUAWEI）", image: "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg", letter: "H"},
            {id: 2036, name: "酷派（Coolpad）", image: "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg", letter: "K"},
            {id: 2037, name: "魅族（MEIZU）", image: "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg", letter: "M"}*!/
          ];*/
          this.totalBrands = 15;
          //后台查询数据，定义一个方法
          this.loadBrands();
      },
      //因为搜索框值一变化就再次发送请求
      watch:{
          //监视搜索字段
          key(){
            //this.loadBrands();
            //查询，查不查到都将当前页从第一页显示,强制为第一页，那么深度监控loadingbrands就加载了
            this.pagination.page =1;
          },
        //深度监控
          pagination:{
            deep: true,
            handler(){
              this.loadBrands();
            }

          }
      },
      methods:{
          loadBrands(){
            this.loading = true;
            //发送ajax
            this.$http.get("/item/brand/page",{
              params:{
                key: this.key, //搜索条件
                //当前页
                page: this.pagination.page,
                //每页条数
                rows: this.pagination.rowsPerPage,
                //排序字段
                sortBy: this.pagination.sortBy,
                //降序
                desc: this.pagination.descending
              }
            }).then(resp => {
              //通关console看到data:{}
              /*console.log(resp);*/ 这是我们从后台发给前端的数据
              this.brands = resp.data.items;
              this.totalBrands = resp.data.total;
              //查询成功后 loading
              this.loading = false;

            })
          }
      }
    }
</script>

<style scoped>

</style>
