
<template>
  <div class="trainers">
  	<!-- WRAPPER -->
  	<div id="wrapper">

  		<!-- PAGE TITLE -->
  		<section class="container">
  	<h2>Find your <strong>guide</strong></h2>
  			<p class="lead">Local trainers dedicated to helping you do your best in any area you want to pursue!</p> 


					<!-- search form -->
				<div>
					<input type="text" class="form-control" name="k" id="k" v-model="searchText" list="tags" placeholder="Search by Tags">
				</div>
					<!-- /search form -->

  			<div class="row">
  				<ul class="sort-destination isotope" data-sort-id="isotope-list" >
  					<li class="isotope-item col-md-3" v-for="trainer in orderBy(filterBy(trainers, searchText), sortAttribute, sortAscending)" v-bind:key="trainer.id">
  						<div class="item-box">
  							<figure>
  								<router-link v-bind:to="'/trainers/' + trainer.id"><img :src="(trainer.image_url)" width="263" height="395" alt=""></router-link>
  							</figure>
  							<div class="item-box-desc">
  								<h4>{{trainer.first_name}} {{trainer.last_name}}</h4>
  								<small>  {{trainer.location}}</small>
<!--   								<p>{{trainer.bio}}</p> -->
  								<hr />
  								<div>
  									<span v-for="tag in trainer.tags" id="tags" class="label label-primary" v-on:click="searchText = tag.name">{{tag.name}}</span>
  								</div>
  							</div>
  						</div>
  					</li>
  				</ul>
  			</div><!-- /.masonry-container -->

  			<!-- CALLOUT -->
  			<div class="bs-callout text-center nomargin-bottom">
  				<h3>Have the drive to train? Be part of <strong>Sparq Team</strong>!<a href="#/trainerssignup" target="_blank" class="btn btn-primary btn-lg">Ok, let's do it!</a></h3>
  			</div>
  			<!-- /CALLOUT -->
  		</section>

  	</div>
  	<!-- /WRAPPER -->
 	 	<datalist id="tags">
 			<option v-for="tag in tags">{{ tag.name }}</option>
 	 	</datalist>
	</div>
</template>

<style>
body {
	margin-top: 20px;
}
img.img-responsive {
/*	height: 350px;*/
}
	#tags {
		padding: 5px;
	}
	hr {
		margin-top: 0px;
		margin-bottom: 10px;
	}
	ul.isotope {
		opacity: 1 !important; 
	}
	input#k {
		width: 23%;
	}

</style>

<script>
var axios = require("axios");
export default {
	data: function() {
		return {
			trainers: [],
			tags: [],
			searchText: "",
			sortAttribute: "name",
			sortAscending: 1,
			errors: []
		};
	},
	created: function() {
		axios.get("http://localhost:3000/api/trainers").then(response => {
			console.log(response.data);
			this.trainers = response.data;
		});
		axios.get("http://localhost:3000/api/tags/").then(response => {
		  this.tags = response.data.map(a => {
		    return { name: a.name, id: a.id };
		  });
		});
	},
	methods: {
  setCurrentTrainer: function(trainer) {
    this.currentTrainer = Trainer;
	},
	setSortAttribute: function(attribute) {
		if(this.sortAttribute === attribute) {
			this.sortAscending = this.sortAscending * -1;
		} else {
			this.sortAscending = 1;
		}
		this.sortAttribute = attribute;
	}
},
	computed: {}
};
</script>

