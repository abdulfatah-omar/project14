# project14

<!DOCTYPE html>
<html>
<head>
  <title>Simple Google Map</title>
  <style>
    /* Set the size of the map */
    #map {
      height: 400px;
      width: 100%;
    }
  </style>
</head>
<body>
  <h1>Google Maps Example</h1>
  <div id="map"></div>

  <script>
    // Initialize and create the map
    function initMap() {
      // Location coordinates (latitude and longitude)
      var myLatLng = { lat: 37.7749, lng: -122.4194 };

      // Create a new map centered at the specified location
      var map = new google.maps.Map(document.getElementById('map'), {
        center: myLatLng,
        zoom: 10 // Adjust the zoom level as needed
      });

      // Create a marker at the specified location
      var marker = new google.maps.Marker({
        position: myLatLng,
        map: map,
        title: 'San Francisco'
      });

      // Create a circle on the map
      var circle = new google.maps.Circle({
        strokeColor: 'blue',
        strokeOpacity: 0.8,
        strokeWeight: 2,
        fillColor: '#FFF',
        fillOpacity: 0.5,
        map: map,
        center: myLatLng,
        radius: 600
      });

      // Info Window
      var infowindow = new google.maps.InfoWindow({
        content: 'Test'
      });

      // Marker click event
      google.maps.event.addListener(marker, 'click', function() {
        infowindow.open(map, marker);
      });

      // Map click event
      map.addListener('click', function(e) {
        alert('You clicked the map at ' + JSON.stringify(e.latLng.toJSON(), null, 2));
      });

      // Geocoding function
      function getCoordinates() {
        var address = document.getElementById('address').value;
        var geocoder = new google.maps.Geocoder();
        geocoder.geocode({ 'address': address }, function(results, status) {
          if (status == 'OK') {
            map.setCenter(results[0].geometry.location);
            var marker = new google.maps.Marker({
              map: map,
              position: results[0].geometry.location
            });
          } else {
            alert('Geocode was not successful for the following reason: ' + status);
          }
        });
      }

      // Directions
      var directionsService = new google.maps.DirectionsService();
      var directionsRenderer = new google.maps.DirectionsRenderer();
      directionsRenderer.setMap(map);

      var request = {
        origin: 'San Francisco, CA',
        destination: 'Mountain View, CA',
        travelMode: 'DRIVING'
      };

      directionsService.route(request, function(result, status) {
        if (status == 'OK') {
          directionsRenderer.setDirections(result);
        }
      });
    }
  </script>

  <!-- Include the Google Maps API with your API key -->
  <script async defer
    src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap">
  </script>

  <!-- Attribution -->
  <p>Attribution: Google Maps Platform. (n.d.). [Video]. Google Developers. <a href="https://developers.google.com/maps">https://developers.google.com/maps</a></p>
</body>
</html>
