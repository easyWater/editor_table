<template>
    <section class="data-main">      
      <section class="normal-bg">
        <h3 class="item-head">可编辑表格</h3>
        <section class="item-content">
          <Table :columns="cityColumns" border :data="citySituation"></Table>
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
          title: "全市警情数量",
          key: "city_alert_total",
          render: (h, params) => {
            return this.renderRow(h, params, 'city_alert_total', 'citySituationCopy')        
          }
        },
        {
          title: "全市诈骗警情数量",
          key: "city_fraud_total",
          render: (h, params) => {
            return this.renderRow(h, params, 'city_fraud_total', 'citySituationCopy')
          }
        },
        {
          title: "全区警情数量",
          key: "alert_total",
          render: (h, params) => {
              return this.renderRow(h, params, 'alert_total', 'citySituationCopy')
          }
        },
        {
          title: "全区诈骗警情数量",
          key: "fraud_total",
          render: (h, params) => {
              return this.renderRow(h, params, 'fraud_total', 'citySituationCopy')
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
                        for (let key in this.citySituationCopy[params.index]) {
                            params.row[key] = this.citySituationCopy[params.index][key];
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
      citySituation: [
        //全市警情
        {
          city_alert_total: 100,
          city_fraud_total: 80,
          alert_total: 80,
          fraud_total: 60,
          edit: false
        },       
      ],
      citySituationCopy: [
        //全市警情
        {
          city_alert_total: 100,
          city_fraud_total: 80,
          alert_total: 80,
          fraud_total: 60,
        },
      ],
    };
  },
  methods: {
    renderRow(h, params, key, copyDataStr) { //全市警情统计表格渲染行
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
  .normal-bg {    
    border: 1px solid #275c9e;
    margin-bottom: 20px;
  }
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