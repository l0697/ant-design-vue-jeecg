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
              <a-form-item label="附件">
                <a-input placeholder="请输入附件" v-model="queryParam.file"></a-input>
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
      <a-button @click="handleAdd" type="primary" icon="plus" v-has="'exTable:button:add'">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('实验数据主表')" v-has="'exTable:button:export'">导出
      </a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl"
                @change="handleImportExcel">
        <a-button type="primary" icon="import" v-has="'exTable:button:import'">导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"
                     v-has="'exTable:button:superSearch'"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay" v-has="'exTable:button:deleteBatch'">
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
          <a @click="handleEdit(record)" v-has="'exTable:button:edit'">编辑</a>

          <a-divider type="vertical" v-has="'exTable:button:edit'"/>
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

    <experiment-main-modal ref="modalForm" @ok="modalFormOk"/>
  </a-card>
</template>

<script>

import {JeecgListMixin} from '@/mixins/JeecgListMixin'
import ExperimentMainModal from './modules/ExperimentMainModal'
import '@/assets/less/TableExpand.less'

export default {
  name: "ExperimentMainList",
  mixins: [JeecgListMixin],
  components: {
    ExperimentMainModal
  },
  data() {
    return {
      description: '实验数据主表管理页面',
      // 表头
      columns: [],
      settingColumns: [],
      //全部字段
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
          title: '附件',
          align: "center",
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
        list: "/exTable/experimentMain/list",
        delete: "/exTable/experimentMain/delete",
        deleteBatch: "/exTable/experimentMain/deleteBatch",
        exportXlsUrl: "/exTable/experimentMain/exportXls",
        importExcelUrl: "exTable/experimentMain/importExcel",

      },
      dictOptions: {},
      superFieldList: [],
    }
  },
  created() {
    this.initColumns();
    this.getSuperFieldList();
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
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
      fieldList.push({type: 'Text', value: 'file', text: '附件', dictCode: ''})


      fieldList.push({type: 'string', value: 'org_struc_param,create_by', text: '组织结构参数-创建人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'org_struc_param,create_time', text: '组织结构参数-创建日期',dictCode:''})
      fieldList.push({type: 'string', value: 'org_struc_param,update_by', text: '组织结构参数-更新人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'org_struc_param,update_time', text: '组织结构参数-更新日期',dictCode:''})
      fieldList.push({type: 'string', value: 'org_struc_param,sys_org_code', text: '组织结构参数-所属部门',dictCode:''})

      fieldList.push({type: 'list_multi', value: 'org_struc_param,appearance', text: '组织结构参数-形貌',dictCode:'appearance'})
      fieldList.push({type: 'string', value: 'org_struc_param,particle_size_dis', text: '组织结构参数-粒度分布',dictCode:''})
      fieldList.push({type: 'string', value: 'org_struc_param,crystal_form', text: '组织结构参数-晶型',dictCode:'crystal_form'})


      fieldList.push({type: 'string', value: 'performance_param,create_by', text: '性能参数-创建人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'performance_param,create_time', text: '性能参数-创建日期',dictCode:''})
      fieldList.push({type: 'string', value: 'performance_param,update_by', text: '性能参数-更新人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'performance_param,update_time', text: '性能参数-更新日期',dictCode:''})
      fieldList.push({type: 'string', value: 'performance_param,sys_org_code', text: '性能参数-所属部门',dictCode:''})

      fieldList.push({type: 'number', value: 'performance_param,bulk_density', text: '性能参数-松装密度（g/mL）',dictCode:''})
      fieldList.push({type: 'number', value: 'performance_param,tap_density', text: '性能参数-振实密度（g/mL）',dictCode:''})
      fieldList.push({type: 'number', value: 'performance_param,spec_surface_area', text: '性能参数-比表面积(m2/g)',dictCode:''})
      fieldList.push({type: 'number', value: 'performance_param,burn_loss110', text: '性能参数-烧损（110℃）(%)',dictCode:''})
      fieldList.push({type: 'number', value: 'performance_param,burn_loss538', text: '性能参数-烧损（538℃）(%)',dictCode:''})
      fieldList.push({type: 'number', value: 'performance_param,purity', text: '性能参数-纯度（%）',dictCode:''})



      fieldList.push({type: 'string', value: 'synthetic_process,create_by', text: '合成工艺-创建人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'synthetic_process,create_time', text: '合成工艺-创建日期',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,update_by', text: '合成工艺-更新人',dictCode:''})
      fieldList.push({type: 'datetime', value: 'synthetic_process,update_time', text: '合成工艺-更新日期',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,sys_org_code', text: '合成工艺-所属部门',dictCode:''})

      fieldList.push({type: 'string', value: 'synthetic_process,reactant_volume', text: '合成工艺-反应溶剂及其体积',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,redagent_con', text: '合成工艺-还原剂及其浓度',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,precursor_con', text: '合成工艺-前驱体及其浓度',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,proagent_con', text: '合成工艺-保护剂及其浓度',dictCode:''})
      fieldList.push({type: 'string', value: 'synthetic_process,add_contra', text: '合成工艺-助剂及其浓度',dictCode:''})
      fieldList.push({type: 'number', value: 'synthetic_process,reflex_tem', text: '合成工艺-反应温度（℃）',dictCode:''})
      fieldList.push({type: 'int', value: 'synthetic_process,reflex_time', text: '合成工艺-反应时间（分钟）',dictCode:''})
      fieldList.push({type: 'int', value: 'synthetic_process,stir_method', text: '合成工艺-搅拌条件（转/分钟）',dictCode:''})
      fieldList.push({type: 'number', value: 'synthetic_process,reactant_add_speed', text: '合成工艺-反应溶液滴加速度（升/分钟）',dictCode:''})
      fieldList.push({type: 'number', value: 'synthetic_process,reactant_ph', text: '合成工艺-反应溶液 pH 值',dictCode:''})

      this.superFieldList = fieldList
    },
    initColumns() {
      //权限过滤（列权限控制时打开，修改第二个参数为授权码前缀）
      //this.defColumns = myColAuthFilter(this.defColumns,'osp:form:V-');
      //var key = this.$route.name+":colsettings";
      //Vue.ls.set(key, checkedValues, 7 * 24 * 60 * 60 * 1000)
      this.settingColumns = this.defColumns.map(item=>{
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
      var key = this.$route.name + ":colsettings";
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