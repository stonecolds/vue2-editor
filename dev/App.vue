<template>
<div id="app">
  <div class="container grid-960">
    <h1>Vue2Editor - Upload Images Example </h1>
    <div class="columns">
      <div class="editorWrapper column col-6 col-sm-12">
        <button @click.prevent="focusEditor">Focus Editor</button>
        <vue-editor
          @focus="onEditorFocus"
          @blur="onEditorBlur"
          @selection-change="onSelectionChange"
          ref="editor"
          @imageAdded="handleImageAdded"
          :editorOptions="editorSettings"
          v-model="editor2Content">
        </vue-editor>
        <button class="btn btn-primary" @click="saveContent(editor2Content)">Save</button>

        <div v-html="editor2Content"></div>
      </div>
    </div>
  </div>
</div>
</template>

<script>

// import { VueEditor, Quill } from '../src/index.js'
import { VueEditor, Quill } from '../dist/vue2-editor.js'

import axios from 'axios'
// import ImageResize from 'quill-image-resize-module';
// import { ImageDrop } from 'quill-image-drop-module'

const CLIENT_ID = '993793b1d8d3e2e'
// Quill.register('modules/imageDrop', ImageDrop)
// Quill.register('modules/imageResize', ImageResize)

export default {
  components: {
    VueEditor,
  },
  data() {
    return {
      editor1Content: '<h2>Editor 1 Starting content</h2>',
      editor2Content: '<h2>Editor 2 Starting content</h2>',
      showPreview: true,
      editor1IsDisabled: false,
      editor2IsDisabled: false,
      // customModulesForEditor: [
      //   { alias: 'imageDrop', module: ImageDrop },
      //   { alias: 'imageResize', module: ImageResize }
      // ],
      editorSettings: {
        // modules: {
        //   imageDrop: true,
        //   imageResize: {}
        // }
      },
      // editorSettings: {
      //   modules: {
      //     history: {
      //       delay: 1000,
      //       maxStack: 50,
      //       userOnly: false
      //     }
      //   }
      // },
      customToolbar: [
        ['bold', 'italic', 'underline'],
        [{
          'list': 'ordered'
        }, {
          'list': 'bullet'
        }],
        ['image', 'code-block']
      ]
    }
  },
  created() {

  },
  methods: {
    focusEditor() {
      this.$refs.editor.quill.focus();
    },

    onSelectionChange(range) {
      console.log('selection change!', range);
    },

    onEditorBlur(quill) {
      console.log('editor blur!', quill)
    },

    onEditorFocus(quill) {
      console.log('editor focus!', quill)
    },

    onEditorReady(quill) {
      console.log('editor ready!', quill)
    },

    onEditorChange({ quill, html, text }) {
      console.log('editor change!', quill, html, text)
      // this.content = html
    },

    setEditor1(editor) {
      this.editor1Content = 'Set Editor 1 Content'
    },

    setEditor2(editor) {
      this.editor2Content = 'Set Editor 2 Content'
    },

    saveContent(content) {
      console.log(content);
      // console.log(this.editorContent);
    },

    toggleDisabledForEditor1() {
      this.editor1IsDisabled = !this.editor1IsDisabled
    },
    toggleDisabledForEditor2() {
      this.editor2IsDisabled = !this.editor2IsDisabled
    },
    sendUrlToEditor() {
      console.log('worked');
    },
    handleImageAdded(file, Editor, cursorLocation) {

      console.log('Using Method in Parent');
      //
      // var formData = new FormData();
      // formData.append('image', file)
      //

      alert('We have an image!')
      return;
      axios({
        url: 'https://api.imgur.com/3/image',
        method: 'POST',
        headers:{
          'Authorization': 'Client-ID ' + CLIENT_ID
        },
        data: formData
      })
      .then((result) => {
        console.log(result);
        let url = result.data.data.link
        Editor.insertEmbed(cursorLocation, 'image', url);
      })
      .catch((err) => {
        console.log(err);
      })
      // axios({
      //   url: CLOUDINARY_URL,
      //   method: 'POST',
      //   headers:{
      //     'Content-Type': 'application/x-www-form-urlencoded'
      //   },
      //   data: formData
      // })
      // .then((result) => {
      //   let url = result.data.url
      //   Editor.insertEmbed(cursorLocation, 'image', url);
      // })
      // .catch((err) => {
      //   console.log(err);
      // })

    },
    uploadImage(file, Editor, cursorLocation) {
      var formData = new FormData();
      // formData.append('file', file)
      // formData.append('upload_preset', UPLOAD_PRESET)
      //
      //

      // var settings = {
      //   "async": true,
      //   "crossDomain": true,
      //   "url": "https://api.imgur.com/3/image",
      //   "method": "POST",
      //   "headers": {
      //     "authorization":
      //   },
      //   "processData": false,
      //   "contentType": false,
      //   "mimeType": "multipart/form-data",
      //   "data": formData
      // }


      axios({
        url: 'https://api.imgur.com/3/image',
        method: 'POST',
        headers:{
          'Content-Type': 'application/x-www-form-urlencoded',
          'authorization': 'Client-ID' + CLIENT_ID
        },
        data: formData
      })
      .then((result) => {
        let url = result.data.url
        Editor.insertEmbed(cursorLocation, 'image', url);
      })
      .catch((err) => {
        console.log(err);
      })

      //
      // axios({
      //   url: CLOUDINARY_URL,
      //   method: 'POST',
      //   headers:{
      //     'Content-Type': 'application/x-www-form-urlencoded'
      //   },
      //   data: formData
      // })
      // .then((result) => {
      //   let url = result.data.url
      //   Editor.insertEmbed(cursorLocation, 'image', url);
      // })
      // .catch((err) => {
      //   console.log(err);
      // })
    }
  }
}
</script>

<style>
/*.flexWrapper {
      display: flex;
}
.editorWrapper {
    flex-basis: 50%;
}
.quillWrapper {
  position: relative;
  z-index: 99999;
}
*/

#preview {
  background: #f6f6f6;
  border-left: 3px solid gray;
  padding: 2em;
  margin-left: 1em;
}
@media (min-width: 601px) {
  #preview {
    width: 47%;
  }
}
.ql-disabled {
    opacity: 0.5;
    background: rgba(153, 153, 153, 0.2);
}

</style>
