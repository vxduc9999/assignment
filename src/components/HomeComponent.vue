<template>
	<div class="main">
		<div class="main-content">
			<!-- the first content -->
			<AmountValueComponent :isActive="isActive" :isMobile="isMobile" />

			<!-- the second content -->
			<DefuseValueComponent :isActive="isActive" :isMobile="isMobile" />

			<!-- The button -->
			<button
				:disabled="isActive"
				@click="betButton"
				:class="{
					disabled: isActive,
					'button bet-button': true,
				}"
			>
				<p>{{ messageBetButton }}</p>
			</button>
		</div>
	</div>
</template>

<script>
import AmountValueComponent from "./AmountValueComponent.vue";
import DefuseValueComponent from "./DefuseValueComponent.vue";

export default {
	name: "HomeComponent",
	components: {
		AmountValueComponent,
		DefuseValueComponent,
	},
	data() {
		return {
			isActive: false,
			isMobile: false,
			messageBetButton: "BET",
		};
	},
	mounted() {
		this.checkIsMobile();
		window.addEventListener("resize", () => {
			this.checkIsMobile();
		});
	},
	methods: {
		betButton: function () {
			this.isActive = true;
			this.messageBetButton = "BET PLACED";
		},
		checkIsMobile: function () {
			this.isMobile = window.innerWidth <= 576;
		},
	},
};
</script>

<style lang="scss" scoped>
.main {
	display: flex;
	justify-content: center;

	&-content {
		display: flex;
		background: #182334;
		padding: 10px;
		gap: 10px;

		@media (min-width: 1280px) {
			max-width: 1000px;
		}

		@media (max-width: 576px) {
			flex-direction: column;
			padding: 20px;
		}

		.button {
			width: 200px;
			border-radius: 10px;
			font-weight: bold;
			font-size: 22px;

			& > p {
				width: 120px;
				margin: auto;
			}

			@media (max-width: 576px) {
				width: 100%;

				& > p {
					display: flex;
					justify-content: center;
					align-items: center;
					height: 70px;
				}
			}
		}

		.bet-button {
			color: white;
			background-image: linear-gradient(284deg, #1cbedd 87%, #2183d6 11%);
		}

		.disabled {
			color: #1cbedd;
			background-image: linear-gradient(
				284deg,
				rgba(28, 190, 221, 0.2) 87%,
				rgba(33, 131, 214, 0.2) 11%
			);
		}
	}
}
</style>
