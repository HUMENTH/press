<template>
	<Dialog
		v-model="showDialog"
		:options="{
			title: 'Add Region',
			actions: [
				{
					label: 'Add Region',
					variant: 'solid',
					loading: groupDocResource.addRegion.loading,
					disabled: !selectedRegion,
					onClick: () =>
						groupDocResource.addRegion.submit({
							region: selectedRegion
						})
				}
			]
		}"
	>
		<template #body-content>
			<div class="space-y-4">
				<RichSelect
					:value="selectedRegion"
					@change="selectedRegion = $event"
					:options="regionOptions"
				/>
				<ErrorMessage :message="groupDocResource.addRegion.error" />
			</div>
		</template>
	</Dialog>
</template>

<script>
import { getCachedDocumentResource } from 'frappe-ui';
import RichSelect from '@/components/RichSelect.vue';

export default {
	name: 'AddRegionDialog',
	props: ['group'],
	components: { RichSelect },
	data() {
		return {
			showDialog: true,
			selectedRegion: null,
			groupDocResource: getCachedDocumentResource('Release Group', this.group)
		};
	},
	computed: {
		regionOptions() {
			return this.$resources.availableRegions.data.map(r => ({
				label: r.title || r.name,
				value: r.name,
				image: r.image,
				beta: r.beta
			}));
		}
	},
	resources: {
		availableRegions() {
			return {
				url: 'press.api.bench.available_regions',
				params: {
					name: this.group
				},
				auto: true,
				initialData: []
			};
		}
	}
};
</script>
