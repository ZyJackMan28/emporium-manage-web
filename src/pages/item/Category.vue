<template>
  <!--vuetify组件给我v-card标签-->
  <v-card>
      <v-flex xs12 sm10>
        <!--引用子组件v-tree
        父组件这里只是引用了子组件，所有的html都在子组件里-->
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  export default {
    name: "category",
    data() {
      return {
        isEdit:true
      }
    },
    methods: {
      /*子向父通信需要子调用父的方法,*/
      handleAdd(node) {
        this.$http({
          method:'post',
          url:'/item/category',
          data:this.$qs.stringify(node)
        }

      ).then(
          ()=>{
            this.$message.info("新增成功");
          }
        ).catch(()=>{
          this.$message.info("新增失败!")
        })

      },
      /**/
      handleEdit(id, name) {
        //封装请求参数
        const node ={
          id: id,
          name: name
        }
        this.$http({
            method: 'put',
            url:'/item/category',
            data:this.$qs.stringify(node)
          }

        ).then(()=>{

            this.$message.info("已经修改")
        }

        ).catch(()=>{
          this.$message.info("修改失败!")
        })
      },
      handleDelete(id) {
        /*
        * 删除，需要维护中间表， 如果删除的是父节点，需要将子节点一同删除，如果是只有一个节点，只要删除自己即可
        * 如果有父节点，需要删除子节点后更改父节点parentstatus false
        * */
        this.$http.delete('/item/category/cid/' + id
        ).then(()=>{
          this.$message.info("删除成功!");
        }).catch(()=>{
          this.$message.info("删除失败!")
        })
      },
      handleClick(node) {
        console.log(node)
      }
    }
  };
</script>

<style scoped>

</style>
