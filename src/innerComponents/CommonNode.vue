<template>
    <div class="flex-container" :id="moduleObject.id + 'common'">
        <div class="icon-container flex-container">
            <svg-icon :icon-class="iconClass"></svg-icon>
        </div>
        <span class="common-font">{{ propData.fontContent }}</span>
    </div>
</template>

<script>
export default {
    name: 'CommonNode',
    props: {
        iconClass: {
            type: String,
            default: ''
        },
        propData: {
            type: Object,
            default: () => { }
        },
        moduleObject: {
            type: Object,
            default: () => { }
        }
    },
    data() {
        return {}
    },
    methods: {
        convertAttrToStyleObject() {
            var styleObject = {}, iconObj = {}, fontObj = {};
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
                        case 'font':
                            IDM.style.setFontStyle(styleObject, element)
                            break
                        case 'box':
                            IDM.style.setBoxStyle(styleObject, element)
                            break
                        case 'border':
                            IDM.style.setBorderStyle(styleObject, element)
                            break
                        case 'bgColor':
                            if (element.hex8) {
                                styleObject['background-color'] = IDM.hex8ToRgbaString(element.hex8) + ' !important'
                            }
                            break
                        case 'iconSize':
                            iconObj['font-size'] = element
                            break
                        case 'containerSize':
                            iconObj['width'] = element
                            iconObj['height'] = element
                            break
                        case 'containerBox':
                            IDM.style.setBoxStyle(iconObj, element)
                            break
                        case 'iconColor':
                            if (element.hex8) {
                                iconObj['color'] = element.hex8
                            }
                            break
                        case 'iconContainerBorder':
                            IDM.style.setBorderStyle(iconObj, element)
                            break

                        case 'iconContainerBgColor':
                            if (element.hex8) {
                                iconObj['background-color'] = IDM.hex8ToRgbaString(element.hex8)
                            }
                            break
                        case 'fontBox':
                            IDM.style.setBoxStyle(fontObj, element)
                            break



                    }
                }
            }
            window.IDM.setStyleToPageHead(this.moduleObject.id + 'common', styleObject);
            window.IDM.setStyleToPageHead(this.moduleObject.id + 'common .icon-container', iconObj);
            window.IDM.setStyleToPageHead(this.moduleObject.id + 'common .common-font', fontObj);
        },
    }
}
</script>

<style lang="scss" scoped>
.flex-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

</style>