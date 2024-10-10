<template>
  <div>
    <div class="buttons">
      <button @click="toggleEditorReadOnly">编辑器开关</button>
      <button class="my-custom-button">点击我不会触发blur事件</button>
      <button @click="appendContentInTinymce">你可以点击我到富文本</button>
      <button draggable="true" @dragstart="(event) => onDragStart(event)">
        你可以拖动我到富文本
      </button>
      <button
        draggable="true"
        @dragstart="(event) => onDragStartMceNonEditable(event)"
      >
        你可以拖动我到富文本，我的内容是一块整体
      </button>
    </div>
    <!-- <textarea ref="tinymce" :id="textareaId"></textarea> -->
    <Editor ref="tinymce" api-key="v08ora9353uw27k7405uh2gwmjde1rjh542gctvw08wczc7v" :init="defaultOptions" />
  </div>
</template>
 
<script>
// import tinymce from "tinymce/tinymce";
// import "tinymce/themes/silver";

import Editor from '@tinymce/tinymce-vue'

export default {
  components: {
    Editor
  },
  props: {
    value: String,
    options: Object,
  },
  data() {
    return {
      defaultOptions: {},
      editor: null,
    };
  },
  created() {
    this.initializeEditor();
  },
  watch: {
    value(newValue) {
      if (newValue !== this.editor.getContent()) {
        this.editor.setContent(newValue);
      }
    },
  },
  methods: {
    initializeEditor() {
      const defaultOptions = {
        /**
         * 插件
         * autoresize 可以根据内容自动调整编辑器的高度
         * code 显示TinyMCE编辑区的原始html
         * lists,advlist 高级列表插件,扩展了默认的UL (bullist) 和OL (numlist)列表样式.另外该插件也提供了自定义的选项advlist_bullet_styles, advlist_number_styles
         * codesample 代码示例插件,扩展codesample_languages
         * directionality 将文本放到左边或右边ltr rtl
         * image 图像插件
         * editimage 编辑图像插件 要收费
         * emoticons 表情库
         * export 导出pdf插件 要收费
         * formatpainter 格式刷 要收费
         * fullscreen 全屏插件
         * help 帮助
         * insertdatetime 插入日期
         * link 链接
         * media 该插件为用户提供了将 HTML5 视频和音频元素添加到可编辑区域的能力
         * nonbreaking 插入不间断空格实体
         * pagebreak 使用户能够在可编辑区域插入分页符
         * preview 预览
         * save 保存
         * searchreplace 搜索和替换插件
         * template 模板插件.即将到来的 TinyMCE 7.0 版本中完全删除。作为替代解决方案，我们建议使用 Advanced Template Premium 插件
         * visualblocks 该插件允许用户在可编辑区域中查看块级元素
         * wordcount 字数统计
         */
        plugins:
          "autoresize code codesample  lists advlist accordion table fullscreen anchor directionality emoticons help image insertdatetime media nonbreaking pagebreak preview save searchreplace visualblocks wordcount",
        autoresize_overflow_padding: 0,
 
        /**
         * 工具栏
         * undo 撤销
         * redo 重做
         * bold 加粗
         * italic 斜体
         * alignleft aligncenter alignright alignjustify 文本对齐
         * outdent 减小当前段落或列表项的缩进级别
         * indent 增加当前段落或列表项的缩进级别
         * lineheight 行高的下拉列表
         * code 显示TinyMCE的html
         * bullist 创建或移除无序列表
         * numlist 创建或移除有序列表
         * backcolor 将背景色应用于选区
         * blocks 标题列表
         * copy 复制到剪切板
         * copy 将当前所选内容剪切到剪贴板中
         * fontfamily 字体系列的下拉列表
         * fontsize 字体大小的下拉列表
         * fontsizeinput 输入字段提供增大和减小字体大小按钮
         * forecolor 修改文本颜色
         * h1-h6 文本标题
         * hr 插入水平线
         * newdocument 创建一个新文档
         * pastetext 打开/关闭纯文本粘贴模式
         * print 打印当前编辑器内容
         * remove 移除（删除）所选内容或光标位置之前的内容
         * removeformat 从当前选定内容中删除格式
         * selectall 选择编辑器中的所有内容
         * strikethrough 将删除线格式应用于当前选区
         * styles 所选内容的样式的下拉列表
         * subscript 将下标格式应用于当前选定内容
         * superscript 将上标格式应用于当前选定内容
         * underline 将下划线格式应用于当前选定内容
         * undo 撤消上一个操作
         * visualaid 切换不可见元素的视觉辅助工具
         */
        toolbar:
          "undo redo | bold italic | alignleft aligncenter alignright alignjustify | outdent indent lineheight accordion| bullist numlist backcolor forecolor blocks fontfamily styles fontsize fontsizeinput h1 h2 h3 h4 h5 h6 | copy cut newdocument pastetext print remove  selectall code codesample  | hr strikethrough subscript superscript underline removeformat visualaid | table tableinsertdialog tablecellprops tableprops  fullscreen anchor ltr rtl emoticons help image media insertdatetime link nonbreaking pagebreak preview save searchreplace visualblocks wordcount",
        autoresize_bottom_margin: 16, // 自动调整高度的底部边距
        height: 500, // 编辑器高度
        min_height: 500, // 最小编辑器高度
        placeholder: "请输入...", // 占位符
        // advlist_bullet_styles: "square", // 无序列表是否使用方形
        advlist_number_styles:
          "lower-alpha,lower-roman,upper-alpha,upper-roman", // 小写字母，小写罗马数字，大写字母，大写罗马数字
        auto_focus: true, // 让编辑器加载完成后自动获得光标焦点
        cache_suffix: Math.random().toString(36).substring(7), // 可在TinyMCE加载js和css文件时，在URL请求后面自动加上指定的后缀，多用于解决缓存问题。
        // content_security_policy: "default-src 'self'",//内容安全策略.仅允许当前域名，不包括子域名
        // external_plugins: {
        //   // 引入站外插件.可用于引入本站外部提供的插件，TinyMCE将根据插件加载规则加载指定URL的插件。当从CDN加载TinyMCE或希望TinyMCE主目录与自定义插件分开时，可使用此配置。
        //   testing: "http://www.testing.com/plugin.min.js",
        //   maths: "http://www.maths.com/plugin.min.js",
        // },
        readonly: false, // 是否只读模式
        // suffix: '.min', // 后缀。如果主程序叫tinymce.js，它在加载插件时就会去找插件文件夹里的plugin.js；如果主程序叫tinymce.min.js，它在加载插件时就会去找plugin.min.js。
        // target: el, // 使用Node替代selector
        custom_ui_selector: ".my-custom-button", // 可指定某些元素成为编辑器的一部分，当焦点移动到此选择器匹配的元素上时，不会触发编辑器的blur事件
        highlight_on_focus: true, // 获得输入焦点时，编辑器添加蓝色轮廓
        toolbar_mode: "wrap", // 不会被折叠隐藏
        codesample_languages: [
          { text: "HTML/XML", value: "markup" },
          { text: "JavaScript", value: "javascript" },
          { text: "CSS", value: "css" },
          { text: "PHP", value: "php" },
          { text: "Ruby", value: "ruby" },
          { text: "Python", value: "python" },
          { text: "Java", value: "java" },
          { text: "C", value: "c" },
          { text: "C#", value: "csharp" },
          { text: "C++", value: "cpp" },
        ],
        extended_valid_elements:
          "script[src],span[class|style|title],table[class|style]", // 给元素添加有效属性
        save_onsavecallback: () => {
          console.log("Saved");
        },
        // 初始化前执行
        setup: (editor) => {
          this.editor = editor;
 
          editor.on("change", () => {
            this.$emit("input", editor.getContent());
          });
 
          editor.on("blur", () => {
            console.log("触发了 blur");
          });
 
          editor.on("dragover", function (event) {
            console.log("dragover");
            // event.preventDefault();
          });
 
          editor.on("drop", function (event) {
            console.log("drop");
            // event.preventDefault();
            // var htmlContent = event.dataTransfer.getData("text/html");
          });
        },
        // 初始化结束后执行
        init_instance_callback: function (editor) {
          console.log("ID为: " + editor.id + " 的编辑器已初始化完成.");
        },
      };
      this.defaultOptions = defaultOptions;
    },
    toggleEditorReadOnly() {
      const status = this.editor.mode.get();
 
      if (status === "design") {
        this.editor.mode.set("readonly"); // 设为设计模式，允许编辑
      } else {
        this.editor.mode.set("design"); // 设为只读模式
      }
    },
    appendContentInTinymce() {
      let html = `<span style="color:red;">我是点击后到富文本的。</span>`;
      this.editor.insertContent(html);
    },
    onDragStart(event) {
      let html = `
      <table style="border-collapse: collapse;width: 80%;margin-left: 20%;" border="1">
        <tbody>
          <tr>
            <td colspan="3" style="width: 100%;">
              <div style="color:blue;text-align:center;">我是被拖进来的</div>
            </td>
          </tr>
          <tr>
            <td style="width: 33.3333%;"></td>
            <td style="width: 33.3333%;"></td>
            <td style="width: 33.3333%;"></td>
          </tr>
          <tr>
            <td style="width: 33.3333%;"></td>
            <td style="width: 33.3333%;"></td>
            <td style="width: 33.3333%;"></td>
          </tr>
        </tbody>
      </table>
    `;
      event.dataTransfer.setData("text/html", html);
    },
    onDragStartMceNonEditable(event) {
      let html = `<span style="color:green;" class="mceNonEditable">我是被拖进来的，我是一块整体。</span>`;
      // 
      event.dataTransfer.setData("text/html", html);
      // this.editor.focus()
    },
  },
  beforeDestroy() {
    if (this.editor) {
      this.editor.destroy();
    }
  },
};
</script>
<style scoped>
.buttons {
  padding-bottom: 30px;
}
button {
  margin-left: 15px;
  cursor: pointer;
}
</style>
