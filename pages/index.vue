<template>
	<div class="w-full h-full relative">
		<vc-viewer
			:animation="animation"
			:camera.sync="camera"
			:fullscreenButton="true"
			@ready="ready"
		>
			<!-- nav control  -->
			<vc-navigation
				ref="navigation"
				:options="options"
			></vc-navigation>

			<!-- layer  -->
			<vc-layer-imagery>
				<vc-provider-imagery-bingmaps
					url="https://dev.virtualearth.net"
					bmKey="AgcbDCAOb9zMfquaT4Z-MdHX4AsHUNvs7xgdHefEA5myMHxZk87NTNgdLbG90IE-"
					mapStyle="AerialWithLabels"
				></vc-provider-imagery-bingmaps>
			</vc-layer-imagery>

			

			<!-- Draw graphic -->
			<vc-datasource-geojson
				v-if="showGraphics"
				ref="ds1"
				@ready="subReady"
				:show="showGraphics"
				:options="options"
				:entities="entities"
			></vc-datasource-geojson>

			<vc-entity
				:position="position1"
				:point.sync="point1"
			>
				<vc-graphics-point
					ref="point1"
					:color="color1"
					:pixelSize="8"
				></vc-graphics-point>
			</vc-entity>

			<vc-collection-primitive :show="true">
				<vc-primitive-model
					ref="model"
					:url="url"
					:modelMatrix="modelMatrix"
					:scale="10000"
					:minimumPixelSize="128"
					:maximumScale="200000"
				></vc-primitive-model>
			</vc-collection-primitive>
		</vc-viewer>

	

	</div>
</template>
<script>
import ToggleButton from "../components/ToggleButton.vue";
import "vue-cesium/lib/vc-navigation.css";
import areaVN from "~/static/AreaVN";
export default {
	components: { ToggleButton },
	data() {
		return {
			animation: true,
			camera: {
				position: {
					lng: 31.791702,
					lat: -7.092620,
					height: 1000000000000,
					
				},
				
			},
			options: {
				enableCompass: true,
				enableZoomControl: true,
				enableDistanceLegend: true,
				enableLocationBar: true,
				enableCompassOuterRing: true,
				enablePrintView: true,
				enableMyLocation: true,
				stroke: "red",
			},
			cesiumGlobal: true,

			dataAreaVN: areaVN,
			entities: [],

			// Point
			point1: null,
			color1: {},
			position1: { lng: 106.6178, lat: 10.8 },

			// Model
			modelMatrix: {},
			url: "~/static/Duck.gltf",

			// Control
			showPolygon: false,
			showGraphics: false,
			showBox: true,
		};
	},

	mounted() {

	},

	methods: {
		ready(cesiumInstance) {
			const { Cesium, viewer } = cesiumInstance;
			this.cesiumGlobal = cesiumInstance;
			// Get Cesium and viewer instances here, then execute the relevant logic code
			this.camera.position.lng = -7.092620;
			this.camera.position.lat = 31.791702;
			this.camera.position.height = 8090099;
			this.animation = true;

			this.color1 = Cesium.Color.RED;

	

		

			this.modelMatrix = Cesium.Transforms.eastNorthUpToFixedFrame(
				Cesium.Cartesian3.fromDegrees(106.6178, 10.8, 100)
			);
		},

		subReady(cesiumInstance) {
			cesiumInstance.viewer.zoomTo(cesiumInstance.cesiumObject);
		},

		

		
	},
};
</script>
<style>
.viewer {
	width: 100%;
	height: 400px;
}
</style>