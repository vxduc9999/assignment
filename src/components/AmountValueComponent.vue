<template>
	<div class="first-content">
		<div class="first-content__header">
			<div class="first-content__header__first">
				<button
					:disabled="isActive"
					:class="{
						'button first-content__header__first-min-button': true,
						disabled: isActive,
					}"
					@click="setMin"
				>
					MIN
				</button>
				<button
					:disabled="isActive"
					:class="{
						'button first-content__header__first-max-button': true,
						disabled: isActive,
					}"
					@click="setMax"
				>
					MAX
				</button>
			</div>

			<div class="first-content__header___second">
				<label for="value">¥</label>
				<input
					:disabled="isActive"
					:class="{ error: hasError }"
					v-model.number="inputValue"
					@keyup="validationInputValue"
				/>
			</div>

			<div class="first-content__header__third">
				<button
					:disabled="isActive"
					@click="setHalf"
					:class="{
						'button first-content__header__first-division-button': true,
						disabled: isActive,
					}"
				>
					1/2
				</button>
				<button
					:disabled="isActive"
					@click="setDouble"
					:class="{
						'button first-content__header__first-multiplication-button': true,
						disabled: isActive,
					}"
				>
					2X
				</button>
			</div>
		</div>

		<div class="chips">
			<div v-for="chip in chips" :key="chip.id" class="first-content__footer">
				<template v-if="chip.id !== 6">
					<button
						:disabled="isActive"
						@click="clickChip(chip.id)"
						:class="{
							disabled: isActive,
							'button plus-button': true,
						}"
					>
						+ {{ chip.label }}
					</button>
				</template>

				<template v-if="chip.id === 6 && isMobile">
					<button
						:disabled="isActive"
						@click="clickChip(chip.id)"
						:class="{
							disabled: isActive,
							'button plus-button': true,
						}"
					>
						+ {{ chip.label }}
					</button>
				</template>
			</div>
		</div>
	</div>
</template>

<script>
import _ from "lodash";

export default {
	name: "HomeComponent",
	props: {
		isActive: {
			type: Boolean,
			default: false,
		},
		isMobile: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		return {
			inputValue: 10,
			hasError: false,
			chips: [
				{
					id: 1,
					label: "5",
					value: 5,
				},
				{
					id: 2,
					label: "10",
					value: 10,
				},
				{
					id: 3,
					label: "50",
					value: 50,
				},
				{
					id: 4,
					label: "100",
					value: 100,
				},
				{
					id: 5,
					label: "500",
					value: 500,
				},
				{
					id: 6,
					label: "1K",
					value: 1000,
				},
			],
		};
	},
	methods: {
		validationInputValue: _.debounce(function () {
			if (this.inputValue !== parseInt(this.inputValue, 10)) {
				this.hasError = true;
				console.log("data is not an integer");
				return;
			}

			if (this.inputValue < 10 || this.inputValue > 3000) {
				this.hasError = true;
				console.log("amount input range from 10 to 3000");
				return;
			}

			this.hasError = false;
		}, 300),

		setMin: function () {
			this.inputValue = 10;
		},

		setMax: function () {
			this.inputValue = 3000;
		},

		setHalf: function () {
			if (parseInt(this.inputValue / 2) < 10) {
				console.log("amount input range from 10 to 3000");
				return;
			}

			this.inputValue = parseInt(this.inputValue / 2);
		},

		setDouble: function () {
			if (this.inputValue * 2 > 3000) {
				console.log("amount input range from 10 to 3000");
				return;
			}

			this.inputValue = this.inputValue * 2;
		},

		clickChip: function (chipId) {
			if (!chipId) {
				return;
			}

			const chipValue = this.chips.find((chip) => chip.id === chipId)?.value;
			if (this.inputValue + chipValue > 3000) {
				console.log("amount input range from 10 to 3000");
				return;
			}

			this.inputValue = this.inputValue + chipValue;
		},
	},
};
</script>

<style lang="scss" scoped>
.first-content {
	height: 100%;
	background: white;
	border: solid #1d374e 1px;
	background: #182334;
	border-radius: 10px;

	&__header {
		display: flex;
		padding: 10px;
		height: 75px;
		justify-content: space-between;
		border-bottom: solid #1d374e 1px;

		&__first {
			display: flex;
			flex-direction: column;

			&-min-button {
				background-color: #1d374e;
				color: #b8d0ed;
				margin-bottom: 5px;
			}

			&-max-button {
				background-color: #1d374e;
				color: #b8d0ed;
			}

			&-division-button {
				background-color: #1d374e;
				color: #b8d0ed;
				margin-bottom: 5px;
			}
		}

		&___second {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 0 20px;
			width: 100%;
			color: white;
			font-weight: bold;
			font-size: 18px;

			& > input {
				color: white;
				background: transparent;
				border: none;
				font-family: "Oxanium", cursive;
				font-weight: bold;
				width: 75%;
				font-size: 18px;

				&:focus {
					outline: unset;
				}
			}
		}

		&__third {
			display: flex;
			flex-direction: column;
		}
	}

	&__footer {
		.plus-button {
			color: #1cbedd;

			@media (max-width: 576px) {
				width: 55px;
			}
		}

		.plus-button:active {
			background: linear-gradient(284deg, #1cbedd 87%, #2183d6 11%);
			color: white;
		}
	}

	.disabled {
		background: transparent;
		border: solid #1d374e 1px;
		color: #2d4264;
	}
}

.chips {
	display: flex;
	justify-content: space-between;
	height: 40px;
	padding: 10px;
	gap: 10px;

	@media (max-width: 576px) {
		gap: 5px;
	}
}

.button {
	background-color: #1d374e;
	color: #b8d0ed;
	border-style: none;
	border-radius: 5px;
	width: 60px;
	height: 35px;
	font-weight: bold;

	@media (max-width: 576px) {
		width: 50px;
	}
}
</style>
