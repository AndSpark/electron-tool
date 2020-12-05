<template lang="pug">
#g-imgResize(transition="scale-transition"
      origin="center center")
  v-btn(block @click="openFiles") 打开文件
  v-row
    v-col(v-for="(img,i) in imgList" :key="img.name" :cols="4")
      v-card
        v-img(:src="img.path"
        class="white--text align-end"
        gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
        height="200px")
          v-card-title(v-text="img.name")
        v-card-actions
          v-spacer
          v-btn(icon @click="infoImg(i)")
            v-icon mdi-information-outline
          v-btn(icon @click="closeImg(i)")
            v-icon mdi-close
</template>

<script>
const { remote } = require('electron')
const path = require('path')
const fs = require('fs')
export default {
	name: 'g-imgResize',
	components: {},
	props: {},
	data() {
		return {
			imgList: [],
		}
	},
	computed: {},
	methods: {
		openFiles() {
			remote.dialog.showOpenDialog(
				remote.getCurrentWindow(),
				{
					properties: ['openFile', 'multiSelections'],
				},
				files => {
					files.forEach(file => {
						let { name } = path.parse(file)
						this.imgList.push({
							name,
							path: 'file://' + file,
						})
					})
				}
			)
		},
		closeImg(i) {
			this.imgList.splice(i, 1)
		},
		infoImg(i) {},
	},
}
</script>

<style lang="scss">
#g-imgResize {
}
</style>
