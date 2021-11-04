<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="创建人">
              <a-input placeholder="请输入创建人" v-model="queryParam.createBy"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="创建日期">
              <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择创建日期"
                      v-model="queryParam.createTime"></j-date>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="更新人">
                <a-input placeholder="请输入更新人" v-model="queryParam.updateBy"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="更新日期">
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择更新日期"
                        v-model="queryParam.updateTime"></j-date>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="所属部门">
                <a-input placeholder="请输入所属部门" v-model="queryParam.sysOrgCode"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="atom_name">
                <a-input placeholder="请输入atom_name" v-model="queryParam.atomName"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="polymorph">
                <a-input placeholder="请输入polymorph" v-model="queryParam.polymorph"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="weighted_surface_energy_epa">
                <a-input placeholder="请输入weighted_surface_energy_epa"
                         v-model="queryParam.weightedSurfaceEnergyEpa"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="pretty_formula">
                <a-input placeholder="请输入pretty_formula" v-model="queryParam.prettyFormula"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="material_id">
                <a-input placeholder="请输入material_id" v-model="queryParam.materialId"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="weighted_surface_energy">
                <a-input placeholder="请输入weighted_surface_energy" v-model="queryParam.weightedSurfaceEnergy"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="spacegroup_number">
                <a-input placeholder="请输入spacegroup_number" v-model="queryParam.spacegroupNumber"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="eabove_hull">
                <a-input placeholder="请输入eabove_hull" v-model="queryParam.eaboveHull"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="surface_anisotropy">
                <a-input placeholder="请输入surface_anisotropy" v-model="queryParam.surfaceAnisotropy"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="shape_factor">
                <a-input placeholder="请输入shape_factor" v-model="queryParam.shapeFactor"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="weighted_work_function">
                <a-input placeholder="请输入weighted_work_function" v-model="queryParam.weightedWorkFunction"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="miller_units">
                <a-input placeholder="请输入miller_units" v-model="queryParam.millerUnits"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="has_reconstructed">
                <a-input placeholder="请输入has_reconstructed" v-model="queryParam.hasReconstructed"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="file">
                <a-input placeholder="请输入file" v-model="queryParam.file"></a-input>
              </a-form-item>
            </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus" v-has="'surfaceTable:button:add'">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('金属界面数据主表')"
                v-has="'surfaceTable:button:export'">导出
      </a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl"
                @change="handleImportExcel">
        <a-button type="primary" icon="import" v-has="'surfaceTable:button:import'">excel导入</a-button>
      </a-upload>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importJsonUrl"
                @change="handleImportExcel">
        <a-button type="primary" icon="import" v-has="'surfaceTable:button:import'">json导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"
                     v-has="'surfaceTable:button:superSearch'"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay" v-has="'surfaceTable:button:deleteBatch'">
          <a-menu-item key="1" @click="batchDel">
            <a-icon type="delete"/>
            删除
          </a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作
          <a-icon type="down"/>
        </a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a
        style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
        <span data-v-22c025ae="" style="float: right;">
          <a-popover title="自定义显示列" trigger="click" placement="leftBottom">
          <template slot="content">
            <a-checkbox-group @change="onColSettingsChange" v-model="settingColumns" :defaultValue="settingColumns">
              <a-row>
                <template v-for="(item) in defColumns">
                  <template v-if="item.key!='rowIndex'&& item.dataIndex!='action'">
                    <a-col :span="12"><a-checkbox :value="item.dataIndex">{{ item.title }}</a-checkbox></a-col>
                  </template>
                </template>
              </a-row>
            </a-checkbox-group>
          </template>
          <a><a-icon type="setting"/></a>
        </a-popover>
        </span>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        class="j-table-force-nowrap"
        :scroll="{x:true}"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt=""
               style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)" v-has="'surfaceTable:button:edit'">编辑</a>

          <a-divider type="vertical" v-has="'surfaceTable:button:edit'"/>
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down"/></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item v-has="'exTable:button:delete'">
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <surface-main-modal ref="modalForm" @ok="modalFormOk"/>
  </a-card>
</template>

<script>

import {JeecgListMixin} from '@/mixins/JeecgListMixin'
import SurfaceMainModal from './modules/SurfaceMainModal'
import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
import '@/assets/less/TableExpand.less'

