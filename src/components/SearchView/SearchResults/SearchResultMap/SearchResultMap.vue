<template>
  <div id="SearchResultMap" class="row">
    <!-- MAPBOX MAP CONTAINER -->
    <div id="map"></div>
  </div>
</template>

<script>
const mapboxgl = require('mapbox-gl');

export default {
  name: "SearchResultMap",
  props: {
    locations: Array
  },
  data: ()=> {
    return {
    }
  },
  methods: {
    placeMarkers: function (locations) {
      console.log({locations});

      const mapConfig = {
        mapContainerDivId: 'map',
        mapboxStyleString: 'mapbox://styles/mapbox/light-v10',
        mapboxIconName: "town-hall-15",
        accessToken: "pk.eyJ1IjoiYWxpcnNhdHRhciIsImEiOiJja2N1dHd5d3EwMGt3MnlwYmUxNHltcDAzIn0.cxS0uUsukIYZeZ_PQOdX6A",
        staticMarkerFilePath: "some_url",
        // mapCenter: [-80.1815, 26.8065],
        zoomLevel: 2,
        clustering: false,
        popupHtml: (siteInfo)=>{
          return `
            <div class="gmap_bubble">
              <h3>${siteInfo.name}</h3>
              <p>
                <a class="directions" href="${siteInfo.url}" target="_blank">
                  Visit Site
                </a>
              </p>
            </div>
          `;
        }
      }

      const markers = [];

      for(let location of locations) {
        const formattedMarker = {
          name: location.name,
          location: [ (location.lng).toString(), (location.lat).toString() ],
          url: location.reserve_url
        }

        markers.push(formattedMarker)
      }
      
      // Set home coordinates (starting center of map) to Tunis, Tunisia (this gives a better global view @ 940px map width)
      // (NB: MapBox uses longitude + latitude, while we use lat-long, so have to reverse the array order)
      let formattedMarkers = [];

      // Marker data was retrieved and parsed correctly
      if (Array.isArray(markers) && markers.length) {

        console.warn('+++ MARKERS COMING THROUGH', markers)

        // Format site info 
        let id = 1;

        for (let site of markers) {
          const markerObject = {
            type: 'Feature',
            id: id,
            geometry: {
              type: 'Point',
              coordinates: [Number(site.location[0]), Number(site.location[1])],
            },
            properties: {
              id:       id,
              name:     site.name,
              url:      site.url
            }
          };

          formattedMarkers.push(markerObject);
          id++;
        }

      } else {
        // Something went wrong with retrieving site info from DB,
        // or with parsing site info
        console.error('NO MARKERS COMING THROUGH');
      }

      mapboxgl.accessToken = mapConfig.accessToken;

      const map = new mapboxgl.Map({
        container:  mapConfig.mapContainerDivId,
        style:      mapConfig.mapboxStyleString,
        center:     mapConfig.mapCenter,
        zoom:       mapConfig.zoomLevel
      });

      // Set up static elements of the map on completion of map loading
      map.on('load', function() {
        map.addSource("site_locations", {
          type: "geojson",
          data: {
            "type": "FeatureCollection",
            "features": formattedMarkers
          },
          cluster: mapConfig.clustering,
          clusterMaxZoom: 1, // Max zoom to cluster points on
          clusterRadius: 1, // Radius of each cluster when clustering points (defaults to 50)
        });

        map.addLayer({
          "id":     "sites",
          "type":   "symbol",
          "source": "site_locations",
          "layout": {
            // To use a custom icon:
            // 1. Uncomment the 'map.on('load'...)' line above
            // 2. Assign the staticMarkerFilePath var to a static URL for the icon
            // 3. Replace the below 'icon-image' field with the value "custom-icon"
            //
            // Listing of all Maki icons and their names:
            // https://labs.mapbox.com/maki-icons/

            "icon-image": mapConfig.mapboxIconName,
            "icon-ignore-placement": true,
            "text-field": "{title}",
            "text-font": ["Open Sans Semibold", "Arial Unicode MS Bold"],
            "text-offset": [0, 0.6],
            "text-anchor": "top"
          }
        });

        let arrayCopy = Object.assign([], markers);
        const markersSortedByGreatestLongitude = arrayCopy.sort((a,b)=> {
          if (a.location[0] > b.location[0]) {
            return 1;
          } else if (a.location[0] < b.location[0]){
            return -1;
          }

          return 0;
        });

        arrayCopy = Object.assign([], markers);
        const markersSortedByGreatestLatitude = arrayCopy.sort((a,b)=> {
          if (a.location[1] > b.location[1]) {
            return 1;
          } else if (a.location[1] < b.location[1]){
            return -1;
          }

          return 0;
        });

        const highestLongitude = markersSortedByGreatestLongitude[0];
        const highestLatitude = markersSortedByGreatestLatitude[0];

        console.log({ markersSortedByGreatestLongitude, markersSortedByGreatestLatitude, highestLongitude, highestLatitude });
        
        map.fitBounds([
          [Number(highestLatitude.location[0]), Number(highestLatitude.location[1])],
          [Number(highestLongitude.location[0]), Number(highestLongitude.location[1])],
        ]);
      });

      // Set up click event for staff member icons
      map.on('click', 'sites', function (e) {
        let site = e.features[0].properties;
        let coordinates = e.features[0].geometry.coordinates.slice();

        const popupHtml = mapConfig.popupHtml(site);
        
        // Ensure that if the map is zoomed out such that multiple
        // copies of the feature are visible, the popup appears
        // over the copy being pointed to.
        while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
          coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
        }
        
        new mapboxgl.Popup()
          .setLngLat(coordinates)
          .setHTML(popupHtml)
          .setMaxWidth('300px')
          .addTo(map);
      });

      // Change the cursor to a pointer when the mouse is over the places layer.
      map.on('mouseenter', 'sites', function () {
        map.getCanvas().style.cursor = 'pointer';
      });
      
      // Change it back to a pointer when it leaves.
      map.on('mouseleave', 'sites', function () {
        map.getCanvas().style.cursor = '';
      });

      console.log({ map });
    }
  },
  watch: {
    locations: function() {
      this.placeMarkers(this.$props.locations)
    }
  }
}
</script>

<style scoped>
#SearchResultMap {
  border: 1px solid;
  padding: 10px;
  margin: 0px 10px 10px 10px;
}

#map {
  height: 450px;
  width: 100%;
  border: 1px solid #333;
}
</style>