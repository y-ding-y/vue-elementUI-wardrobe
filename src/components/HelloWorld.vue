<template>
	<div>
		<el-container>
			<el-aside width="200px">
				<div style="margin-top: 10px;">
					<el-button @click="handleOk" type="primary"> 下载</el-button>
				</div>
				<div style="margin-top: 10px;">
					<div style="width: 100%;height: 100px;"> 
						<el-form label-width="80px" label-position="top">
							<el-form-item label="顶部姓名：">
								<el-input placeholder="输入ID" v-model="header_name">
								</el-input>
							</el-form-item>
						</el-form>
						<el-form label-width="80px" label-position="top">
							<el-form-item label="顶部信息：">
								<el-input placeholder="随便输点什么" v-model="header_info">
								</el-input>
							</el-form-item>
						</el-form>
						<el-form label-width="80px" label-position="top">
							<el-form-item label="底部信息：">
								<el-input type="textarea" placeholder="用','英文逗号分隔" v-model="bottom_info_str">
								</el-input>
							</el-form-item>
						</el-form>
					</div>
				</div>

			</el-aside>

			<div class="char" id="char" ref="screen" >
				<div class="top">
					<div class="header_css">
						<div style="height: 100px;width: 100px;margin-top: 10px;">
							<el-avatar shape="square" :size="80" fit="'fill'" :src="require('d:\\wardrobe/sa.jpeg')">
							</el-avatar>
						</div>
						<div style="border: 5px solid rgba(248, 248, 248, 0.712); height: 50px;width:50%;margin-top: 20px;background-color: rgba(43, 174, 226, 0.301);   border-radius: 5px;
							box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);">
							<div class="header_name">
								{{header_name}}
							</div>
							<div class="header_info">
								{{header_info}}
							</div>
						</div>
					</div>
					<div class="divide">
						<div class="divide_span">衣服</div>
					</div>
					<div class="cloth">
						<div class="cloth_1" v-for="(item,index) in cloth_arr" :key="index">
							<img :src="require('d:\\wardrobe/cloth/'+item)">
						</div>
					</div>
					<div class="divide">
						<div class="divide_span">发型</div>
					</div>
					<div class="hair">
						<div class="hair_1" v-for="(item,index) in hair_arr" :key="index">
							<img :src="require('d:\\wardrobe/hair/'+item)">
						</div>
					</div>
					<div class="divide">
						<div class="divide_span">脸型</div>
					</div>
					<div class="face">
						<div class="face_1" v-for="(item,index) in face_arr" :key="index">
							<img :src="require('d:\\wardrobe/face/'+item)">
						</div>
					</div>
					<div style="height: 20px;width: 100%;">

					</div>
					<div class="bottom_css" v-if="bottom_info_str!=''">
						<div style="padding: 10px; width: 80%;  display: flex;justify-content: left;flex-wrap: wrap;">
							<div class="bottom_info" v-for="(item,index) in bottom_info" :key="index">
								{{item.info}}
							</div>

						</div>

					</div>
				</div>

				<!-- <img style="width: 50%;" src="/static/bg/bg.png"> -->
			</div>

		</el-container>




	</div>
</template>

<script>
	import html2canvas from 'html2canvas'
	import './index.css'

	export default {
		data() {
			return {
				drawer: false,
				direction: 'rtl',

				cloth_arr: [],
				hair_arr: [],
				face_arr: [],
				 
				
				header_name: "",
				header_info: "",
				bottom_info_str: "",
				charcss:"100%",
			};
		},
		watch: {
			bottom_info_str(){
				//this.$refs.screen.style.background = "url('/static/sa.jpeg') 100% 100% no-repeat"
				
			},
		},
		computed: {
			// charcss() {
			// 	if (this.$refs.screen)
			// 		console.log(this.$refs.screen.offsetHeight)
			// 	return this.$refs.screen ? this.$refs.screen.offsetHeight : "100%"
			// },
			bottom_info() {
				var temp = [];
				this.bottom_info_str.split(',').map((e, i) => {
					temp.push({ key: i, info: e, })
				})
				return temp;
			},
		},
		created() { 
			var arr = [];
			var requireModule = require.context(
				"d:\\wardrobe/cloth",
				false,
				///\.jpeg$/
			);
			for (var i = 0; i < requireModule.keys().length; i++) {
				this.cloth_arr.push(
					requireModule.keys()[i].substr(2, requireModule.keys()[i].length)
				);
			} 
			var requireModules = require.context(
				"d:\\wardrobe/hair",
				false,
				///\.jpeg$/
			);
			for (var i = 0; i < requireModules.keys().length; i++) {
				this.hair_arr.push(
					requireModules.keys()[i].substr(2, requireModules.keys()[i].length)
				);
			}

			var requireModules2 = require.context(
				"d:\\wardrobe/face",
				false,
				///\.jpeg$/
			);
			for (var i = 0; i < requireModules2.keys().length; i++) {
				this.face_arr.push(
					requireModules2.keys()[i].substr(2, requireModules2.keys()[i].length)
				);
			}
			console.log(this.cloth_arr)
		},
		methods: {
			handleOk() {
				html2canvas(this.$refs.screen, {
					useCORS: true,
				}).then((canvas) => {
					if (navigator.msSaveBlob) {
						// IE10+
						let blob = canvas.msToBlob()
						return navigator.msSaveBlob(blob, name)
					} else {
						let imageurl = canvas.toDataURL('image/png')
						//这里需要自己选择命名规则
						let imagename = ''
						this.fileDownload(imageurl, imagename)
					}
				})
			},
			//下载截屏图片
			fileDownload(downloadUrl, downloadName) {
				let aLink = document.createElement('a')
				aLink.style.display = 'none'
				aLink.href = downloadUrl
				aLink.download = `${downloadName}.jpg`
				// 触发点击-然后移除
				document.body.appendChild(aLink)
				aLink.click()
				document.body.removeChild(aLink)
			},

		}
	}
</script>

<style>
	.el-header,
	.el-footer {
		background-color: #B3C0D1;
		color: #333;
		text-align: center;
		line-height: 60px;
	}

	.el-aside {
		background-color: #D3DCE6;
		color: #333;
	}

	.el-main {
		background-color: #E9EEF3;
		color: #333;
		line-height: 160px;
	}

	body>.el-container {
		margin-bottom: 40px;
	}

	.el-container:nth-child(5) .el-aside,
	.el-container:nth-child(6) .el-aside {
		line-height: 260px;
	}

	.el-container:nth-child(7) .el-aside {
		line-height: 320px;
	}
</style>