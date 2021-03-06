<template>
  <el-dialog
      custom-class="common-dialog role-dialog"
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="36%"
      @open="handleOpen"
      @close="handleClose"
    >
      <el-form
        v-loading="loading" class="common-form"
        ref="form" :rules="rules" :model="formData"
        label-width="120px">
        <el-form-item label="角色名称" prop="name">
          <el-input v-model="formData.name"></el-input>
        </el-form-item>
        <!-- 状态 -->
        <el-form-item v-if="roleId!==-1" label="状态" prop="state">
          <el-select v-model="formData.state" class="w-100">
            <el-option 
              v-for="item in roleStates" :key="item.value" 
              :label="item.label" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item v-if="roleId === -1" prop="type" label="角色类型">
          <el-select v-model="formData.type">
            <el-option 
              v-for="item in roleTypes" 
              :key="item.value" 
              :value="item.value" 
              :label="item.label">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="角色菜单" prop="menus" :class="['tree-wrapper']">
          <el-input v-model="filterText" placeholder="输入菜单名称过滤">
            <i slot="prefix" class="el-input__icon el-icon-search"></i>
          </el-input>
          <div class="custom-tree">
            <el-tree
              show-checkbox
              node-key="id"
              ref="authTree"
              :data="menuList"
              :default-expand-all="true"
              :props="defaultProps"
              @check="changeCheck"
              :filter-node-method="filterNode"
            ></el-tree>
          </div>
        </el-form-item>
        <el-form-item label="备注" prop="remark">
          <el-input type="textarea" v-model="formData.remark"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" class="cancel" @click="dialogVisible = false">取消</el-button>
        <el-button v-if="roleId === -1" type="primary" class="purple-btn" @click="addRole">新增</el-button>
        <el-button v-else type="primary" class="purple-btn" @click="updateRole">保存</el-button>
      </span>
    </el-dialog>
</template>

<script>
import { roleTypes, roleStates } from '@/utils/dictionary'
import { getMenus, addRole, getRoleDetails, updateRole } from '@/api/role'
export default {
  name: "",
  components: {},
  props: {},
  directive: {},
  data() {
    return {
      roleId: -1, // 新增/编辑标识
      loading: false,
      dialogVisible: false,
      roleTypes,
      formData: {
        name: '',
        state: '',
        type: '',
        remark: '',
        menus: []
      },
      rules: {
        name: [
          {
            required: true,
            message: "请填写角色名称",
            trigger: "blur"
          }
        ],
        state: [
          {
            required: true,
            message: '请选择角色状态',
            trigger: "blur"
          }
        ],
        type: [
          {
            required: true,
            message: '请选择角色类型',
            trigger: 'blur'
          }
        ],
        menus: [
          {
            required: true,
            message: '请勾选角色权限',
            trigger: "blur"
          }
        ]
      },
      menuList: [],
      defaultProps: {
        children: 'child',
        label: 'name'
      },
      roleStates,
      filterText: ''
    };
  },
  computed: {
    dialogTitle() {
      return this.roleId === -1 ? '新增角色' : '编辑角色'
    }
  },
  watch: {
    filterText(val) {
      this.$refs.authTree.filter(val);
    }
  },
  created() {
    this.getMenus()
  },
  beforeMount() {},
  mounted() {},
  beforeDestroy() {},
  destroyed() {},
  methods: {
    async getMenus() {
      const res = await getMenus()
      this.menuList = res && res.data ? [res.data.data] : []
    },
    filterNode(value, data) {
      if (!value) return true;
      return data.name.indexOf(value) !== -1;
    },
    handleClose() {
      this.$refs.form.resetFields()
      this.$refs.authTree.setCheckedKeys([])
      this.dialogVisible = false;
    },
    handleOpen() {
      /* 编辑 */
      if (this.roleId !== -1) {
        this.loading = true
        getRoleDetails(this.roleId).then(res => {
          if (res.data.code ==0 || res.data.code == 200) {
            this.formData = _.cloneDeep(res.data.data)
            this.$refs.authTree.setCheckedKeys(this.formData.menus)
            setTimeout(() => {
              this.loading = false
            }, 150)
          } else {
            this.loading = false
          }
        }).catch(() => {
          this.loading = false
        })
      }
    },
    changeCheck(curData, checkedData) {
      if (!checkedData.checkedKeys.length) {
        this.$refs.form.validateField('menus')
      } else {
        this.$refs.form.clearValidate('menus')
      }
    },
    // 新增角色
    addRole() {
      const authTree = this.$refs.authTree
      this.formData.menus = [...authTree.getCheckedKeys(), ...authTree.getHalfCheckedKeys()]
      this.$refs.form.validate((valid) => {
        if (valid) {
          addRole(this.formData).then(res => {
            this.$emit('refresh')
            this.dialogVisible = false
            this.$message.success('新增角色成功')
          })
        }
      })
    },
    // 更新角色
    updateRole() {
      const { roleId } = this
      const reqData = _.cloneDeep(this.formData)
      const authTree = this.$refs.authTree
      reqData.menus = [...authTree.getCheckedKeys(), ...authTree.getHalfCheckedKeys()]
      updateRole(roleId, reqData).then(res => {
        this.$emit('refresh')
        this.dialogVisible = false
        this.$message.success('修改角色成功')
      })
    }
  }
};
</script>

<style lang="scss">
.role-dialog{
  .tree-wrapper{
    .el-form-item__content {
      .custom-tree{
        height: 220px !important;
        border: 1px solid #DCDFE6;
        border-radius: 4px;
        padding: 12px;
        overflow: auto;
        margin-top: 4px;
      }
    }
  }
}

@media screen and (max-width: 1366px) {
  .role-dialog{
    margin-top: 10vh !important;
  }
}
</style>
