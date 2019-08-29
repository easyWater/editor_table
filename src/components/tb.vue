<template>
    <section class="data-main">      
      <section class="normal-bg">
        <h3 class="item-head">可编辑表格</h3>
        <section class="item-content">
          <Table :columns="cityColumns" border :data="tbData"></Table>
        </section>
        <section class="btn-group">
            <Button type="info">取消</Button>
            <Button type="success" style="margin-left: 24px;">提交</Button>
        </section>
      </section>
    </section>
</template>

<script>
export default {
  name: "editTb",
  data() {
    return {
      cityColumns: [
        //全市警情
        {
          title: "姓名",
          key: "name",
          render: (h, params) => {
            return this.renderInput(h, params, 'name', 'tbDataCopy')        
          }
        },
        {
          title: "性别",
          key: "gender",
          render: (h, params) => {
            // 0：男, 1：女
            // return this.renderRadio(h, params, 'gender', 'tbDataCopy')
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
              // return this.renderDate(h, params, 'birthday', 'tbDataCopy')
          }
        },
        {
          title: "所在城市",
          key: "city",
          render: (h, params) => {
              // return this.renderSelect(h, params, 'city', 'tbDataCopy')
          }
        },
        {
          title: "爱好",
          key: "hobby",
          render: (h, params) => {
              // return this.renderMulSelect(h, params, 'hobby', 'tbDataCopy')
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
                        //数据改变
                        for (let key in this.tbDataCopy[params.index]) {
                            this.tbData[params.index][key] = this.tbDataCopy[params.index][key];
                        }
                        params.row.edit = false;
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
                        params.row.edit = false;
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
                        params.row.edit = true;
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
        //全市警情
        {
          name: '张三',
          gender: 1,
          birthday: '2019/08/29',
          city: '1',
          hobby: [],
          edit: false
        },       
      ],
      tbDataCopy: [
        //全市警情
        {
          name: '张三',
          gender: 1,
          birthday: '2019/08/29',
          city: '1',
          hobby: [],
        },
      ],
    };
  },
  methods: {
    renderInput(h, params, key, copyDataStr) {
        if (params.row.edit) {
            return h("Input", {
                props: {
                    value: params.row[key],
                },
                attrs: {
                    id: `${key}_${params.index}`
                },
                on: {
                    "on-blur": () => {                      
                        this[copyDataStr][params.index][key] = document.querySelector(`#${key}_${params.index} input`).value;
                    }
                }
            });
        } else {
            return h("span", params.row[key]);
        }
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