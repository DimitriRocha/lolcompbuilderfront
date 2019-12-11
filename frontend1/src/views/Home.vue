<template>
	<div class="home">
		<h2>LEAGUE COMP BUILDER</h2>
		<div class="container my-2">
			Selecione a lane
			<select v-model="selectedRole" class="form-control" id="exampleFormControlSelect1">
				<option value="top">Top</option>
				<option value="jng">Jungle</option>
				<option value="mid">Mid</option>
				<option value="adc">Adc</option>
				<option value="sup">Supp</option>
			</select>
		</div>
		<div class="container selectedChampsContainer p-2">
			<span v-on:click="removeSelectedChamp(champ)" v-for="champ in selectedChamps" class="selectedChamps">
				<img v-if="champ" class="m-1" v-bind:src="'/images/' + champ.image.full"/>
				<img v-else class="iconDefaultSize m-1" v-bind:src="'/images/NoSelected.png'"/>
			</span>
		</div>
		<br>
		<div class="container">
			<span v-on:click="selectChampion(champ)" v-for="champ in champs" class="champImg">
				<img class="m-1" v-bind:src="'/images/' + champ.image.full"/>
			</span>
		</div>
	</div>
</template>

<script>
export default {
	name: 'home',
	data(){
		return {
			selectedRole: 'top',
			selectedChamps: [],
			champs: []
		}
	},
	mounted(){
		let context = this;

		this.axios.get('http://ddragon.leagueoflegends.com/cdn/9.23.1/data/en_US/champion.json')
		.then(function(response){
			console.log(response.data);
			context.champs = response.data.data;
		});

		this.selectedChamps = {
			top: null,
			jng: null,
			mid: null,
			adc: null,
			sup: null,
		};
	},
	methods:{
		selectChampion(champion){
			this.selectedChamps[this.selectedRole] = champion;
		},
		removeSelectedChamp(champion){
			let index = this.selectedChamps.indexOf(champion);
			if (index > -1) {
				this.selectedChamps.splice(index, 1);
			}
		}
	}
}
</script>

<style scoped>
.champImg{
	cursor: pointer;
}

.selectedChampsContainer{
	min-height: 100px;
	border: 1px solid black;
}

.iconDefaultSize{
	height: 120px;
	width: 120px;
	-webkit-filter: grayscale(100%); 
	filter: grayscale(100%);
}
</style>
