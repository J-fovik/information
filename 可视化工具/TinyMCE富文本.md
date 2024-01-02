# 一、安装tinymce编辑器

**文档：**

```sh
npm i tinymce
npm i @tinymce/tinymce-vue
```

# 二、创建文件到public目录

**从 node_modules/tinymce 复制样式文件到public 目录下**

```
|public
|-- tinymce 文件夹
|---- langs 语言目录
|----- zh-Hans.js
|---- skins 皮肤目录
|------ content
|------ ui
|---- tinymce.min.js
```

:::tip 中文语言包 zh-Hans.js 下载地址
https://www.tiny.cloud/get-tiny/language-packages/



# 三、新增组件

```vue
<template>
<editor v-model="content" tag-name="div" :init="init" />
</template>

<script setup>
import tinymce from "tinymce/tinymce";
import Editor from "@tinymce/tinymce-vue";
import { ref, watch } from "vue"
import "tinymce/themes/silver/theme"; // 引用主题文件
import "tinymce/icons/default"; // 引用图标文件
import 'tinymce/models/dom'
// tinymce插件可按自己的需要进行导入
// 更多插件参考：https://www.tiny.cloud/docs/plugins/
import "tinymce/plugins/advlist"
import "tinymce/plugins/anchor"
import "tinymce/plugins/autolink"
import "tinymce/plugins/autoresize"
import "tinymce/plugins/autosave"
import "tinymce/plugins/charmap" // 特殊字符
import "tinymce/plugins/code" // 查看源码
import "tinymce/plugins/codesample" // 插入代码
import "tinymce/plugins/directionality"
import "tinymce/plugins/emoticons"
import "tinymce/plugins/fullscreen" //全屏
import "tinymce/plugins/help"
import "tinymce/plugins/image" // 插入上传图片插件
import "tinymce/plugins/importcss" //图片工具
import "tinymce/plugins/insertdatetime" //时间插入
import " /plugins/link"
import "tinymce/plugins/lists" // 列表插件
import "tinymce/plugins/media" // 插入视频插件
import "tinymce/plugins/nonbreaking"
import "tinymce/plugins/pagebreak" //分页
import "tinymce/plugins/preview" // 预览
import "tinymce/plugins/quickbars"
import "tinymce/plugins/save" // 保存
import "tinymce/plugins/searchreplace" //查询替换
import "tinymce/plugins/table" // 插入表格插件
import "tinymce/plugins/template" //插入模板
import "tinymce/plugins/visualblocks"
import "tinymce/plugins/visualchars"
import "tinymce/plugins/wordcount" // 字数统计插件

// v-model
const props = defineProps({
modelValue: String,
})
const emit = defineEmits(["update:modelValue"])
// 配置
const init = {
    language_url: '/tinymce/langs/zh-Hans.js', // 中文语言包路径
    language: "zh-Hans",
    skin_url: '/tinymce/skins/ui/oxide', // 编辑器皮肤样式
    content_css:"/tinymce/skins/content/default/content.min.css",
    menubar: false, // 隐藏菜单栏
    autoresize_bottom_margin: 50,
    max_height: 500,
    min_height: 450,
    // height: 320,
    toolbar_mode: "none",
    plugins:
        'wordcount visualchars visualblocks template searchreplace save quickbars preview pagebreak nonbreaking media insertdatetime importcss image help fullscreen directionality codesample code charmap link code table lists advlist anchor autolink autoresize autosave',
    toolbar:
        "formats undo redo fontsizeselect fontselect ltr rtl searchreplace media|outdent indent aligncenter alignleft alignright alignjustify lineheight underline quicklink h2 h3 blockquote numlist bullist table removeformat forecolor backcolor bold italic strikethrough hr link preview fullscreen help ",
    content_style: "p {margin: 5px 0; font-size: 14px}",
    fontsize_formats: "12px 14px 16px 18px 24px 36px 48px 56px 72px",
    font_formats:
        "微软雅黑=Microsoft YaHei,Helvetica Neue,PingFang SC,sans-serif;苹果苹方 =PingFang SC,Microsoft YaHei,sans-serif;宋体=simsun,serif;仿宋体=FangSong,serif;黑体=SimHei,sans-serif;Arial=arial,helvetica,sans-serif;Arial Black=arial black,avant garde;Book Antiqua=book antiqua,palatino;",
    branding: false,
    elementpath: false,
    resize: false, // 禁止改变大小
    statusbar: false, // 隐藏底部状态栏
};

tinymce.init; // 初始化
const content = ref(props.modelValue)
watch(props, (newVal) => content.value = newVal.modelValue)
watch(content, (newVal) =>emit("update:modelValue", newVal))
</script>
<style>
.tox-tinymce-aux {
z-index: 9999 !important;
}
</style>
```





# 四、自定义按钮

```vue
<script setup>
// ...
const init = {
    // ...
    toolbar: " imageUpload ",
    // ...
    // 自定义按钮
    setup: (editor) => {
        editor.ui.registry.addButton('imageUpload', {
            tooltip: '插入图片',
            icon: 'image',
            onAction: (e) => {
                let url = "url地址"
                editor.insertContent(`&nbsp;<img src="${url}" style="width:100%;" />&nbsp;`)
            }
        })
    }
};
</script>
```