export default {
  name: "SurfaceMainList",
  mixins: [JeecgListMixin],
  components: {
    SurfaceMainModal
  },
  data() {
    return {
      description: '金属界面数据主表管理页面',
      // 表头
      columns: [],
      settingColumns: [],
      defColumns: [
        {
          title: '#',
          dataIndex: '',
          key: 'rowIndex',
          width: 60,
          align: "center",
          customRender: function (t, r, index) {
            return parseInt(index) + 1;
          }
        },
        {
          title: '创建人',
          align: "center",
          sorter: true,
          dataIndex: 'create_by'
        },
        {
          title: '创建日期',
          align: "center",
          sorter: true,
          dataIndex: 'create_time'
        },
        {
          title: '更新人',
          align: "center",
          sorter: true,
          dataIndex: 'update_by'
        },
        {
          title: '更新日期',
          align: "center",
          sorter: true,
          dataIndex: 'update_time'
        },
        {
          title: '所属部门',
          align: "center",
          sorter: true,
          dataIndex: 'sys_org_code'
        },
        {
          title: 'atom_name',
          align: "center",
          sorter: true,
          dataIndex: 'atom_name'
        },
        {
          title: 'polymorph',
          align: "center",
          sorter: true,
          dataIndex: 'polymorph'
        },
        {
          title: 'weighted_surface_energy_epa',
          align: "center",
          sorter: true,
          dataIndex: 'weighted_surface_energy_epa'
        },
        {
          title: 'pretty_formula',
          align: "center",
          sorter: true,
          dataIndex: 'pretty_formula'
        },
        {
          title: 'material_id',
          align: "center",
          sorter: true,
          dataIndex: 'material_id'
        },
        {
          title: 'weighted_surface_energy',
          align: "center",
          sorter: true,
          dataIndex: 'weighted_surface_energy'
        },
        {
          title: 'spacegroup_number',
          align: "center",
          sorter: true,
          dataIndex: 'spacegroup_number'
        },
        {
          title: 'eabove_hull',
          align: "center",
          sorter: true,
          dataIndex: 'eabove_hull'
        },
        {
          title: 'surface_anisotropy',
          align: "center",
          sorter: true,
          dataIndex: 'surface_anisotropy'
        },
        {
          title: 'shape_factor',
          align: "center",
          sorter: true,
          dataIndex: 'shape_factor'
        },
        {
          title: 'weighted_work_function',
          align: "center",
          sorter: true,
          dataIndex: 'weighted_work_function'
        },
        {
          title: 'miller_units',
          align: "center",
          sorter: true,
          dataIndex: 'miller_units'
        },
        {
          title: 'has_reconstructed',
          align: "center",
          sorter: true,
          dataIndex: 'has_reconstructed',
          customRender: (text) => (!text ? "" : (text == [1, 0][0] ? "是" : "否"))
        },
        {
          title: 'file',
          align: "center",
          sorter: true,
          dataIndex: 'file',
          scopedSlots: {customRender: 'fileSlot'}
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: "center",
          fixed: "right",
          width: 147,
          scopedSlots: {customRender: 'action'},
        }
      ],
      url: {
        list: "/surfaceTable/surfaceMain/list",
        delete: "/surfaceTable/surfaceMain/delete",
        deleteBatch: "/surfaceTable/surfaceMain/deleteBatch",
        exportXlsUrl: "/surfaceTable/surfaceMain/exportXls",
        importExcelUrl: "surfaceTable/surfaceMain/importExcel",
        importJsonUrl: "surfaceTable/surfaceMain/importJson",
      },
      dictOptions: {},
      superFieldList: [],
    }
  },
  created() {
    this.getSuperFieldList();
    this.initColumns();
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    },
    importJsonUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importJsonUrl}`;
    }
  },
  methods: {
    initDictConfig() {
    },
    getSuperFieldList() {
      let fieldList = [];
      fieldList.push({type: 'string', value: 'create_by', text: '创建人', dictCode: ''})
      fieldList.push({type: 'datetime', value: 'create_time', text: '创建日期'})
      fieldList.push({type: 'string', value: 'update_by', text: '更新人', dictCode: ''})
      fieldList.push({type: 'datetime', value: 'update_time', text: '更新日期'})
      fieldList.push({type: 'string', value: 'sys_org_code', text: '所属部门', dictCode: ''})
      fieldList.push({type: 'string', value: 'atom_name', text: 'atom_name', dictCode: ''})
      fieldList.push({type: 'int', value: 'polymorph', text: 'polymorph', dictCode: ''})
      fieldList.push({
        type: 'number',
        value: 'weighted_surface_energy_epa',
        text: 'weighted_surface_energy_epa',
        dictCode: ''
      })
      fieldList.push({type: 'string', value: 'pretty_formula', text: 'pretty_formula', dictCode: ''})
      fieldList.push({type: 'string', value: 'material_id', text: 'material_id', dictCode: ''})
      fieldList.push({type: 'number', value: 'weighted_surface_energy', text: 'weighted_surface_energy', dictCode: ''})
      fieldList.push({type: 'int', value: 'spacegroup_number', text: 'spacegroup_number', dictCode: ''})
      fieldList.push({type: 'number', value: 'eabove_hull', text: 'eabove_hull', dictCode: ''})
      fieldList.push({type: 'number', value: 'surface_anisotropy', text: 'surface_anisotropy', dictCode: ''})
      fieldList.push({type: 'number', value: 'shape_factor', text: 'shape_factor', dictCode: ''})
      fieldList.push({type: 'number', value: 'weighted_work_function', text: 'weighted_work_function', dictCode: ''})
      fieldList.push({type: 'string', value: 'miller_units', text: 'miller_units', dictCode: ''})
      fieldList.push({type: 'int', value: 'has_reconstructed', text: 'has_reconstructed', dictCode: 'is_true'})
      fieldList.push({type: 'Text', value: 'file', text: 'file', dictCode: ''})


      fieldList.push({type: 'string', value: 'surface_son,create_by', text: '金属界面数据附表-创建人', dictCode: ''})
      fieldList.push({type: 'datetime', value: 'surface_son,create_time', text: '金属界面数据附表-创建日期'})
      fieldList.push({type: 'string', value: 'surface_son,update_by', text: '金属界面数据附表-更新人', dictCode: ''})
      fieldList.push({type: 'datetime', value: 'surface_son,update_time', text: '金属界面数据附表-更新日期'})
      fieldList.push({type: 'string', value: 'surface_son,sys_org_code', text: '金属界面数据附表-所属部门', dictCode: ''})

      fieldList.push({
        type: 'number',
        value: 'surface_son,surface_energy',
        text: '金属界面数据附表-surface_energy',
        dictCode: ''
      })
      fieldList.push({
        type: 'int',
        value: 'surface_son,is_reconstructed',
        text: '金属界面数据附表-is_reconstructed',
        dictCode: ''
      })
      fieldList.push({type: 'string', value: 'surface_son,structure', text: '金属界面数据附表-structure', dictCode: ''})
      fieldList.push({
        type: 'string',
        value: 'surface_son,initial_structure',
        text: '金属界面数据附表-initial_structure',
        dictCode: ''
      })
      fieldList.push({type: 'number', value: 'surface_son,work_function', text: '金属界面数据附表-work_function', dictCode: ''})
      fieldList.push({
        type: 'string',
        value: 'surface_son,locpot_along_c',
        text: '金属界面数据附表-locpot_along_c',
        dictCode: ''
      })
      fieldList.push({type: 'number', value: 'surface_son,efermi', text: '金属界面数据附表-efermi', dictCode: ''})
      fieldList.push({type: 'number', value: 'surface_son,area_fraction', text: '金属界面数据附表-area_fraction', dictCode: ''})
      fieldList.push({type: 'int', value: 'surface_son,has_wulff', text: '金属界面数据附表-has_wulff', dictCode: 'is_true'})

      this.superFieldList = fieldList
    },
    initColumns() {
      //权限过滤（列权限控制时打开，修改第二个参数为授权码前缀）
      //this.defColumns = myColAuthFilter(this.defColumns,'osp:form:V-');
      //var key = this.$route.name+":colsettings";
      //Vue.ls.set(key, checkedValues, 7 * 24 * 60 * 60 * 1000)
      this.settingColumns = this.defColumns.slice(0, 7).map(item => {
        return item.dataIndex;
      });
      const cols = this.defColumns.filter(item => {
        if (item.key == 'rowIndex' || item.dataIndex == 'action') {
          return true
        }
        if (this.settingColumns.includes(item.dataIndex)) {
          return true
        }
        return false
      })
      this.columns = cols;

    },
    onColSettingsChange(checkedValues) {
      // var key = this.$route.name+":colsettings";
      //Vue.ls.set(key, checkedValues, 7 * 24 * 60 * 60 * 1000)
      this.settingColumns = checkedValues;
      const cols = this.defColumns.filter(item => {
        if (item.key == 'rowIndex' || item.dataIndex == 'action') {
          return true
        }
        if (this.settingColumns.includes(item.dataIndex)) {
          return true
        }
        return false
      })
      this.columns = cols;
    },
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>