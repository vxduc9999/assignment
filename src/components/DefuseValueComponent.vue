<template>
	<div class="second-content">
		<div class="second-content__first">
			<p class="second-content__first-title">DEFUSE AT</p>

			<div class="second-content__first-input">
				<button
					:disabled="isActive"
					@click="decreaseButton"
					:class="{ 'button decrease-button': true, disabled: isActive }"
				>
					-
				</button>
				<input
					v-model.number="defuseValue"
					:disabled="isActive"
					@keyup="validationDefuseValue"
					:class="{ error: hasDefuseError }"
				/>
				<button
					:disabled="isActive"
					@click="increaseButton"
					:class="{ 'button increasse-button': true, disabled: isActive }"
				>
					+
				</button>
			</div>
		</div>

		<!-- if desktop will show it -->
		<div v-if="!isMobile" class="chips">
			<div v-for="chip in defuseChips" :key="chip.id">
				<button
					:disabled="isActive"
					@click="clickDefuseChip(chip.id)"
					:class="{ 'button second-button': true, disabled: isActive }"
				>
					{{ chip.label }}x
				</button>
			</div>
		</div>
	</div>
</template>

<script>
import _ from "lodash";

export default {
	name: "AmountValueComponent",
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
			defuseValue: 1.01,
			minDefuseValue: 1.01,
			maxDefuseValue: 100,
			hasDefuseError: false,
			defuseChips: [
				{
					id: 1,
					label: "1.5",
					value: 1.5,
				},
				{
					id: 2,
					label: "2.0",
					value: 2,
				},
				{
					id: 3,
					label: "5",
					value: 5,
				},
			],
		};
	},
	methods: {
		checkValidation: function (input, min, max) {
			//check is not number
			if (isNaN(input)) {
				console.log("data is not an number");

				return true;
			}

			// out of range
			if (input < min || this.defuseValue > max) {
				console.log("amount input range from 10 to 3000");
				return true;
			}

			return false;
		},

		validationDefuseValue: _.debounce(function () {
			if (
				this.checkValidation(
					this.defuseValue,
					this.minDefuseValue,
					this.maxDefuseValue
				)
			) {
				this.hasDefuseError = true;
				return;
			}

			this.defuseValue = parseFloat(Number(this.defuseValue).toFixed(2));
			this.hasDefuseError = false;
		}, 300),

		decreaseButton: function () {
			if (
				this.checkValidation(
					this.defuseValue,
					this.minDefuseValue,
					this.maxDefuseValue
				) ||
				this.defuseValue <= this.minDefuseValue
			) {
				return;
			}

			const tempDefuseValue = parseFloat(Number(this.defuseValue).toFixed(2));

			if (tempDefuseValue < 2) {
				this.defuseValue =
					Number(tempDefuseValue - 0.1).toFixed(2) >= this.minDefuseValue
						? Number(tempDefuseValue - 0.1).toFixed(2)
						: tempDefuseValue;

				return;
			}

			this.defuseValue = Number(tempDefuseValue - 1).toFixed(2);
		},

		increaseButton: function () {
			if (
				this.checkValidation(
					this.defuseValue,
					this.minDefuseValue,
					this.maxDefuseValue
				) ||
				this.defuseValue >= this.maxDefuseValue
			) {
				return;
			}

			const tempDefuseValue = parseFloat(Number(this.defuseValue).toFixed(2));

			if (tempDefuseValue < 2) {
				this.defuseValue = Number(tempDefuseValue + 0.1).toFixed(2);

				return;
			}

			this.defuseValue =
				Number(tempDefuseValue + 1).toFixed(2) < this.maxDefuseValue
					? Number(tempDefuseValue + 1).toFixed(2)
					: tempDefuseValue;
		},

		clickDefuseChip: function (chipId) {
			if (!chipId) {
				return;
			}

			const chipValue = this.defuseChips.find(
				(chip) => chip.id === chipId
			)?.value;

			const tempDefuseValue = parseFloat(
				Number(this.defuseValue * chipValue).toFixed(2)
			);

			if (tempDefuseValue > this.maxDefuseValue) {
				console.log("amount input range from 1.01 to 100");
				return;
			}

			this.defuseValue = tempDefuseValue;
		},
	},
};
</script>

<style lang="scss" scoped>
.second-content {
	height: 100%;
	background: white;
	border: solid #1d374e 1px;
	background: #182334;
	border-radius: 10px;

	&__first {
		padding: 10px;
		height: 75px;
		border-bottom: solid #1d374e 1px;

		@media (max-width: 576px) {
			height: 50px;
			display: flex;
			justify-content: space-between;
			align-items: center;
		}

		&-title {
			display: flex;
			align-items: center;
			justify-content: center;
			color: #2d4264;
			font-weight: bold;
			margin: 0;
			height: 30px;
			margin-bottom: 5px;
		}

		&-input {
			& > input {
				font-size: 18px;
				background: transparent;
				border: none;
				font-family: "Oxanium", cursive;
				font-weight: bold;
				width: 80px;
				color: white;
				font-size: 22px;
				margin: 0 10px;
				text-align: center;

				&:focus {
					outline: unset;
				}
			}
		}
	}

	.button {
		background-color: #1d374e;
		color: #b8d0ed;
		border-style: none;
		border-radius: 5px;
		width: 40px;
		height: 40px;
		font-weight: bold;
	}

	.chips {
		display: flex;
		justify-content: space-between;
		height: 40px;
		padding: 10px;
		gap: 5px;
	}

	.second-button {
		width: 60px;
		color: #1cbedd;
		height: 35px;
	}

	.disabled {
		background: transparent;
		border: solid #1d374e 1px;
		color: #2d4264;
	}
}
</style>
