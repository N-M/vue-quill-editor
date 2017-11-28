[![](https://badge.juejin.im/entry/5852b6fc61ff4b006c89b49d/likes.svg?style=flat-square)](https://juejin.im/entry/5852b6fc61ff4b006c89b49d/detail)
[![GitHub issues](https://img.shields.io/github/issues/surmon-china/vue-quill-editor.svg?style=flat-square)](https://github.com/surmon-china/vue-quill-editor/issues)
[![GitHub forks](https://img.shields.io/github/forks/surmon-china/vue-quill-editor.svg?style=flat-square)](https://github.com/surmon-china/vue-quill-editor/network)
[![GitHub stars](https://img.shields.io/github/stars/surmon-china/vue-quill-editor.svg?style=flat-square)](https://github.com/surmon-china/vue-quill-editor/stargazers)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://raw.githubusercontent.com/surmon-china/vue-quill-editor/master/LICENSE)
[![Twitter](https://img.shields.io/twitter/url/https/github.com/surmon-china/vue-quill-editor.svg?style=social?style=flat-square)](https://twitter.com/intent/tweet?url=https://github.com/surmon-china/vue-quill-editor)

[![NPM](https://nodei.co/npm/vue-quill-editor.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/vue-quill-editor/)
[![NPM](https://nodei.co/npm-dl/vue-quill-editor.png?months=9&height=3)](https://nodei.co/npm/vue-quill-editor/)


# Vue-Quill-Editor
🍡Quill editor component for Vue, support SPA and SSR.

基于 Quill、适用于 Vue 的富文本编辑器，支持服务端渲染和单页应用。


# Example
[Demo Page](https://surmon-china.github.io/vue-quill-editor/)


# Use

### Install

``` bash
npm install vue-quill-editor --save
```


### Mount

``` javascript
// import
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'


// or require
var Vue = require('vue')
var VueQuillEditor = require('vue-quill-editor')


// require styles
require('quill/dist/quill.core.css')
require('quill/dist/quill.snow.css')
require('quill/dist/quill.bubble.css')


// mount with global
Vue.use(VueQuillEditor, /* {  default global options }*/)

// or mount with component(can't work in Nuxt.js/SSR)
import { quillEditor } from 'vue-quill-editor'

export default {
  components: {
    quillEditor
  }
}

// or mount with ssr
if (process.browser) {
  const VueQuillEditor = require('vue-quill-editor/dist/ssr')
  Vue.use(VueQuillEditor)
}

// if you need register quill modules, you need to introduce and register before the vue program is instantiated
import Quill from 'quill'
// or 
import { Quill } from 'vue-quill-editor'
import { yourQuillModule } from '../yourModulePath/yourQuillModule.js'
Quill.register('modules/yourQuillModule', yourQuillModule)
```

### Difference（使用方法的区别）

**SSR and the only difference in the use of the SPA:**
- SPA worked by the `component`, find quill instance by `ref attribute`.
- SSR worked by the `directive`, find quill instance by `directive arg`.
- Other configurations, events are the same.

### SSR

``` vue
<!-- You can custom the "myQuillEditor" name used to find the quill instance in current component -->
<template>
  <!-- bidirectional data binding（双向数据绑定） -->
  <div class="quill-editor" 
       v-model="content"
       v-quill:myQuillEditor="editorOption">
  </div>

  <!-- Or manually control the data synchronization（手动控制数据流）  -->
  <div class="quill-editor" 
       :content="content"
       @change="onEditorChange($event)"
       v-quill:myQuillEditor="editorOption">
  </div>
</template>

<script>
  export default {
    mounted() {
      console.log('this is current quill instance object', this.myQuillEditor)
    }
    // Omit the same parts as in the following component sample code
    // ...
  }
</script>
```


### SPA

``` vue
<template>
  <!-- bidirectional data binding（双向数据绑定） -->
  <quill-editor v-model="content"
                ref="myQuillEditor"
                :options="editorOption"
                @blur="onEditorBlur($event)"
                @focus="onEditorFocus($event)"
                @ready="onEditorReady($event)">
  </quill-editor>

  <!-- Or manually control the data synchronization（或手动控制数据流） -->
  <quill-editor :content="content"
                :options="editorOption"
                @change="onEditorChange($event)">
  </quill-editor>
</template>

<script>
  // You can also register quill modules in the component
  import Quill from 'quill'
  import { someModule } from '../yourModulePath/someQuillModule.js'
  Quill.register('modules/someModule', someModule)
  
  export default {
    data () {
      return {
        content: '<h2>I am Example</h2>',
        editorOption: {
          // some quill options
        }
      }
    },
    // if you need to manually control the data synchronization, parent component needs to explicitly emit an event instead of relying on implicit binding
    // 如果需要手动控制数据同步，父组件需要显式地处理changed事件
    methods: {
      onEditorBlur(editor) {
        console.log('editor blur!', editor)
      },
      onEditorFocus(editor) {
        console.log('editor focus!', editor)
      },
      onEditorReady(editor) {
        console.log('editor ready!', editor)
      },
      onEditorChange({ quill, html, text }) {
        console.log('editor change!', quill, html, text)
        this.content = html
      }
    },
    // get the current quill instace object.
    computed: {
      editor() {
        return this.$refs.myQuillEditor.quill
      }
    },
    mounted() {
      // you can use current editor object to do something(quill methods)
      console.log('this is current quill instance object', this.editor)
    }
  }
</script>
```


# Modules
- [quill-image-resize-module](https://github.com/kensnyder/quill-image-resize-module)
- [quill-image-drop-module](https://github.com/kensnyder/quill-image-drop-module)
- [more modules...](https://github.com/search?o=desc&q=quill+module&s=stars&type=Repositories&utf8=%E2%9C%93)


# Issues
- [Quill - Modules - ImageImport and ImageResize](https://www.webpackbin.com/bins/-Ket3Oz1330Cy0MbddU3)
- [Quill - toolbar - attributes](https://github.com/quilljs/quill/issues/1084)
- [Quill - Issues - Option to insert an image from a URL](https://github.com/quilljs/quill/issues/893)
- [How vue-quill-editor combine with the syntax highlighter module of highlight.js ](https://github.com/surmon-china/vue-quill-editor/issues/39)
- [如何将图片上传至七牛等服务器](https://github.com/surmon-china/vue-quill-editor/issues/102)


# Quill documents
[Api docs](https://quilljs.com/docs/quickstart/)


# Author Blog
[Surmon](https://surmon.me)
