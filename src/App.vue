<template>
	<div id="app">
		<div class="wrapper">
			<h1 class="caption">My Candidates</h1>
			<div class="grid">
				<div class="grid__head">
					<div class="grid__cell grid__cell_photo"></div>
					<div class="grid__cell">Name</div>
					<div class="grid__cell">Surname</div>
					<div class="grid__cell grid__cell_occupation">Occupation</div>
					<div class="grid__cell"></div>
				</div>
				<div class="grid__row" v-for="item in evenCandidate" :key="item.id">
					<div class="grid__cell grid__cell_photo">
						<img :src="item.photoUrl" alt="candidate photo" class="grid__photo">
					</div>
					<div class="grid__cell">{{ item.firstName }}</div>
					<div class="grid__cell">{{ item.lastName }}</div>
					<div class="grid__cell grid__cell_occupation">{{ item.jobTitle }}</div>
					<div class="grid__cell">
						<button class="grid__button" @click="modalState(), modalInfoSet(item.id)">View Candidate</button>
					</div>
				</div>
			</div>
			<div class="modal" :class="{ modal_open: modalOpen }">
				<div class="modal__layout">
					<div class="modal__box">
						<img :src="this.modalInfo.photoUrl" alt="candidate photo" class="modal__photo">
						<div class="modal__info">{{ this.modalInfo.id }}</div>
						<div class="modal__info">{{ this.modalInfo.firstName }}</div>
						<div class="modal__info">{{ this.modalInfo.lastName }}</div>
						<div class="modal__info">{{ this.modalInfo.jobTitle }}</div>
						<button class="modal__close" @click="modalState()">Close</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import Axios from 'axios'

export default {
	name: 'app',
	data() {
		return {
			responseData: [],
			modalOpen: false,
			modalInfo: {
				photoUrl: "",
				id: "",
				firstName: "",
				lastName: "",
				jobTitle: ""
			}
		}
	},
	computed: {
		evenCandidate() {
			return this.responseData.filter(elem => elem.id % 2 == 0)
		}
	},
	methods: {
		modalState() {
			if (event.type == "click" || !document.querySelector(".modal__box").contains(event.target) && this.modalOpen || event.keyCode === 27 && this.modalOpen) {
				this.modalOpen ? this.modalOpen = false : this.modalOpen = true;
				document.body.style.paddingRight == window.innerWidth - document.body.clientWidth + "px" ? document.body.style.paddingRight = 0 : document.body.style.paddingRight = window.innerWidth - document.body.clientWidth + "px";
				document.body.classList.toggle("overflow-hidden");
			}
		},
		modalInfoSet(i) {
			this.modalInfo.photoUrl = this.responseData[i].photoUrl;
			this.modalInfo.id = this.responseData[i].id;
			this.modalInfo.firstName = this.responseData[i].firstName;
			this.modalInfo.lastName = this.responseData[i].lastName;
			this.modalInfo.jobTitle = this.responseData[i].jobTitle;
		}
	},
	mounted() {
		Axios
		.get('https://fakedata.dev/users/v1/get_users')
		.then((response) => {
			this.responseData = response.data;
		})
		
		window.addEventListener("keydown", this.modalState)
		window.addEventListener("mouseup", this.modalState)
	}
}
</script>

<style lang="scss">
body {
	margin: 0;
	font-family: Arial;
	overflow-x: hidden;
}

.overflow-hidden {
	overflow: hidden;
}

.wrapper {
	width: 1140px;
	margin: 0 auto;
	padding: 0 15px;
	box-sizing: border-box;
}

.caption {
	margin-bottom: 50px;
}
	
.grid {
	&__head, &__row {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: 30px 0;
	}

	&__head {
		font-weight: bold;
		font-size: 20px;
	}

	&__cell {
		width: 20%;
		padding: 0 15px;
		text-align: center;
		box-sizing: border-box;

		&_photo {
			width: 10%;
		}

		&_occupation {
			width: 25%;
		}
	}

	&__photo {
		display: block;
		width: 80px;
		margin: 0 auto;
		border-radius: 50%;
	}

	&__button {
		width: 150px;
		padding: 12px;
		color: #fff;
		background-color: #64D77B;
		border: 0;
		border-radius: 15px;
		box-sizing: border-box;
		outline: 0;
		cursor: pointer;
		transition: .25s;

		&:hover {
			background-color: #5CC371;
		}

		&:active {
			background-color: #57B76A;
		}
	}
}

.modal {
	position: fixed;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	z-index: 99;
	background-color: rgba(51,51,51,.65);
	overflow-y: auto;
	opacity: 0;
	visibility: hidden;
	transition: .25s;

	&_open {
		opacity: 1;
		visibility: visible;
	}

	&__layout {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 100vh;
		min-height: 511px;
	}

	&__box {
		width: 670px;
		padding: 28px 56px;
		background-color: #fff;
		border-radius: 8px;
		box-sizing: border-box;
		overflow: hidden;
	}

	&__photo {
		display: block;
		width: 120px;
		margin-bottom: 25px;
		border-radius: 50%;
	}

	&__info {
		margin: 10px 0;
	}

	&__close {
		display: block;
		width: 100px;
		margin-top: 25px;
		padding: 14px;
		color: #595959;
		background-color: #e9e9e9;
		border: 2px solid #595959;
		border-radius: 25px;
		outline: 0;
		cursor: pointer;
		transition: .25s;

		&:hover {
			background-color: #d9d9d9;
		}

		&:active {
			background-color: #b9b9b9;
		}
	}
}

@media (max-width: 1199px) {
	.wrapper {
		width: 962px;
	}
}

@media (max-width: 991px) {
	.wrapper {
		width: 100%;
	}

	.caption {
		text-align: center;
	}

	.grid {
		&__head {
			display: none;
		}

		&__row {
			flex-direction: column;
		}

		&__cell {
			width: 100%;
			margin: 6px 0;
		}
	}

	.modal {
		&__box {
			width: 290px;
			padding: 14px 36px;
		}

		&__photo, &__close {
			margin: 20px auto;
		}

		&__info {
			margin: 15px 0;
			text-align: center;
		}
	}
}
</style>
