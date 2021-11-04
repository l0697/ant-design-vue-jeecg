<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <!-- 主表单区域 -->
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="24" >
            <a-form-model-item label="创建人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createBy">
              <a-input v-model="model.createBy" placeholder="请输入创建人" disabled></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="创建日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createTime">
              <j-date placeholder="请选择创建日期" v-model="model.createTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="更新人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="updateBy">
              <a-input v-model="model.updateBy" placeholder="请输入更新人" disabled></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="更新日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="updateTime">
              <j-date placeholder="请选择更新日期" v-model="model.updateTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="所属部门" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="sysOrgCode">
              <a-input v-model="model.sysOrgCode" placeholder="请输入所属部门" disabled></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="atom_name" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="atomName">
              <a-input v-model="model.atomName" placeholder="请输入atom_name" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="polymorph" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="polymorph">
              <a-input-number v-model="model.polymorph" placeholder="请输入polymorph" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="weighted_surface_energy_epa" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weightedSurfaceEnergyEpa">
              <a-input-number v-model="model.weightedSurfaceEnergyEpa" placeholder="请输入weighted_surface_energy_epa" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="pretty_formula" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="prettyFormula">
              <a-input v-model="model.prettyFormula" placeholder="请输入pretty_formula" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="material_id" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialId">
              <a-input v-model="model.materialId" placeholder="请输入material_id" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="weighted_surface_energy" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weightedSurfaceEnergy">
              <a-input-number v-model="model.weightedSurfaceEnergy" placeholder="请输入weighted_surface_energy" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="spacegroup_number" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="spacegroupNumber">
              <a-input-number v-model="model.spacegroupNumber" placeholder="请输入spacegroup_number" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="eabove_hull" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="eaboveHull">
              <a-input-number v-model="model.eaboveHull" placeholder="请输入eabove_hull" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="surface_anisotropy" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="surfaceAnisotropy">
              <a-input-number v-model="model.surfaceAnisotropy" placeholder="请输入surface_anisotropy" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="shape_factor" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="shapeFactor">
              <a-input-number v-model="model.shapeFactor" placeholder="请输入shape_factor" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="weighted_work_function" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weightedWorkFunction">
              <a-input-number v-model="model.weightedWorkFunction" placeholder="请输入weighted_work_function" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="miller_units" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="millerUnits">
              <a-input v-model="model.millerUnits" placeholder="请输入miller_units" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="has_reconstructed" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="hasReconstructed">
              <j-switch v-model="model.hasReconstructed" :options="[1,0]" ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="file" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="file">
              <j-upload v-model="model.file"  ></j-upload>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
      <!-- 子表单区域 -->
    <a-tabs v-model="activeKey" @change="handleChangeTabs">
      <a-tab-pane tab="金属界面数据附表" :key="refKeys[0]" :forceRender="true">
        <j-editable-table
          :ref="refKeys[0]"
          :loading="surfaceSonTable.loading"
          :columns="surfaceSonTable.columns"
          :dataSource="surfaceSonTable.dataSource"
          :maxHeight="300"
          :disabled="formDisabled"
          :rowNumber="true"
          :rowSelection="true"
          :actionButton="true"/>
      </a-tab-pane>
    </a-tabs>
  </a-spin>
</template>

<script>

  import { getAction } from '@/api/manage'
  import { FormTypes,getRefPromise,VALIDATE_NO_PASSED } from '@/utils/JEditableTableUtil'
  import { JEditableTableModelMixin } from '@/mixins/JEditableTableModelMixin'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: 'SurfaceMainForm',
    mixins: [JEditableTableModelMixin],
    components: {
    },
    data() {
      return {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 6 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        labelCol2: {
          xs: { span: 24 },
          sm: { span: 3 },
        },
        wrapperCol2: {
          xs: { span: 24 },
          sm: { span: 20 },
        },
        model:{
        },
        // 新增时子表默认添加几行空数据
        addDefaultRowNum: 1,
        validatorRules: {
        },
        refKeys: ['surfaceSon', ],
        tableKeys:['surfaceSon', ],
        activeKey: 'surfaceSon',
        // 金属界面数据附表
        surfaceSonTable: {
          loading: false,
          dataSource: [],
          columns: [
            {
              title: '创建人',
              key: 'createBy',
              type: FormTypes.input,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '创建日期',
              key: 'createTime',
              type: FormTypes.datetime,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '更新人',
              key: 'updateBy',
              type: FormTypes.input,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '更新日期',
              key: 'updateTime',
              type: FormTypes.datetime,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '所属部门',
              key: 'sysOrgCode',
              type: FormTypes.input,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'surface_energy',
              key: 'surfaceEnergy',
              type: FormTypes.inputNumber,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'is_reconstructed',
              key: 'isReconstructed',
              type: FormTypes.checkbox,
              customValue: [1,0],
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'structure',
              key: 'structure',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'initial_structure',
              key: 'initialStructure',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'work_function',
              key: 'workFunction',
              type: FormTypes.inputNumber,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'locpot_along_c',
              key: 'locpotAlongC',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'efermi',
              key: 'efermi',
              type: FormTypes.inputNumber,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'area_fraction',
              key: 'areaFraction',
              type: FormTypes.inputNumber,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: 'has_wulff',
              key: 'hasWulff',
              type: FormTypes.checkbox,
              customValue: [1,0],
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
          ]
        },
        url: {
          add: "/surfaceTable/surfaceMain/add",
          edit: "/surfaceTable/surfaceMain/edit",
          queryById: "/surfaceTable/surfaceMain/queryById",
          surfaceSon: {
            list: '/surfaceTable/surfaceMain/querySurfaceSonByMainId'
          },
        }
      }
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
    },
    methods: {
      addBefore(){
        this.surfaceSonTable.dataSource=[]
      },
      getAllTable() {
        let values = this.tableKeys.map(key => getRefPromise(this, key))
        return Promise.all(values)
      },
      /** 调用完edit()方法之后会自动调用此方法 */
      editAfter() {
        this.$nextTick(() => {
        })
        // 加载子表数据
        if (this.model.id) {
          let params = { id: this.model.id }
          this.requestSubTableData(this.url.surfaceSon.list, params, this.surfaceSonTable)
        }
      },
      //校验所有一对一子表表单
      validateSubForm(allValues){
          return new Promise((resolve,reject)=>{
            Promise.all([
            ]).then(() => {
              resolve(allValues)
            }).catch(e => {
              if (e.error === VALIDATE_NO_PASSED) {
                // 如果有未通过表单验证的子表，就自动跳转到它所在的tab
                this.activeKey = e.index == null ? this.activeKey : this.refKeys[e.index]
              } else {
                console.error(e)
              }
            })
          })
      },
      /** 整理成formData */
      classifyIntoFormData(allValues) {
        let main = Object.assign(this.model, allValues.formValue)
        return {
          ...main, // 展开
          surfaceSonList: allValues.tablesValue[0].values,
        }
      },
      validateError(msg){
        this.$message.error(msg)
      },

    }
  }
</script>

<style scoped>
</style>