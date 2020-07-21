<template>
  <div id="SearchResultMap" class="row">
    <!-- MAPBOX MAP CONTAINER -->
    <div id="map"></div>
  </div>
</template>

<script>
  /* eslint-disable no-unused-vars */
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
    mounted: function() {
      this.loadDefaultPosition();
    },
    methods: {
      // Run when there ARE map locations to display
      placeMarkers: function (locations) {
        const mapConfig = {
          mapCenter: [-80.1918, 25.7617],
          mapContainerDivId: 'map',
          mapboxStyleString: 'mapbox://styles/mapbox/streets-v11',
          mapboxIconName: "fast-food-15",
          accessToken: "pk.eyJ1IjoiYWxpcnNhdHRhciIsImEiOiJja2N1dHd5d3EwMGt3MnlwYmUxNHltcDAzIn0.cxS0uUsukIYZeZ_PQOdX6A",
          staticMarkerFilePath: "some_url",
          zoomLevel: 8,
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
        
        let formattedMarkers = [];

        // Marker data was retrieved and parsed correctly
        if (Array.isArray(markers) && markers.length) {
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
          
          map.jumpTo({
            center: [ markers[0].location[0], markers[0].location[1] ]
          });
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
      },
      // Run by default, with no map locations
      loadDefaultPosition: function() {
        const mapConfig = {
          mapCenter: [-80.1918, 25.7617],
          mapContainerDivId: 'map',
          mapboxStyleString: 'mapbox://styles/mapbox/streets-v11',
          mapboxIconName: "fast-food-15",
          accessToken: "pk.eyJ1IjoiYWxpcnNhdHRhciIsImEiOiJja2N1dHd5d3EwMGt3MnlwYmUxNHltcDAzIn0.cxS0uUsukIYZeZ_PQOdX6A",
          staticMarkerFilePath: "some_url",
          zoomLevel: 8,
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
        };

        mapboxgl.accessToken = mapConfig.accessToken;

        const map = new mapboxgl.Map({
          container:  mapConfig.mapContainerDivId,
          style:      mapConfig.mapboxStyleString,
          center:     mapConfig.mapCenter,
          zoom:       mapConfig.zoomLevel
        });
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
  #map {
    height: 450px;
    width: 100%;
    border-radius: 5px;
  }
</style>