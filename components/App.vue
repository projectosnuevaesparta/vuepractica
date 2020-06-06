<template>
	<div>
		<h1>LISTADO DE ARTISTAS</h1>
		<div class="container">
			<select v-model="select">
				<option v-for="c in countries" :value="c.value"> {{c.name}}</option>
			</select>
			<transition name="fade" v-if="loading">
				<spinner></spinner>
			</transition>
			<transition name="fade" v-if="artiss">
				<div class="row">
					<div class="col-md-3" v-for="a in artist" :key="a.mbid">
						<div class="card">
							<div class="card-header">
								{{a.name}}
							</div>
							<div class="card-body">
								<img :src="a.image[2]['#text']">
							</div>
							<div class="card-footer"></div>
						</div>
					<hr>
					</div>
				</div>
			</transition>
		</div>

		
		
	</div>



</template>

<script>
	import config from '../config.js';
	import spinner from './spinnerComponent';
	export default{
		name: 'App',
		data(){
			return{
				mensaje: 'hola',
				artist: [],
				countries:[
					{name: 'España', value: 'spain'},
					{name: 'Colombia', value: 'colombia'},
					{name: 'Argentina', value: 'argentina'},
				],
				select: 'argentina',
				loading: false
				artiss = true
			}
		},
		components:{
			spinner: spinner
		},
		
		mounted(){
			this.getArtis();
		},
		methods:{
			getArtis(){
				this.loading = true
				this.artiss = false
				const key = config.apiKey;
				const url = 'http://ws.audioscrobbler.com/2.0/?method=geo.gettopartists&country='+this.select+'&api_key='+key+'&format=json';
				//const url = url.replace('select', select)
				axios.get(url)
					.then(response => {
						this.artiss = false
						this.artist = response.data.topartists.artist
						this.loading = false
					})
			}
		},
		watch: {
			select(){
				this.getArtis();
			}
		}
	}

</script>
<style scoped>
.col-md-3{
	width: 30%;
	margin-right: 1%;
	border: 2px solid #2a2a2a;
	display: inline-block;
	float: left;
	margin-bottom: 10px;
}

</style>