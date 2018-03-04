<template>
	<div :style="{background:'url('+imgs.hotwordsBg+') no-repeat center bottom',backgroundSize:'cover'}"  :class="{'show':show}" class="zmiti-choose-main-ui lt-full" ref='page'>
		<div class="zmiti-text">
			<img :src="imgs.text" alt="">
		</div>
		<ul class="zmiti-choose-list">
			<transition-group name='zmiti-word-animate'  >
				<li v-for='(word,i) in chooseWords' :key='word.hotword'>
					{{word.hotword}}
					<span @click="unchoose(i,word)"></span>
				</li>
				
			</transition-group>
		</ul>
		<div class="zmiti-line">
			
		</div>

		<ul class="zmiti-all-words">
			<transition-group name='zmiti-word-animate'  >
				<li v-for='(word,i) in words' :key='word.hotword' @click='choose(i,word)' >
					{{word.hotword}}
				</li>
			</transition-group>
		</ul>

		<div v-if='chooseWords.length>0' class="zmiti-choose-btn" v-tap='entryForm'>
			<img :src="imgs.followBtn" alt="">
		</div>

		<audio src='./assets/music/1.mp3' ref='audio1'></audio>
		<audio src='./assets/music/2.mp3' ref='audio2'></audio>
	</div>
</template>
<script>
	import './index.css';
	import Vue from "vue";
	import imgs from '../lib/assets.js';

	//Vue.use(VueAnimatedList)
	export default {
		name:'zmitipage1',
		props:['obserable'],
		data(){
			return {
				imgs,
				play:false,
				show:false,
				words:[],
				chooseWords:[]
			}
		},
		methods:{
			choose(index,word){
				this.chooseWords.push({
					index,
					hotword:word.hotword,
					hotwordid:word.hotwordid

				});
				this.$refs['audio1'].play();
				//console.log(JSON.stringify(this.chooseWords))

				this.words.splice(index,1);
			},
			unchoose(index,word){
				this.chooseWords.splice(index,1);
				this.words.splice(word.index,0,word);
				this.$refs['audio2'].play();
			},
			entryForm(){
				var {obserable} = this;
				obserable.trigger({
					type:'setShareWords',
					data:this.chooseWords.concat([])
				})
				obserable.trigger({
					type:'showFormApp'
				});


				var ids = '';
				this.chooseWords.forEach((word,i)=>{
					if(i<this.chooseWords.length-1){
						ids += word.hotwordid+','
					}
					else{
						ids += word.hotwordid;
					}
				});

				

				obserable.trigger({
					type:'setHotwordsid',
					data:ids
				})
				
			}
		},
		components:{
		},
		mounted(){
			
			var {obserable} = this;

		/*	obserable.trigger({
				type:'setPlay',
				data:{
					top:'.5rem !important'
				}
			});*/

			obserable.on('showChooseApp',(data)=>{
				this.words = data;
				
				this.show = true;

				setTimeout(()=>{
					obserable.trigger({
						type:'setPlay',
						data:{
							top:'.5rem !important'
						}
					});
				},300)
			})

			/*setTimeout(()=>{

				this.words = obserable.trigger({type:'getWords'})

			},400)*/
		}
	}
</script>