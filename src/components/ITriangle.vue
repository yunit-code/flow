<template>
  <div idm-ctrl="idm_module" :id="moduleObject.id" class="d-flex diamond-container" :idm-ctrl-id="moduleObject.id"
    @click="textClickHandle">
  </div>
</template>
<script>
export default {
  name: 'ITriangle',
  data() {
    return {
      moduleObject: {},
      propData: this.$root.propData.compositeAttr || {}
    }
  },
  created() {
    this.moduleObject = this.$root.moduleObject
    this.convertAttrToStyleObject();
    this.convertThemeListAttrToStyleObject()
  },
  methods: {
    propDataWatchHandle(propData) {
      this.propData = propData.compositeAttr || {};
      this.convertAttrToStyleObject();
      this.convertThemeListAttrToStyleObject()
    },
    convertAttrToStyleObject() {
      var styleObject = {};
      for (const key in this.propData) {
        if (this.propData.hasOwnProperty.call(this.propData, key)) {
          const element = this.propData[key];
          if (!element && element !== false && element != 0) {
            continue;
          }
          switch (key) {
            case "width":
            case "height":
              styleObject[key] = element;
              break;
            case 'box':
              IDM.style.setBoxStyle(styleObject, element)
              break
            case 'borderColor':
              if (element.hex8) {
                styleObject['background-color'] = IDM.hex8ToRgbaString(element.hex8) + ' !important'
              }
              break

          }
        }
      }
      window.IDM.setStyleToPageHead(this.moduleObject.id, {
        ...styleObject,
        'clip-path': this.handleGetPolygon(this.propData.rightAnglePosition)
      });
      this.$nextTick(() => {
        const { width, height } = document.getElementById(this.moduleObject.id).getBoundingClientRect()
        window.IDM.setStyleToPageHead(this.moduleObject.id + '::before', {
          content: '" "',
          display: 'block',
          position: 'absolute',
          height: '100%',
          width: '100%',
          'background-color': this.propData?.bgColor?.hex8 + ' !important',
          'clip-path': this.handleGetPolygon(this.propData.rightAnglePosition, this.propData.borderWidth, width, height),
        });
        this.initData();
      })
    },
    /**
     * ???????????????????????????
     * @param {*} position ????????????
     * @param {*} difference ??????
     * @param {*} width ???????????????
     * @param {*} height ???????????????
     */
    handleGetPolygon(position, difference = 0, width = 0, height = 0) {
      let polygon = '', vx = 0, vy = 0
      if (difference !== 0) {
        // ???????????????????????????????????????????????????????????????
        const minWidth = width / (height / difference)
        const minHeight = height / (width / difference)
        // ????????????????????????????????????????????????
        vx = Math.sqrt(Math.pow(minWidth, 2) + Math.pow(difference, 2)) + minWidth
        vy = Math.sqrt(Math.pow(minHeight, 2) + Math.pow(difference, 2)) + minHeight
      }
      switch (position) {
        case 'leftTop':
          polygon = `polygon(${difference}px ${difference}px, calc(100% - ${vx}px) ${difference}px, ${difference}px calc(100% - ${vy}px))`
          break
        case 'leftBottom':
          polygon = `polygon(${difference}px ${vy}px, calc(100% - ${vx}px) calc(100% - ${difference}px), ${difference}px calc(100% - ${difference}px))`
          break
        case 'rightTop':
          polygon = `polygon(${vx}px ${difference}px, calc(100% - ${difference}px) ${difference}px, calc(100% - ${difference}px) calc(100% - ${vy}px))`
          break
        case 'rightBottom':
          polygon = `polygon(calc(100% - ${difference}px) ${vy}px, calc(100% - ${difference}px) calc(100% - ${difference}px), ${vx}px calc(100% - ${difference}px))`
          break
      }
      return polygon
    },
    reload() {
      //???????????????
      this.initData();
    },
    initData() {
      let that = this;
      //???????????????url????????????
      var params = that.commonParam();
      switch (this.propData.dataSourceType) {
        case "pageCommonInterface":
          //????????????????????????????????????setContextValue??????
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
    /**
     * ????????????
     */
    convertThemeListAttrToStyleObject() {
      var themeList = this.propData.themeList;
      if (!themeList) {
        return;
      }
      const themeNamePrefix =
        IDM.setting &&
          IDM.setting.applications &&
          IDM.setting.applications.themeNamePrefix
          ? IDM.setting.applications.themeNamePrefix
          : "idm-theme-";
      for (var i = 0; i < themeList.length; i++) {
        var item = themeList[i];
        //item.key?????????????????????key
        //item.mainColor??????????????????
        //item.minorColor??????????????????
        // if(item.key!=IDM.theme.getCurrentThemeInfo()){
        //     //?????????????????????????????????????????????????????????????????????????????????????????????
        //     continue;
        // }
        let bgColorObj = {
          'background-color': item.mainColor ? IDM.hex8ToRgbaString(item.mainColor.hex8) : "",
        }, borderColorObj = {
          'background-color': item.minorColor ? IDM.hex8ToRgbaString(item.minorColor.hex8) : "",
        };
        IDM.setStyleToPageHead(
          "." +
          themeNamePrefix +
          item.key +
          " #" +
          (this.moduleObject.id + '::before' || "module_demo"),
          bgColorObj
        );
        IDM.setStyleToPageHead(
          "." +
          themeNamePrefix +
          item.key +
          " #" +
          (this.moduleObject.id || "module_demo"),
          borderColorObj
        );
      }
    },
    textClickHandle() {
      let that = this;
      if (this.moduleObject.env == "develop") {
        //?????????????????????????????????
        return;
      }
      //???????????????URL???????????????ID???pageId????????????????????????????????????????????????????????????IDM??????????????????????????????????????????
      let urlObject = window.IDM.url.queryObject(),
        pageId = window.IDM.broadcast && window.IDM.broadcast.pageModule ? window.IDM.broadcast.pageModule.id : "";
      //???????????????
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
    receiveBroadcastMessage(messageObject) {
      console.log("??????????????????", messageObject)
      switch (messageObject.type) {

      }
    },
    /**
  * ??????????????????????????????????????????
  */
    getExpressData(dataName, dataFiled, resultData) {
      //???defaultValue??????dataFiled??????
      var _defaultVal = undefined;
      if (dataFiled) {
        var filedExp = dataFiled;
        filedExp =
          dataName +
          (filedExp.startsWiths("[") ? "" : ".") +
          filedExp;
        var dataObject = { IDM: window.IDM };
        dataObject[dataName] = resultData;
        _defaultVal = window.IDM.express.replace.call(
          this,
          "@[" + filedExp + "]",
          dataObject
        );
      }
      //????????????????????????????????????
      if (this.propData.customFunction && this.propData.customFunction.length > 0) {
        var params = this.commonParam();
        var resValue = "";
        try {
          resValue = window[this.propData.customFunction[0].name] && window[this.propData.customFunction[0].name].call(this, {
            ...params,
            ...this.propData.customFunction[0].param,
            moduleObject: this.moduleObject,
            expressData: _defaultVal, interfaceData: resultData
          });
        } catch (error) {
        }
        _defaultVal = resValue;
      }

      return _defaultVal;
    },
    setContextValue(object) {
      console.log("????????????????????????", object);
      if (object.type != "pageCommonInterface") {
        return;
      }
      //??????????????????????????????????????????????????????????????????????????????????????????????????????
      if (object.key == this.propData.dataName) {
        // this.propData.fontContent = this.getExpressData(this.propData.dataName,this.propData.dataFiled,object.data);
        this.$set(this.propData, "fontContent", this.getExpressData(this.propData.dataName, this.propData.dataFiled, object.data));
      }
    },
    sendBroadcastMessage(object) {
      window.IDM.broadcast && window.IDM.broadcast.send(object);
    },
    /**
   * ?????????url????????????
   * ???????????????url????????????
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

<style lang="scss" scoped>
.fontContent {
  font: inherit;
  position: absolute;
  width: 100%;
}
</style>