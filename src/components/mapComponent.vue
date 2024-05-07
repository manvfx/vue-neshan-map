<script setup>

import { ref, onMounted, watch } from 'vue'

import { MapComponent, MapTypes } from "@neshan-maps-platform/mapbox-gl-vue";
import "@neshan-maps-platform/mapbox-gl-vue/dist/style.css";
import nmp_mapboxgl from "@neshan-maps-platform/mapbox-gl";

import polyline from "@mapbox/polyline"


const count = ref(0)
const mapRef = ref()

let marker = null;

function mapFunction(map) {

  // افزودن مارکر
  marker = new nmp_mapboxgl.Marker({ color: "red", draggable: true })
        .setLngLat([51.338057, 35.699736]).addTo(map)

  // افزودن کنترلر به نقشه
  map.addControl(new nmp_mapboxgl.FullscreenControl());
  map.addControl(new nmp_mapboxgl.NavigationControl());
  map.addControl(new nmp_mapboxgl.GeolocateControl());

  // افزودن مارکر به نقشه
  drawRouteOnMap(map)


  // افزودن مسیر به نقشه
  drawMarkersOnMap(map)

  setMarker(map)

}

function setMarker(map){

  map.on('click', (e) => {
    console.log(e.lngLat)
      marker.setLngLat([e.lngLat.lng, e.lngLat.lat])
      map.flyTo(
      {
        center: [e.lngLat.lng, e.lngLat.lat],
        essential: true,
        speed: 0.6
      })

    });

}


