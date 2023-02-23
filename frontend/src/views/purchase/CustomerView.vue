<template>
  <Layout>
    <Title title="客户管理"></Title>
    <el-button type="primary" size="medium" @click="dialogVisible = true">新增客户</el-button>
    <div style="margin-top: 10px">
      <el-table
        :data="customerList"
        stripe
        style="width: 100%"
        :header-cell-style="{'text-align':'center'}"
        :cell-style="{'text-align':'center'}">
        <el-table-column
          prop="id"
          label="id"
          width="60">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="70">
        </el-table-column>
        <el-table-column
          prop="type"
          label="类型"
          width="100"
          :filters="[{ text: '供应商', value: '供应商' }, { text: '销售商', value: '销售商' }]"
          :filter-method="filterTag"
          filter-placement="bottom-end">
          <template slot-scope="scope">
            <el-tag
              :type="scope.row.type === '供应商' ? 'primary' : 'success'"
              disable-transitions>{{scope.row.type}}</el-tag>
          </template>
        </el-table-column>
        <el-table-column
          prop="level"
          label="级别"
          width="50">
        </el-table-column>
        <el-table-column
          prop="phone"
          label="电话"
          width="150">
        </el-table-column>
        <el-table-column
          prop="address"
          label="地址"
          width="150">
        </el-table-column>
        <el-table-column
          prop="zipcode"
          label="邮编"
          width="100">
        </el-table-column>
        <el-table-column
          prop="email"
          label="邮箱"
          width="200">
        </el-table-column>
        <el-table-column
          prop="lineOfCredit"
          label="应收额度(元)"
          width="120">
        </el-table-column>
        <el-table-column
          prop="receivable"
          label="应收(元)"
          width="120">
        </el-table-column>
        <el-table-column
          prop="payable"
          label="应付(元)"
          width="120">
        </el-table-column>
        <el-table-column
          prop="operator"
          label="操作员"
          width="120">
        </el-table-column>
        <el-table-column
          label="操作">
          <template slot-scope="scope">
            <el-button
              @click="showEditDialog(scope.row.id)"
              type="text"
              size="small">
              编辑
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <el-dialog title="新增客户"
    :visible.sync="dialogVisible"
    width="40%"
    :before-close="handleClose">
    <div style="width: 90%; margin: 0 auto">
      <el-form :model="customerForm" label-width="100px" ref="customerForm" :rules="rules">
        <el-form-item label="姓名: " prop="name">
            <el-input v-model="customerForm.name"></el-input>
        </el-form-item>
        <el-form-item label="类型: " prop="type">
            <el-select v-model="customerForm.type" placeholder="请选择类别">
              <el-option
                v-for="item in customerTypeList" :key="item.id" :value="item">
                {{ item }}
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="级别: " prop="level">
            <el-input v-model="customerForm.level"></el-input>
        </el-form-item>
        <el-form-item label="电话: " prop="phone">
            <el-input v-model="customerForm.phone"></el-input>
        </el-form-item>
        <el-form-item label="地址: " prop="address">
            <el-input v-model="customerForm.address"></el-input>
        </el-form-item>
        <el-form-item label="邮编: " prop="zipcode">
            <el-input v-model="customerForm.zipcode"></el-input>
        </el-form-item>
        <el-form-item label="邮箱: " prop="email">
            <el-input v-model="customerForm.email"></el-input>
        </el-form-item>
        <el-form-item label="应收额度: " prop="lineOfCredit">
            <el-input v-model="customerForm.lineOfCredit"></el-input>
        </el-form-item>
        <el-form-item label="应收: " prop="receivable">
            <el-input v-model="customerForm.receivable"></el-input>
        </el-form-item>
        <el-form-item label="应付: " prop="payable">
            <el-input v-model="customerForm.payable"></el-input>
        </el-form-item>
        <el-form-item label="操作员: " prop="operator">
            <el-input v-model="customerForm.operator"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm('customerForm')">确认新增</el-button>
      </span>
    </el-dialog>
    <el-dialog title="编辑客户信息"
    :visible.sync="editDialogVisible"
    width="40%"
    :before-close="handleClose">
    <div style="width: 90%; margin: 0 auto">
      <el-form :model="editForm" label-width="100px" ref="editForm" :rules="rules">
        <el-form-item label="姓名: " prop="name">
            <el-input v-model="editForm.name"></el-input>
        </el-form-item>
        <el-form-item label="类型: " prop="type">
            <el-select v-model="editForm.type" placeholder="请选择类别">
              <el-option
                v-for="item in customerTypeList" :key="item.id" :value="item">
                {{ item }}
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="级别: " prop="level">
            <el-input v-model="editForm.level"></el-input>
        </el-form-item>
        <el-form-item label="电话: " prop="phone">
            <el-input v-model="editForm.phone"></el-input>
        </el-form-item>
        <el-form-item label="地址: " prop="address">
            <el-input v-model="editForm.address"></el-input>
        </el-form-item>
        <el-form-item label="邮编: " prop="zipcode">
            <el-input v-model="editForm.zipcode"></el-input>
        </el-form-item>
        <el-form-item label="邮箱: " prop="email">
            <el-input v-model="editForm.email"></el-input>
        </el-form-item>
        <el-form-item label="应收额度: " prop="lineOfCredit">
            <el-input v-model="editForm.lineOfCredit"></el-input>
        </el-form-item>
        <el-form-item label="应收: " prop="receivable">
            <el-input v-model="editForm.receivable"></el-input>
        </el-form-item>
        <el-form-item label="应付: " prop="payable">
            <el-input v-model="editForm.payable"></el-input>
        </el-form-item>
        <el-form-item label="操作员: " prop="operator">
            <el-input v-model="editForm.operator"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="updateForm('editForm')">确认修改</el-button>
      </span>
      <span slot="footer" class="dialog-footer">
        <el-button type="danger" @click="deleteCustomer">删除</el-button>
      </span>
    </el-dialog>
  </Layout>
