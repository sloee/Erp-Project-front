<template>
  <a-drawer
    :title="title"
    :width="width"
    placement="right"
    :closable="false"
    @close="close"
    :visible="visible">
  
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <a-form-item label="证照类别编码" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['licenseTypeId']" placeholder="请输入证照类别编码"></a-input>
        </a-form-item>
        <a-form-item label="证照名字" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['licenseName']" placeholder="请输入证照名字"></a-input>
        </a-form-item>
        <a-form-item label="一级分类" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['firstClass']" placeholder="请输入一级分类"></a-input>
        </a-form-item>
        <a-form-item label="控制类别编码" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['controlClassCode']" placeholder="请输入控制类别编码"></a-input>
        </a-form-item>
        <a-form-item label="控制类别名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['controlClassName']" placeholder="请输入控制类别名称"></a-input>
        </a-form-item>
        <a-form-item label="提醒状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-dict-select-tag type="list" v-decorator="['remindStatus']" :trigger-change="true" dictCode="" placeholder="请选择提醒状态"/>
        </a-form-item>
        <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['notes']" placeholder="请输入备注"></a-input>
        </a-form-item>
        
      </a-form>
    </a-spin>
    <a-button type="primary" @click="handleOk">确定</a-button>
    <a-button type="primary" @click="handleCancel">取消</a-button>
  </a-drawer>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  import JDictSelectTag from "@/components/dict/JDictSelectTag"
  
  export default {
    name: "YwCustomLicenseMangeModal",
    components: { 
      JDictSelectTag,
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
        },
        url: {
          add: "/yw_customlicensemange/ywCustomLicenseMange/add",
          edit: "/yw_customlicensemange/ywCustomLicenseMange/edit",
        }
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'licenseTypeId','licenseName','firstClass','controlClassCode','controlClassName','remindStatus','notes','updateTime','updateBy'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'licenseTypeId','licenseName','firstClass','controlClassCode','controlClassName','remindStatus','notes','updateTime','updateBy'))
      }
      
    }
  }
</script>

<style lang="less" scoped>
/** Button按钮间距 */
  .ant-btn {
    margin-left: 30px;
    margin-bottom: 30px;
    float: right;
  }
</style>