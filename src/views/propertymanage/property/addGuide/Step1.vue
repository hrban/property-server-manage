<template>
  <div>
    <a-form-model ref="ruleForm" :model="form" :rules="rules" style="margin: 20px auto 0;">
      <a-row>
        <a-col :span="12">
          <a-form-model-item
            label="所属公司"
            :labelCol="labelCol"
            prop="affiliates"
            :wrapperCol="wrapperCol"
          >
            <a-select v-model="form.affiliates">
              <a-select-option
                :value="index"
                v-for="(item, index) in select"
                :key="index"
              >{{ item }}</a-select-option>
            </a-select>
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="12">
          <a-form-model-item
            label="住宅编码"
            :labelCol="labelCol"
            :wrapperCol="wrapperCol"
            prop="code"
            ref="code"
          >
            <a-input v-model="form.code" @blur="() => {$refs.code.onFieldBlur()}" />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item
            label="住宅名称"
            prop="housename"
            :labelCol="labelCol"
            :wrapperCol="wrapperCol"
          >
            <a-input v-model="form.housename" />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="12">
          <a-form-model-item label="占地面积(平房米)" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item label="建筑面积(平房米)" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="12">
          <a-form-model-item label="绿地面积(平房米)" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item label="道路面积(平房米)" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="12">
          <a-form-model-item
            label="楼宇数量"
            :labelCol="labelCol"
            prop="housecount"
            :wrapperCol="wrapperCol"
          >
            <a-input v-model.number="form.housecount" />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item label="负责人" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-form-model-item label="住宅地址" :labelCol="{span: 3}" :wrapperCol="{span: 20}">
          <a-input />
        </a-form-model-item>
      </a-row>
      <a-divider>开发商信息</a-divider>
      <a-row>
        <a-col :span="12">
          <a-form-model-item label="公司名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item label="法人代表" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :span="12">
          <a-form-model-item label="联系人" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
        <a-col :span="12">
          <a-form-model-item label="联系电话" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-form-model-item label="备注" :labelCol="{span: 3}" :wrapperCol="{span: 20}">
          <a-input />
        </a-form-model-item>
      </a-row>
      <a-form-model-item :wrapperCol="{span: 19, offset: 5}">
        <a-button type="primary" @click="nextStep(123)">下一步</a-button>
        <a-button style="margin-left: 10px;" @click="resetForm">Reset</a-button>
      </a-form-model-item>
    </a-form-model>
  </div>
</template>

<script>
// import { oneStep } from '@/api/login'
import { selectCompany } from '@/api/estate'

export default {
    name: 'Step1',
    data() {
        return {
            select: [],
            labelCol: { lg: { span: 6 }, sm: { span: 4 } },
            wrapperCol: { lg: { span: 16 }, sm: { span: 20 } },
            form: {
                code: '',
                affiliates: [],
                housename: '',
                housecount: ''
            },
            rules: {
                code: [
                    { required: true, message: '住宅编码必须填写', trigger: 'blur' }
                    // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' }
                ],
                affiliates: [{ required: true, message: '所属公司必须填写', trigger: 'change' }],
                housename: [{ required: true, message: '楼宇名称必须填写', trigger: 'blur' }],
                housecount: [
                    {
                        required: true,
                        message: '楼宇数量必须填写',
                        trigger: 'change'
                    },
                    { min: 1, max: 20, type: 'number', message: 'Length should be 3 to 5', trigger: 'change' }
                ]
            }
        }
    },
    created() {
        selectCompany()
            .then(res => {
                this.select = res.result
            })
            // eslint-disable-next-line handle-callback-err
            .catch(err => {
                this.$notification['error']({
                    message: '错误',
                    description: err.toString(),
                    duration: 1
                })
            })
            .finally()
    },
    methods: {
        nextStep() {
            this.$emit('nextStep')
            this.$store.commit('SET_TITLE', { title: '哈哈' })
            // this.$refs.ruleForm.validate(valid => {
            //     if (valid) {
            //         alert('submit!')
            //         const random = Math.random()
            //         if (random > 0.5) {
            //              this.$emit('nextStep')
            //         } else {
            //             this.$notification.error({
            //                 message: 'error',
            //                 description: '123',
            //                 duration: 1
            //             })
            //         }
            //     } else {
            //         console.log('error submit!!')
            //         return false
            //     }
            // })
        },
        resetForm() {
            this.$refs.ruleForm.resetFields()
            console.log(this.$refs.ruleForm.resetFields)
        }
    }
}
</script>

<style lang="less" scoped>
.step-form-style-desc {
    padding: 0 56px;
    color: rgba(0, 0, 0, 0.45);

    h3 {
        margin: 0 0 12px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        line-height: 32px;
    }

    h4 {
        margin: 0 0 4px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 14px;
        line-height: 22px;
    }

    p {
        margin-top: 0;
        margin-bottom: 12px;
        line-height: 22px;
    }
}
.ant-form-item {
    margin: 4px 0;
}
</style>