</template>

<script>
import Layout from "@/components/content/Layout";
import Title from "@/components/content/Title";
import { getAllCustomer, createCustomer, findCustomerById, updateCustomer, deleteCustomerById } from "../../network/purchase";
export default {
  name: 'CustomerView',
  components: {
    Layout,
    Title
  },
  data() {
    return {
      customerList: [],
      dialogVisible: false,
      customerTypeList: ['销售商', '供应商'],
      customerForm: {},
      editDialogVisible: false,
      editForm: {}
    }
  },
  async mounted() {
    await getAllCustomer({ params : { type: 'SUPPLIER' } }).then(_res => {
      this.customerList = this.customerList.concat(_res.result)
    })
    await getAllCustomer({ params : { type: 'SELLER' } }).then(_res => {
      this.customerList = this.customerList.concat(_res.result)
    })
  },
  methods: {
    filterTag(value, row) {
      return row.type === value
    },
    addCustomer() {
      // TODO: 新增客户
      alert('TODO: 新增客户')
    },
    editInfo(id) {
      // TODO: 修改客户信息
      alert(`TODO: 修改${id}客户信息`)
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          this.resetForm()
          done();
        })
        .catch(_ => {});
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.customerForm.id = null
          this.customerForm.level = parseInt(this.customerForm.level)
          this.customerForm.lineOfCredit = Number(this.customerForm.lineOfCredit)
          this.customerForm.receivable = Number(this.customerForm.receivable)
          this.customerForm.payable = Number(this.customerForm.payable)
          createCustomer(this.customerForm).then(_res => {
            if (_res.msg === 'Success') {
              this.$message.success('创建成功!')
              this.dialogVisible = false
              this.resetForm()
              this.customerList = []
              getAllCustomer({ params : { type: 'SUPPLIER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
              getAllCustomer({ params : { type: 'SELLER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
            }
          })
        }
      })
    },
    resetForm() {
      this.customerForm = {}
      this.editForm = {}
    },
    showEditDialog(id_){
      //TODO: Query backend and then show the customer info
      findCustomerById({params: { id: id_}}).then(_res => {
        if(_res.msg == 'Success'){
          let customer = _res.result
          this.editForm = customer
          this.editDialogVisible = true
        }else {
          console.log('Something wrong!')
          alert('Something wrong!')
        }
      })
    },
    updateForm(formName){
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.editForm.id = parseInt(this.editForm.id)
          this.editForm.level = parseInt(this.editForm.level)
          this.editForm.lineOfCredit = Number(this.editForm.lineOfCredit)
          this.editForm.receivable = Number(this.editForm.receivable)
          this.editForm.payable = Number(this.editForm.payable)

          updateCustomer(this.editForm).then(_res => {
            if (_res.msg === 'Success') {
              this.$message.success('修改成功!')
              this.editDialogVisible = false
              this.resetForm()
              this.customerList = []
              getAllCustomer({ params : { type: 'SUPPLIER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
              getAllCustomer({ params : { type: 'SELLER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
            }
          })
        }
      })
    },
    deleteCustomer(){
      deleteCustomerById({params: {id: this.editForm.id}}).then(_res => {
        if(_res.msg == 'Success'){
          this.$message.success('删除成功!')
              this.editDialogVisible = false
              this.resetForm()
              this.customerList = []
              getAllCustomer({ params : { type: 'SUPPLIER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
              getAllCustomer({ params : { type: 'SELLER' } }).then(_res => {
                this.customerList = this.customerList.concat(_res.result)
              })
        }
      })
    }
  }
}
</script>

<style>

</style>