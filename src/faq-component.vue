<template>
	<div>
		<div class="d-flex flex-wrap mt-4 mb-4">
			<a class="col-12 col-md-6 col-lg-3 categorie_button my-1" v-for="categorie in categories" @click="switchCategorie(categorie.id)">
				<div class="card">
					<div class="card-body text-center text-dark">
						<h1 class="categorie_titre">{{categorie.nom}}</h1>
					</div>
				</div>
			</a>
		</div>

		<div class="d-flex flex-wrap mt-4">
			<div class="col-12 col-lg-8" v-if="faqs.length > 0 && faq.length > 0">
				<div class="card">
					<div class="card-header">
						Categorie {{categorie.nom}}
					</div>
					<div class="card-body">
						<div v-for="f in faq" class="border m-4 p-4 col-12 col-lg-6 faq_button" @click="displayQuestion(f.id)">
							<a class="d-flex justify-content-between align-items-center">
								<h3>{{f.objet}}</h3>
								<i class="fas fa-arrow-right"></i>
							</a>
						</div>
					</div>
				</div>
			</div>
			<div class="col-12" v-else-if="faqs.length === 0">
				<div class="card">
					<div class="card-body">
						La FAQ est vide pour le moment. Merci de revenir plus tard.
					</div>
				</div>
			</div>
		</div>

		
		<div>
			<div class="col-0 col-lg-8 popup-overflow" v-show="display_question">

			</div>
			
			<transition name="slide">
				<div class="col-12 col-lg-4 popup-faq p-4" v-show="display_question">
					<i class="fas fa-times fa-2x faq-close-button" @click="display_question = false;"></i>

					<div class="col-12 mt-4 text-center">
						<p class="h2 mt-4">{{question.objet}}</p>

						<p class="h3 mt-4">
							{{question.question}}
						</p>

						<p class="mt-4">
							{{question.reponse}}
						</p>

						<button class="btn btn-dark" @click="display_question = false;">
							Fermer
						</button>
					</div>

				</div>
			</transition>
		</div>
	</div>
</template>

<script>
	export default{
		props: ['api_url'],
		data() {
			return {
				categories: [],
				categorie: [],
				faqs: [],
				faq: [],
				question: [],
				display_question: false,
			}
		},
		methods: {
			switchCategorie(categorie_id){
				this.categorie = this.categories.find(c => c.id === categorie_id);

				if(this.categorie){
					this.faq = this.faqs.filter(f => f.categorie_id === categorie_id);
				}
			},
			displayQuestion(faq_id){
				let question = this.faq.find(f => f.id === faq_id);
				if(question){
					this.display_question = true;
					this.question = question;
				}
			}
		},
		mounted(){
			const self = this;
            self.isLoading = true;
            axios.get(self.api_url)
            .then(function (resp) {
            	self.faqs = resp.data.faqs;

            	let categories = [];
            	self.faqs.forEach(function(faq){
            		if(!categories.some(c => Number(c.id) === Number(faq.categorie.id))){
            			categories.push(faq.categorie);
            		}
            	})

            	self.categories = categories;
            	self.isLoading = false;
            })
            .catch(function (resp) {
                return self.checkError(resp);
            });
		}
	}
</script>

<style>
.faq-close-button:hover{
	cursor: pointer;
}
.popup-faq{
	position: absolute;
    top: 0;
    right: 0;
    height: 100vh;
    background: white;
    z-index: 9;
}
.popup-overflow{
    position: absolute;
    height: 100vh;
    background-color: #808080;
    top: 0;
    z-index: 9;
    width: 100%;
    left: 0;
    opacity: 0.5;
}
.categorie_button, .faq_button{
	transition: 0.2s;
}
.categorie_button:hover, .faq_button:hover{
	cursor: pointer;
	transition: 0.2s;
	transform: scale(1.05);
	text-decoration: none!important;
}
.categorie_button:hover .card{
	border: 1px solid #005ead;
}


.slide-leave-active, .slide-enter-active {
  transition: all .2s ease;
}
.slide-enter, .slide-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>