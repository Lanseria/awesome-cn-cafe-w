<template>
  <div class="h-full w-full relative">
    <div
      ref="mapContainer"
      :style="{
        height: '100%',
        width: '100%',
        top: 0,
        bottom: 0,
        left: 0,
        right: 0,
      }"
    />
  </div>
</template>
<script lang="ts" setup>
import { onMounted, ref, watch, type PropType } from "vue";

import { setCurrent, setLoc } from "@/stores/store";
import { emitter } from "@/event";

window.mapboxgl.accessToken =
  "pk.eyJ1IjoibGFuc2VyaWEiLCJhIjoiY2wxMGo5ZWk3MTF3dTNkcnRwcDMyMXowOSJ9.kxLDvTThtaU0uiBOXanNvA";

function createColorPoint(...color: number[]) {
  const d = 48;
  const r = d / 2;
  const r2 = r ** 2;
  const bytesPerPixel = 4;

  const data = new Uint8Array(d * d * bytesPerPixel);

  for (let x = 0; x < d; x++) {
    for (let y = 0; y < d; y++) {
      if ((x - r) ** 2 + (y - r) ** 2 >= r2) continue;

      const offset = (y * d + x) * bytesPerPixel;
      for (let b = 0; b < bytesPerPixel; b++) data[offset + b] = color[b];
    }
  }
  return { width: d, height: d, data };
}

const props = defineProps({
  city: {
    tyoe: String,
    required: true,
  },
  filter: {
    tyoe: String,
    required: true,
  },
  data: {
    tyoe: Object as PropType<any>,
    required: true,
  },
  geo: {
    tyoe: Object as PropType<any>,
    required: true,
  },
});
const SCALE = 10;
let map: any = null;
let geoControl: any = null;
let last = "";
let lastFilter = "";
const mapContainer = ref();
watch(
  () => props,
  (next) => {
    if (last !== next.city || lastFilter !== next.filter) {
      updateMap();
      setTimeout(() => {
        window.dispatchEvent(new Event("resize"));
      }, 300);
    }
  },
  {
    deep: true,
  }
);
onMounted(() => {
  window.map = map = new window.mapboxgl.Map({
    container: mapContainer.value,
    style: "mapbox://styles/lanseria/cl10jowpd006m15odyj4d5d9j",
    center: props.data.center,
    zoom: SCALE,
  });

  map.addControl(new window.MapboxLanguage({ defaultLanguage: "zh-Hans" }));

  map.on("load", () => {
    map.addImage("#50C240", createColorPoint(80, 194, 64, 255));
    map.addImage("#F3AE1A", createColorPoint(255, 193, 7, 255));
    map.addImage("#C24740", createColorPoint(194, 71, 64, 255));
    map.addImage("#BEBEBE", createColorPoint(125, 125, 125, 255));

    updateMap();
  });

  map.on("click", "layer", (e: any) => {
    if (!e.features) return;

    const coordinates = e.features[0].geometry.coordinates.slice();
    const properties = e.features[0].properties;

    setCurrent({
      coordinates,
      properties,
    });
  });

  map.on("click", "clusters", (e: any) => {
    const features = map.queryRenderedFeatures(e.point, {
      layers: ["clusters"],
    });
    if (!features.length) return;
    const clusterId = features[0].properties.cluster_id;
    map
      .getSource("source")
      .getClusterExpansionZoom(clusterId, (err: any, zoom: any) => {
        if (err) return;

        map.easeTo({
          center: features[0].geometry.coordinates,
          zoom,
        });
      });
  });

  map.on("mouseenter", "layer", () => {
    map.getCanvas().style.cursor = "pointer";
  });

  map.on("mouseleave", "layer", () => {
    map.getCanvas().style.cursor = "";
  });

  geoControl = new window.mapboxgl.GeolocateControl({
    positionOptions: {
      enableHighAccuracy: true,
    },
    trackUserLocation: true,
  });

  map.addControl(geoControl);

  emitter.on("track", () => {
    geoControl.trigger();
  });

  emitter.on("fly-to", (data: any) => {
    map.flyTo(data);
  });

  geoControl.on("geolocate", (e: any) => {
    setLoc([e.coords.longitude, e.coords.latitude]);
  });
});

const updateMap = () => {
  if (last !== props.city) {
    map.flyTo({
      center: props.data.center,
      zoom: SCALE,
      speed: 2,
    });
  }

  if (lastFilter !== props.filter) {
    if (map.getLayer("layer")) map.removeLayer("layer");
    if (map.getLayer("clusters")) map.removeLayer("clusters");
    if (map.getLayer("clusters-count")) map.removeLayer("clusters-count");
    if (map.getSource("source")) map.removeSource("source");

    map.addSource("source", {
      type: "geojson",
      data: props.geo,
      cluster: true,
      clusterMaxZoom: 12,
      clusterRadius: 25,
    });

    map.addLayer({
      id: "clusters",
      type: "circle",
      source: "source",
      filter: ["has", "point_count"],
      paint: {
        "circle-color": "#d3cdc0",
        "circle-stroke-color": "#a59a83",
        "circle-stroke-width": 1,
        "circle-radius": 10,
      },
    });

    map.addLayer({
      id: "clusters-count",
      type: "symbol",
      source: "source",
      filter: ["has", "point_count"],
      layout: {
        "text-field": "{point_count_abbreviated}",
        "text-size": 12,
        "text-allow-overlap": true,
      },
      paint: {
        "text-color": "white",
      },
    });

    map.addLayer({
      id: "layer",
      type: "symbol",
      source: "source",
      layout: {
        "icon-image": ["get", "marker-color"],
        "icon-size": 0.25,
        "text-field": ["get", "shortname"],
        "text-size": 12,
        "text-offset": [0, 0.5],
        "text-anchor": "top",
        "icon-allow-overlap": true,
      },
      paint: {
        "text-color": "#7e6c56",
        "text-halo-color": "#fff",
        "text-halo-width": 1,
        "text-halo-blur": 0,
      },
    });
  }

  last = props.city;
  lastFilter = props.filter;
};
</script>
