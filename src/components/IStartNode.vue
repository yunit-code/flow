<template>
  <div idm-ctrl="idm_module" :id="moduleObject.id" :idm-ctrl-id="moduleObject.id" @click="handleNodeClick">
    <CommonNode ref="commonNode" :moduleObject="moduleObject" iconClass="start" :propData="propData"></CommonNode>
  </div>
</template>
<script>
import CommonNode from '../innerComponents/CommonNode.vue'
export default {
  name: 'IStartNode',
  components: {
    CommonNode
  },
  data() {
    return {
      moduleObject: {},
      propData: this.$root.propData.compositeAttr || {
        fontContent: "开始节点",
        width: '60px',
          textAlign: 'center',
          textVertical: 'center',
          containerSize: '100%',
          height: '80px',
          iconSize: '40px',
          iconColor: {
            hex8: '#FFFFFFFF'
          },
          iconContainerBgColor: {
            hex8: '#5FB93AFF'
          },
          iconContainerBorder: {
            border: {
              top: {
                  style: 'solid',
                  width: 0,
                  widthUnit: 'px',
                  colors: {
                      hex8: ''
                  }
              },
              right: {
                  style: 'solid',
                  width: 0,
                  widthUnit: 'px',
                  colors: {
                      hex8: ''
                  }
              },
              bottom: {
                  style: 'solid',
                  width: 0,
                  widthUnit: 'px',
                  colors: {
                      hex8: ''
                  }
              },
              left: {
                  style: 'solid',
                  width: 0,
                  widthUnit: 'px',
                  colors: {
                      hex8: ''
                  }
              }
          },
          radius: {
              leftTop: {
                  radius: 50,
                  radiusUnit: '%'
              },
              rightTop: {
                  radius: 50,
                  radiusUnit: '%'
              },
              leftBottom: {
                  radius: 50,
                  radiusUnit: '%'
              },
              rightBottom: {
                  radius: 50,
                  radiusUnit: '%'
              }
          }
        }
      }
    }
  },
  created() {
    this.moduleObject = this.$root.moduleObject
    this.convertAttrToStyleObject();
  },
  methods: {
    propDataWatchHandle(propData) {
      this.propData = propData.compositeAttr || {};
      this.convertAttrToStyleObject();
    },
    convertAttrToStyleObject() {
      this.$nextTick(() => {
        this.$refs['commonNode'].convertAttrToStyleObject()
        this.initData();
      })
    },
    reload() {
      //请求数据源
      this.initData();
    },
    initData() {
      let that = this;
      //所有地址的url参数转换
      var params = that.commonParam();
      switch (this.propData.dataSourceType) {
        case "customInterface":
          this.propData.customInterfaceUrl && window.IDM.http.get(this.propData.customInterfaceUrl, params)
            .then((res) => {
              //res.data
              that.$set(that.propData, "fontContent", that.getExpressData("resultData", that.propData.dataFiled, res.data));
              // that.propData.fontContent = ;
            })
            .catch(function (error) { });
          break;
        case "pageCommonInterface":
          //使用通用接口直接跳过，在setContextValue执行
          break;
        case "customFunction":
          if (this.propData.customFunction && this.propData.customFunction.length > 0) {
            var resValue = "";
            try {
              resValue = window[this.propData.customFunction[0].name] && window[this.propData.customFunction[0].name].call(this, { ...params, ...this.propData.customFunction[0].param, moduleObject: this.moduleObject });
            } catch (error) {
            }
            that.propData.fontContent = resValue;
          }
          break;
      }
    },
    handleNodeClick() {
      let that = this;
      if (this.moduleObject.env == "develop") {
        //开发模式下不执行此事件
        return;
      }
      //获取所有的URL参数、页面ID（pageId）、以及所有组件的返回值（用范围值去调用IDM提供的方法取出所有的组件值）
      let urlObject = window.IDM.url.queryObject(),
        pageId = window.IDM.broadcast && window.IDM.broadcast.pageModule ? window.IDM.broadcast.pageModule.id : "";
      //自定义函数
      /**
       * [
       * {name:"",param:{}}
       * ]
       */
      var clickFunction = this.propData.clickFunction;
      clickFunction && clickFunction.forEach(item => {
        window[item.name] && window[item.name].call(this, {
          urlData: urlObject,
          pageId,
          customParam: item.param,
          _this: this
        });
      })
    },
    receiveBroadcastMessage(object) {
      console.log("组件收到消息", object)
      if (object.type && object.type == "linkageShowModule") {
        this.showThisModuleHandle();
      } else if (object.type && object.type == "linkageHideModule") {
        this.hideThisModuleHandle();
      }
    },
    setContextValue(object) {
      console.log("统一接口设置的值", object);
      if (object.type != "pageCommonInterface") {
        return;
      }
      //这里使用的是子表，所以要循环匹配所有子表的属性然后再去设置修改默认值
      if (object.key == this.propData.dataName) {
        // this.propData.fontContent = this.getExpressData(this.propData.dataName,this.propData.dataFiled,object.data);
        this.$set(this.propData, "fontContent", this.getExpressData(this.propData.dataName, this.propData.dataFiled, object.data));
      }
    },
    sendBroadcastMessage(object) {
      window.IDM.broadcast && window.IDM.broadcast.send(object);
    },
    /**
   * 通用的url参数对象
   * 所有地址的url参数转换
   */
    commonParam() {
      let urlObject = IDM.url.queryObject();
      var params = {
        pageId:
          window.IDM.broadcast && window.IDM.broadcast.pageModule
            ? window.IDM.broadcast.pageModule.id
            : "",
        urlData: JSON.stringify(urlObject),
      };
      return params;
    },
  }
};
</script>