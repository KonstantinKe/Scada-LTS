<template>
	<div class="panel-body">
		<div>
			<input
				type="text"
				name="url"
				id="url"
				placeholder="example.com"
				pattern=".*"
				size="30"
				required
				v-model="url"
			/>
		</div>
		<div>
			<btn v-if="enable_save" size="xs" type="success" v-on:click="save()">Save</btn>
			<btn v-if="!enable_save" id="dis_btn_sms_dom" size="xs" type="light">Save</btn>
			<tooltip
				v-if="!enable_save"
				text="I have nothing to write down"
				target="#dis_btn_sms_dom"
			/>
		</div>
	</div>
</template>

<script>
import axios from 'axios';

/**
 * @author grzegorz.bylica@gmail.com
 *
 */

export default {
	data() {
		return {
			url: '',
			url_save: '',
			enable_save: false,
		};
	},
	methods: {
		save() {
			const parts_of_domain = this.url.split('.');
			const api = `./api/systemSettings/saveSMSDomain/${parts_of_domain}`;
			axios
				.post(api)
				.then((response) => {
					this.url_save = this.url;
				})
				.catch((error) => {
					console.error(error);
				});
		},
		load() {
			const api = `./api/systemSettings/getSMSDomain`;
			axios
				.get(api)
				.then((response) => {
					this.url = response.data;
					this.url_save = response.data;
				})
				.catch((error) => {
					console.error(error);
				});
		},
	},
	mounted() {
		this.load();
	},
	watch: {
		url: function (val) {
			if (val != this.url_save) {
				this.enable_save = true;
			} else {
				this.enable_save = false;
			}
		},
		url_save: function (val) {
			if (val != this.url) {
				this.enable_save = true;
			} else {
				this.enable_save = false;
			}
		},
	},
};
</script>

<style scoped>
.number-width {
	width: 70px;
}
.move-top {
	top: -170px;
}
.format_font {
	font-size: 12px;
}
</style>
