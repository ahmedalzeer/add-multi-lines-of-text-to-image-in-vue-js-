<template>
  <div class="editor-container">
    <!-- Left Panel: Text Settings -->
    <div class="left-panel">
      <h2>قائمة الاعدادات</h2>
      <div class="setting-group">
        <label>رفع صورة:</label>
        <input type="file" @change="handleImageUpload">
      </div>

      <div v-for="(textLine, index) in textLines" :key="index" >
        <div class="setting-group form-group">
          <div class="setting-group row align-items-center">
            <label class="col-8">النص رقم {{index + 1}}</label>
            <div class="col-4 text-right row">
              <i @click="removeTextLine(index)" class="col-2  fa fa-trash" style="color:#db7878; font-size: 16px; cursor: pointer;"></i>
              <i @click="toggleTextLineVisibility(index)" class="fa" :class="{ 'col-2  fa-minus-circle': textLine.visible, 'col-2  fa-plus-circle': !textLine.visible }" style="color:#9797ee; font-size: 16px; cursor: pointer;"></i>
            </div>
          </div>
        </div>
        <div v-if="textLine.visible" class="setting-group form-group">
          <div class="setting-group row">
            <label class="col">اكتب النص :</label>
            <textarea v-model="textLine.text" rows="1" placeholder="Enter text for certificate" class="col form-control form-control-sm"></textarea>
          </div>
          <div class="setting-group row">
            <label class="col">لون النص:</label>
            <input type="color" v-model="textLine.color" @input="updatePreview" class="col form-control form-control-sm">
          </div>
          <div class="setting-group row">
            <label class="col">حجم الخط:</label>
            <input type="number" v-model="textLine.fontSize" @input="updatePreview" min="10" max="80" class="col form-control form-control-sm">
          </div>
          <div class="setting-group row">
            <label class="col">نوع الخط:</label>
            <select v-model="textLine.fontFamily" @change="updatePreview" class="col form-control form-control-sm">
              <option value="Arial">Arial</option>
              <option value="Times New Roman">Times New Roman</option>
              <option value="Verdana">Verdana</option>
            </select>
          </div>
          <div class="setting-group row">
            <label class="col">رأسيً :</label>
            <input type="number" v-model="textLine.vertical" @input="updatePreview" class="col form-control form-control-sm">
          </div>
          <div class="setting-group row">
            <label class="col">أفقيً :</label>
            <input type="number" v-model="textLine.horizontal" @input="updatePreview" class="col form-control form-control-sm">
          </div>
        </div>
      </div>

      <div class="row">
        <i @click="addTextLine" class="col-2 fa fa-plus-circle" style="color:#82ce82; font-size: 24px; cursor: pointer;"></i>
        <i @click="saveData" v-if="textLines.length" class="col-2 fa fa-save" style="color:#5164ff; font-size: 24px; cursor: pointer;"></i>
      </div>
    </div>

    <!-- Right Panel: Preview -->
    <div class="right-panel">
      <h2 class="text-center">نتيجة التصميم</h2>
      <div class="preview-container" :style="{ backgroundImage: 'url(' + backgroundImage + ')' }">
        <div v-for="(textLine, index) in textLines" :key="index" class="preview-text" :style="getTextStyle(textLine)">{{ textLine.text }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'addMultiTextToImg',
  data() {
    return {
      backgroundImage: null,
      textLines: [],
    };
  },
  methods: {
    handleImageUpload(event) {
      this.backgroundImage = URL.createObjectURL(event.target.files[0]);
      this.updatePreview();
    },
    updatePreview() {
      const previewText = document.querySelectorAll('.preview-text');
      previewText.forEach((textElement, index) => {
        const textLine = this.textLines[index];
        if (textLine) {
          textElement.style.color = textLine.color;
          textElement.style.fontSize = textLine.fontSize + 'px';
          textElement.style.fontFamily = textLine.fontFamily;
          textElement.style.top = textLine.vertical + 'px';
          textElement.style.left = textLine.horizontal + 'px';
        }
      });
    },
    getTextStyle(textLine) {
      return {
        color: textLine.color,
        fontSize: textLine.fontSize + 'px',
        fontFamily: textLine.fontFamily,
        position: 'absolute',
        top: textLine.vertical + 'px',
        left: textLine.horizontal + 'px',
        textAlign: 'center',
        width: '100%',
      };
    },
    addTextLine() {
      this.textLines.push({
        text: 'New Line Text',
        color: '#000000',
        fontSize: 24,
        fontFamily: 'Arial',
        vertical: 0,
        horizontal: 0,
        visible: true,
      });
    },
    removeTextLine(index) {
      this.textLines.splice(index, 1);
    },
    toggleTextLineVisibility(index) {
      this.textLines[index].visible = !this.textLines[index].visible;
    },
    saveData() {
      this.$emit('textLinesSaved', {'textLines':this.textLines, 'backgroundImage':this.backgroundImage});
    },
  }
};
</script>

<style scoped>
.editor-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.left-panel {
  margin-top: 50px;
  width: 200px;
  padding: 20px;
  border: 1px solid #ebeeea;
  background-color: #ebf4f0;
}

.left-panel h2 {
  margin-bottom: 20px;
}

.setting-group {
  margin-bottom: 15px;
}

.setting-group label {
  display: block;
  margin-bottom: 5px;
}

textarea,
input[type="number"],
select {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.right-panel {
  flex: 1;
  padding: 20px;
}

.preview-container {
  width: 100%;
  height: 800px;
  position: relative;
  background-color: #f0f0f0;
  background-size: cover;
  background-position: center center;
  border-radius: 5px;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.preview-text {
  word-wrap: break-word;
}
</style>