function drawMarkersOnMap(map) {

  var marker = new nmp_mapboxgl.Marker({ color: "purple" })
    .setLngLat([51.391173, 35.700954])
    .addTo(map);

  var popup = new nmp_mapboxgl.Popup({ offset: 25 }).setText(
    'با نگه داشتن مارکر می‌توانید آن را روی نقشه جابه‌جا کنید'
  );

  var marker_with_popup = new nmp_mapboxgl.Marker({ color: "#00F955", draggable: true }).setPopup(popup)
    .setLngLat([51.4055941, 35.70019216])
    .addTo(map).togglePopup();

  const geojson = {
    'type': 'FeatureCollection',
    'features': [
      {
        'type': 'Feature',
        'geometry': {
          'type': 'Point',
          'coordinates': [51.338057, 35.699736]
        },
        'properties': {
          'title': 'میدان آزادی',
          'description': 'نمایش مارکر با آیکون اختصاصی <br/> مختصات:<br/> [51.338057 , 35.699736]'
        }
      },
      {
        'type': 'Feature',
        'geometry': {
          'type': 'Point',
          'coordinates': [51.375265, 35.744720]
        },
        'properties': {
          'title': 'برج میلاد',
          'description': 'مختصات:<br/> [51.375265 , 35.744720]'
        }
      }
    ]
  };

  // add markers to map
  for (const feature of geojson.features) {
    const el = document.createElement('div');
    el.className = 'marker';
    el.style.background = `url(\'/custom_marker.png\')`;
    el.style.width = '32px'
    el.style.height = '40px'
    el.style.top = '-20px'
    el.style.cursor = 'pointer'
    console.log(el)

    new nmp_mapboxgl.Marker(el)
      .setLngLat(feature.geometry.coordinates)
      .setPopup(
        new nmp_mapboxgl.Popup({ offset: 40 })
          .setHTML(
            `<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
          )
      )
      .addTo(map).togglePopup();
  }

}


function drawRouteOnMap(map) {

  var exampleResponse = {
    "routes": [
      {
        "overview_polyline": {
          "points": "cy{xEa{sxHCyEr@}FIi@MWi@Um@L[l@A^{Jr@"
        },
        "legs": [
          {
            "summary": "میدان انقلاب اسلامی - کارگر شمالی",
            "distance": {
              "value": 555.0,
              "text": "۵۷۵ متر"
            },
            "duration": {
              "value": 99.0,
              "text": "۲ دقیقه"
            },
            "steps": [
              {
                "name": "آزادی",
                "instruction": "در جهت شرق در آزادی قرار بگیرید",
                "bearing_after": 88,
                "type": "depart",
                "distance": {
                  "value": 197.0,
                  "text": "۲۰۰ متر"
                },
                "duration": {
                  "value": 35.0,
                  "text": "۱ دقیقه"
                },
                "polyline": "cy{xEa{sxHAkBAmBDa@BKHs@BWD]J{@",
                "start_location": [
                  51.388811,
                  35.70082
                ]
              },
              {
                "name": "کارگر شمالی",
                "instruction": "در میدان انقلاب اسلامی، از خروجی سوم، خارج شوید",
                "rotaryName": "میدان انقلاب اسلامی",
                "bearing_after": 111,
                "type": "rotary",
                "modifier": "straight",
                "exit": 3,
                "distance": {
                  "value": 146.0,
                  "text": "۱۵۰ متر"
                },
                "duration": {
                  "value": 38.0,
                  "text": "۱ دقیقه"
                },
                "polyline": "}w{xEohtxHDSBUCUESEKGKSOUEW@UJORKXAN?N",
                "start_location": [
                  51.390956,
                  35.700632
                ]
              },
              {
                "name": "",
                "instruction": "به مسیر خود ادامه دهید",
                "bearing_after": 354,
                "type": "exit rotary",
                "modifier": "right",
                "exit": 3,
                "distance": {
                  "value": 212.0,
                  "text": "۲۲۵ متر"
                },
                "duration": {
                  "value": 39.0,
                  "text": "۱ دقیقه"
                },
                "polyline": "a|{xEuitxH_ADaBLO@{BRmAH",
                "start_location": [
                  51.391154,
                  35.701293
                ]
              },
              {
                "name": "کارگر شمالی",
                "instruction": "در مقصد قرار دارید",
                "bearing_after": 0,
                "type": "arrive",
                "distance": {
                  "value": 0.0,
                  "text": ""
                },
                "duration": {
                  "value": 0.0,
                  "text": ""
                },
                "polyline": "}g|xEahtxH",
                "start_location": [
                  51.390885,
                  35.703188
                ]
              }
            ]
          }
        ]
      }
    ]
  }


  var routes = [];
  var points = [];

  for (let k = 0; k < exampleResponse.routes.length; k++) {
    for (let j = 0; j < exampleResponse.routes[k].legs.length; j++) {
      for (let i = 0; i < exampleResponse.routes[k].legs[j].steps.length; i++) {

        var step = exampleResponse.routes[k].legs[j].steps[i]["polyline"];
        var point = exampleResponse.routes[k].legs[j].steps[i]["start_location"];

        var route = polyline.decode(step, 5);

        route.map(item => { item.reverse() })

        routes.push(route);
        points.push(point);

      }
    }
  }

  var routeObj = {
    type: 'FeatureCollection',
    features: [
      {
        type: 'Feature',
        geometry: {
          type: 'MultiLineString',
          coordinates: routes
        }
      }
    ]
  };

  var pointsObj = {
    type: 'FeatureCollection',
    features: [
      {
        type: "Feature",
        geometry:
        {
          "type": "MultiPoint",
          "coordinates": points
        }
      }
    ]
  };


  map.on('load', () => {
    map.addSource('route', {
      type: 'geojson',
      data: routeObj
    });

    map.addSource('points1', {
      type: 'geojson',
      data: pointsObj
    });

    map.addLayer({
      id: 'route-line',
      type: 'line',
      source: 'route',
      layout: {
        'line-join': 'round',
        'line-cap': 'round'
      },
      paint: {
        'line-color': '#250ECD',
        'line-width': 9
      }
    });

    map.addLayer({
      id: 'points1',
      type: 'circle',
      source: 'points1',
      paint: {
        "circle-color": "#9fbef9",
        "circle-stroke-color": "#FFFFFF",
        "circle-stroke-width": 2,
        "circle-radius": 5
      }
    });
  });

}
</script>


<style scoped>
.mapboxgl-popup {
  max-width: 200px;

}

.mapboxgl-popup-content {
  direction: rtl;
  background-color: red !important;
  text-align: center;
  font-family: 'Open Sans', sans-serif;
}
</style>


<template>
  <div style="background-color: #999; width:960px;height: 680px;">

    <MapComponent :options="{
      mapKey: 'web.b70a6381fedd45cb9257d78a29606dae',
      mapType: MapTypes.neshanVector,
      poi: false,
      traffic: false,
      isTouchPlatform: false,
      zoom: 13,
      center: [51.391173, 35.700954],
      trackResize: true,
      mapTypeControllerStatus: {
        show: true,
        position: 'bottom-left'
      }
    }" :map-setter="mapFunction" />
  </div>
</template>




