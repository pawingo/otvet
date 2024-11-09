<script>
import axios from 'axios';

export default {
	data() {
		return {
			answers: [],
			aearchQuery: "",
			searchResults: [],
			isLoading: false,
			title: "",
			answer: ""
		};
	},

	mounted() {
		this.loadAnswers();
	},

	methods: {
		async loadAnswers() {
			this.isLoading = true;
			try {
				let response = await axios.get('http://localhost:3005/answers');
				this.answers = response.data;
				this.searchResults = this.answers;
			} catch (error) {
				console.error('Ошибка поиска:', error);
			} finally {
				this.isLoading = false;
			}
		},

		searchAnswers() {
			this.searchResults = this.answers.filter(answer => {
				return answer.question.toLowerCase().includes(this.searchQuery.toLowerCase());
			});
		},
	},
};
</script>


<template>
	<div class="body">
		<div class="input-group mb-3" style="width: 50%; margin: auto;">
			<span class="input-group-text" id="inputGroup-sizing-default">Поиск по названию</span>
			<input type="text" class="form-control" aria-label="Sizing example input"
				aria-describedby="inputGroup-sizing-default" v-model="searchQuery" @input="searchAnswers"
				style="width: 60px;">
		</div>
		<div v-if="isLoading" class="text-center">
			<div class="spinner-border text-primary" role="status">
				<span class="visually-hidden">Загрузка...</span>
			</div>
		</div>
		<div v-else-if="searchResults.length === 0 && !isLoading">
			<p class="text-center">Таких вопросов не найдено.</p>
		</div>
		<div class="container" style="margin-top: 70px;">
			<div class="row">
				<div class="col-md-4" style="margin-bottom: 20px;" v-for="card in searchResults">
					<div class="card" style="width: 100%; padding: 7px; height: 400px; display: flex; overflow-y: auto">
						<div class="card-body">
							<h5 class="card-title">Вопрос:</h5>
							<p>{{ card.question }}</p>
							<h5>Ответ: </h5>
							<div class="alert alert-success" role="alert">
								{{ card.answer }}
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>


<style>
.body {
	width: 75%;
	margin: auto;
	margin-top: 30px;
}

.container {
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
}

.card::-webkit-scrollbar {
	width: 7px;
	background-color: #f9f9fd;
}

.card::-webkit-scrollbar-thumb {
	border-radius: 10px;
	background-color: #18aaaa;
}

.card::-webkit-scrollbar-track {
	-webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.2);
	border-radius: 10px;
	background-color: #c5eeee;
}

textarea::-webkit-scrollbar {
	width: 7px;
	background-color: #f9f9fd;
}

textarea::-webkit-scrollbar-thumb {
	border-radius: 10px;
	background-color: #18aaaa;
}

textarea::-webkit-scrollbar-track {
	-webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.2);
	border-radius: 10px;
	background-color: #c5eeee;
}
</style>