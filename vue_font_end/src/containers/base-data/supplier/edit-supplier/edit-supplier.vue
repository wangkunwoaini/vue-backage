<template>
    <div class="edit-supplier">
      <BastTitle :title="title"></BastTitle>

      <Form ref="editData" :model="editData" :label-width="100" v-if="editData">
        <FormItem label="供货商名称" prop="supplierName">
          <Input v-model="editData.supplierName" placeholder="请输入供货商名称" />
        </FormItem>
        <FormItem label="供货商编码" prop="supplierCode">
          <Input v-model="editData.supplierCode" placeholder="请输入供货商编码"/>
        </FormItem>
        <FormItem label="供货商电话" prop="mobilePhone">
          <Input v-model="editData.mobilePhone" placeholder="请输入供货商电话"/>
        </FormItem>
        <FormItem label="座机" prop="telephone">
          <Input v-model="editData.telephone" placeholder="请输入座机"/>
        </FormItem>
        <FormItem label="微信" prop="wechat">
          <Input v-model="editData.wechat" placeholder="请输入微信"/>
        </FormItem>
        <FormItem label="税率" prop="taxRate">
          <Input v-model="editData.taxRate" placeholder="请输入税率"/>
        </FormItem>
        <FormItem label="地址" prop="addressId">
          <Input v-model="editData.addressId" placeholder="请输入地址"/>
        </FormItem>
        <FormItem label="备注" prop="mark">
          <Input v-model="editData.mark" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请填写备注"/>
        </FormItem>
        <FormItem>
          <Button type="primary" @click="submit">提交</Button>
        </FormItem>
      </Form>

    </div>
</template>

<script>
  import BastTitle from "@/components/base-title";
  import { Form,Select,Upload,Avatar,Button } from 'iview'
    export default {
      name: "edit-supplier",
      data(){
        return {
          title:this.$route.query.id?'编辑供货商':'新增供货商',
          editData:{
            supplierName:"",
            supplierCode:"",
            mobilePhone:"",
            telephone:"",
            taxRate:"",
            addressId:"",
            mark:"",
            wechat:""
          },
          api:"http://192.168.31.222:8080"

        }
      },
      components:{
        BastTitle
      },
      mounted(){
        if(this.$route.query.id){
          this.$http.get(`${this.api}/base/supplier/SupplierfindById`,{
            params:{ id:this.$route.query.id }
          }).then(response=>{
            let res = response.data;

            if(res){
              this.editData = res
            }
          });
        }


      },
      methods:{
        submit(){
          this.$http.post(`${this.api}/base/supplier/saveSupplier`,{...this.editData}).then(response=>{
            let res = response.data;
            console.log(res);
            if(res.result){
              this.$Message.info('修改成功');
              this.$router.push('/baseData/supplier')
            }
          });
          console.log(this.editData)
        }
      }
    }
</script>

<style scoped lang="stylus">
@import './edit-supplier.styl'
</style>
