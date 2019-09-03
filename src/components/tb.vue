<template>
    <section class="data-main">      
      <section class="normal-bg">
        <h3 class="item-head">可编辑表格</h3>
        <section class="item-content">
          <Table :columns="tbColumns" border :data="tbData"></Table>
        </section>
        <section class="btn-group">
            <Button type="info">取消</Button>
            <Button type="success" style="margin-left: 24px;" @click="handleSubmit">提交</Button>
        </section>
      </section>
    </section>
</template>

<script>
export default {
  name: "editTb",
  data() {
    return {
      tbColumns: [ //主要是render不同
        {
          title: "姓名",
          key: "name",
          render: (h, params) => {
            if (params.row.edit) {
              return h("Input", {
                  props: {
                      value: params.row.name,
                  },
                  attrs: {
                      id: `name_${params.index}`
                  },
                  on: {
                      "on-blur": () => {
                        this.tbDataCopy[params.index].name = document.querySelector(`#name_${params.index} input`).value;
                      }
                  }
              });
            } else {
                return h("span", params.row.name);
            }  
          }
        },
        {
          title: "性别",
          key: "gender",
          render: (h, params) => {
            // 0：男, 1：女
            if(params.row.edit) {
              return h('RadioGroup', {
                props: {
                  value: params.row.gender
                },
                on: {
                  'on-change': (val) => {
                    this.tbDataCopy[params.index].gender = val
                  }
                }
              },[
                h('Radio', {
                  props: {
                    label: 0
                  }
                }, '男'),
                h('Radio', {
                  props: {
                    label: 1
                  }
                }, '女')
              ])
            }else {
              return h('span', {}, params.row.gender === 0 ? '男' : '女')
            }
          }
        },
        {
          title: "出生日期",
          key: "birthday",
          render: (h, params) => {
            if(params.row.edit) {
              return h('DatePicker', {
                props: {
                  type: 'date',
                  placeholder: '请选择出生日期',
                  options: this.dateOption,
                  value: params.row.birthday,
                  transfer: true
                },
                on: {
                  'on-change': (val) => {
                    this.tbDataCopy[params.index].birthday = val
                  }
                }
              })
            }else {
              return h('span', {}, params.row.birthday)
            }
          }
        },
        {
          title: "所在城市",
          key: "city",
          render: (h, params) => {
            if(params.row.edit) {
              let children = [] //模拟处理接口请求的数据
              this.cityList.forEach(city => {
                children.push(h('Option', {props: {value: city.value}}, city.label))
              })
              return h('Select', {
                props: {
                  value: params.row.city,
                  transfer: true
                },
                on: {
                  'on-change': (val) => {
                    this.tbDataCopy[params.index].city = val
                  }
                }
              }, children)
            }else {
              let cityName = ''
              this.cityList.forEach(city => {
                if(city.value === params.row.city) {
                  cityName = city.label
                }
              })
              return h('span', {}, cityName)
            }
          }
        },
        {
          title: "爱好",
          key: "hobby",
          render: (h, params) => {
            if(params.row.edit) {
              let children = [] //模拟处理接口请求的数据
              this.hobbyList.forEach(hobby => {
                children.push(h('Option', {props: {value: hobby.value}}, hobby.label))
              })
              return h('Select', {
                props: {
                  value: params.row.hobby,
                  transfer: true,
                  multiple: true
                },
                on: {
                  'on-change': (valList) => {
                    this.tbDataCopy[params.index].hobby = valList
                  }
                }
              }, children)
            }else {
              let hobbys = []
              params.row.hobby.forEach(item => {
                this.hobbyList.forEach(hobby => {
                  if(hobby.value === item) {
                    hobbys.push(hobby.label)
                  }
                })
              })              
              return h('span', {}, hobbys.join(','))
            }
          }
        },
        {
          title: "操作",
          align: "center",
          render: (h, params) => {
            if (params.row.edit) {
              return h("div", [
                h(
                  "span",
                  {
                    style: {
                      cursor: "pointer",
                      color: "#81d740"
                    },
                    on: {
                      click: () => {
                        //保存数据
                        for (let key in this.tbDataCopy[params.index]) {
                            this.tbData[params.index][key] = this.tbDataCopy[params.index][key];
                        }
                        this.tbData[params.index].edit = false;
                      }
                    }
                  },
                  "保存"
                ),
                h(
                  "span",
                  {
                    style: {
                      cursor: "pointer",
                      color: "#4d85e8",
                      "margin-left": "20px"
                    },
                    on: {
                      click: () => {
                        this.tbData[params.index].edit = false;
                      }
                    }
                  },
                  "取消"
                )
              ]);
            } else {
              return h("div", [
                h(
                  "span",
                  {
                    style: {
                      cursor: "pointer",
                      color: "red"
                    },
                    on: {
                      click: () => {
                        this.tbData[params.index].edit = true;
                      }
                    }
                  },
                  "编辑"
                )
              ]);
            }
          }
        }
      ],
      tbData: [
        //表格数据
        {
          name: '张三',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['movie', 'sing'],
          edit: false
        }, 
        {
          name: '李四',
          gender: 0,
          birthday: '2019/08/30',
          city: 'shenzhen',
          hobby: ['sing'],
          edit: false
        }, 
        {
          name: '小李',
          gender: 0,
          birthday: '2019/09/01',
          city: 'shanghai',
          hobby: ['movie', 'stroll'],
          edit: false
        }, 
        {
          name: '小张',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['travel'],
          edit: false
        }, 
        {
          name: '小徐',
          gender: 0,
          birthday: '2019/09/01',
          city: 'shenzhen',
          hobby: ['sing'],
          edit: false
        }, 
        {
          name: '小杨',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['movie', 'sing'],
          edit: false
        },       
      ],
      tbDataCopy: [
        //表格数据副本
        {
          name: '张三',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['movie', 'sing'],
        },
        {
          name: '李四',
          gender: 0,
          birthday: '2019/08/30',
          city: 'shenzhen',
          hobby: ['sing'],
        }, 
        {
          name: '小李',
          gender: 0,
          birthday: '2019/09/01',
          city: 'shanghai',
          hobby: ['movie', 'stroll'],
        }, 
        {
          name: '小张',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['travel'],
        }, 
        {
          name: '小徐',
          gender: 0,
          birthday: '2019/09/01',
          city: 'shenzhen',
          hobby: ['sing'],
        }, 
        {
          name: '小杨',
          gender: 1,
          birthday: '2019/08/29',
          city: 'beijing',
          hobby: ['movie', 'sing'],
        },
      ],
      dateOption: {
        shortcuts: [
            {
                text: '近一周',
                value () {                      
                    const start = new Date();
                    start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                    return start;
                }
            },
            {
                text: '近一月',
                value () {
                    const start = new Date();
                    start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                    return start;
                }
            },
            {
                text: '近三月',
                value () {
                    const start = new Date();
                    start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                    return start;
                }
            }
        ]
      },
      cityList: [
        {
          value: 'beijing',
          label: '北京'
        },
        {
          value: 'shanghai',
          label: '上海'
        },
        {
          value: 'shenzhen',
          label: '深圳'
        }
      ],
      hobbyList: [
        {
          value: 'movie',
          label: '电影'
        },
        {
          value: 'travel',
          label: '旅行'
        },
        {
          value: 'sing',
          label: 'k歌'
        },
        {
          value: 'stroll',
          label: '散步'
        }
      ]
    };
  },
  methods: {
    handleSubmit() {
      this.$Message.success('请于控制台查看输出')
      let params = []
      this.tbData.forEach(row => {
        params.push({
          name: row.name,
          gender: row.gender,
          birthday: row.birthday,
          city: row.city,
          hobby: row.hobby,
        })
      })
      console.log('表格数据：', params)
    }
  }
};
</script>

<style lang="scss" scoped>
.data-main {
   height: 100%;
   background-color: #1c3d67;
  .item-head {
    font-size: 18px;
    color: #6ad1fe;
    padding: 20px;
  }
  .item-content {
    border-top: 1px solid #275c9e;
    padding: 20px;
  }
  .btn-group {
    padding: 16px;
    text-align: center;
  }
}
</style>