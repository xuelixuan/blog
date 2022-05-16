# vue3 vue-cropper 图片裁剪插件使用

```html
<template>
	<el-dialog
		width="420px"
		v-model="dialogVisible"
		title="设置封面"
		destroy-on-close
		:close-on-click-modal="false"
		:close-on-press-escape="false"
		@closed="closeDialog"
		:append-to-body="true"
	>
		<div class="avatar-editor">
			<div class="avatar-container">
				<div v-if="!options.img">
					<el-upload
						class="upload"
						ref="upload"
						:on-change="upload"
						accept="image/png, image/jpeg, image/jpg"
						:show-file-list="false"
						:auto-upload="false"
						action=""
					>
						<el-button size="small" type="primary" ref="uploadBtn"
							>选择图片</el-button
						>
						<div class="ed-tip">
							支持jpg、png格式的图片，大小不超过5M
						</div>
					</el-upload>
				</div>
				<div v-if="options.img" class="avatar-crop">
					<VueCropper
						ref="cropper"
						:img="options.img"
						:outputSize="options.outputSize"
						:outputType="options.outputType"
						:autoCropWidth="options.autoCropWidth"
						:autoCropHeight="options.autoCropHeight"
						:info="true"
						:full="options.full"
						:canMove="options.canMove"
						:canMoveBox="options.canMoveBox"
						:original="options.original"
						:autoCrop="options.autoCrop"
						:fixed="options.fixed"
						:fixedNumber="options.fixedNumber"
						:centerBox="options.centerBox"
						:infoTrue="options.infoTrue"
						:fixedBox="options.fixedBox"
						@realTime="realTime"
					></VueCropper>
					<div class="control-btn">
						<span class="delete" @click="clearAvatar"></span>
						<span class="left-rotate" @click="rotateLeft"></span>
						<span class="right-rotate" @click="rotateRight"></span>
					</div>
				</div>
			</div>
			<div class="preview-box">
				<div class="preview-avatar">
					<img :src="previews.url" :style="previews.img" />
				</div>
				<div class="ed-tip">预览</div>
			</div>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="dialogVisible = false">取消</el-button>
				<el-button
					type="primary"
					@click="confirmChange"
					:loading="isUploading"
					>确认</el-button
				>
			</span>
		</template>
	</el-dialog>
</template>

<script>
	import 'vue-cropper/dist/index.css'
	import { VueCropper } from 'vue-cropper'
	import { getQiniuToken } from '@/http/rank/api'
	import { uploadFile } from '@/utils/qiniu'
	export default {
		name: 'dialog-avatar',
		props: ['isShow'],
		emits: ['isShow'],
		components: {
			VueCropper,
		},
		data() {
			return {
				isUploading: false,
				dialogVisible: false,
				// vueCropper组件 裁剪配置信息
				options: {
					img: '', // 原图文件base64
					autoCrop: true, // 默认生成截图框
					autoCropWidth: 120,
					autoCropHeight: 120,
					fixedBox: true, // 固定截图框大小
					fixed: true, // 截图框宽高固定比例
					fixedNumber: [1, 1], // 截图框的宽高比例
					canMoveBox: true, // 截图框可以拖动
					centerBox: true, // 截图框被限制在图片里面
					canMove: true, // 上传图片是否允许拖动
					canScale: true, // 上传图片是否允许滚轮缩放
					outputSize: 0.8, // 裁剪生成图片的质量
					outputType: 'jpeg', // 裁剪生成图片的格式
					full: false, // 是否输出原图比例的截图
					infoTrue: false, // true 为展示真实输出图片宽高 false 展示看到的截图框宽高
					original: false, // 上传图片是否按照原始比例渲染
				},
				previews: {},
			}
		},
		watch: {
			isShow: function (val) {
				this.dialogVisible = val
			},
		},
		methods: {
			closeDialog() {
				this.clearAvatar()
				this.$emit('isShow', false)
			},
			// 读取原图
			upload(file) {
				this.fileName = file.raw.name
				const isIMAGE =
					file.raw.type === 'image/jpeg' ||
					file.raw.type === 'image/png'
				const isLt5M = file.raw.size / 1024 / 1024 < 5
				if (!isIMAGE) {
					this.$message.error('请选择 jpg、png 格式的图片')
					return false
				}
				if (!isLt5M) {
					this.$message.error('图片大小不能超过 5MB')
					return false
				}
				let reader = new FileReader()
				reader.readAsDataURL(file.raw)
				reader.onload = (e) => {
					this.$nextTick(() => {
						this.options.img = e.target.result // base64
					})
				}
			},
			rotateLeft() {
				this.$refs.cropper.rotateLeft()
			},
			rotateRight() {
				this.$refs.cropper.rotateRight()
			},
			clearAvatar() {
				this.options.img = ''
				this.fileName = ''
				this.previews = {}
			},
			// 实时预览
			realTime(data) {
				this.previews = data
			},
			// 获取截图信息
			confirmChange() {
				this.isUploading = true
				// 获取截图的 blob 数据
				this.$refs.cropper.getCropBlob((data) => {
					getQiniuToken({
						fileType: 2,
						filename: this.fileName,
					}).then((res) => {
						let file = {
							data: data,
							name: res.filename,
							token: res.token,
						}
						uploadFile(file).subscribe({
							next: (result) => {},
							error: () => {
								ElMessage.error('上传失败')
							},
							complete: (e) => {
								this.$localInfo.setItem('avatarUrl', newAvatar)
								this.$api
									.updateUserInfo({
										type: 0,
										value: newAvatar,
									})
									.then(() => {
										this.isUploading = false
										ElMessage.success('更新成功')
										this.dialogVisible = false
									})
							},
						})
					})
				})
			},
		},
	}
</script>

<style>
	.el-dialog__body {
		padding: 10px 20px;
	}
</style>
<style lang="scss" scoped>
	.el-dialog {
		width: 420px;
		height: 384px;

		.avatar-editor {
			width: 380px;
			height: 240px;
			display: flex;

			.avatar-container {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 240px;
				height: 240px;
				background-color: #f0f2f5;
				margin-right: 20px;
				border-radius: 4px;

				.upload {
					text-align: center;

					.ed-tip {
						font-size: 10px;
						margin-top: 10px;
					}
				}

				.avatar-crop {
					width: 240px;
					height: 240px;
					position: relative;

					.crop-box {
						width: 100%;
						height: 100%;
						border-radius: 4px;
						overflow: hidden;
					}

					.control-btn {
						width: 100%;
						height: 24px;
						text-align: center;
						position: absolute;
						left: 0;
						bottom: 5px;

						span {
							display: inline-block;
							width: 24px;
							height: 24px;
							cursor: pointer;
							margin: 0 5px;
						}
					}
				}
			}

			.preview-box {
				width: 120px;
				height: auto;

				.preview-avatar {
					width: 120px;
					height: 120px;
					overflow: hidden;
					/* border-radius: 60px; */
					background: #f0f2f5;
				}

				.ed-tip {
					width: 100%;
					height: 36px;
					line-height: 36px;
					font-size: 12px;
					text-align: center;
				}
			}
		}
	}
</style>
```
