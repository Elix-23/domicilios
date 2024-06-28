<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reservar Domicilio</title>
    <style>
        #pickupMap, #dropoffMap {
            height: 300px;
            width: 100%;
            margin-bottom: 20px;
        }
    </style>
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places"></script>
</head>
<body>
    <h1>Reservar Domicilio</h1>
    <form id="reservationForm" action="/reserve" method="post">
        <label for="pickup_city">Ciudad de Recogida:</label>
        <select id="pickup_city" name="pickup_city" required>
            <option value="">Seleccione una ciudad</option>
            <option value="cali">Cali</option>
            <option value="jamundi">Jamundí</option>
        </select><br>

        <label for="pickup">Dirección de Recogida:</label>
        <input type="text" id="pickup" name="pickup" required>
        <div id="pickupMap"></div><br>

        <label for="dropoff_city">Ciudad de Entrega:</label>
        <select id="dropoff_city" name="dropoff_city" required>
            <option value="">Seleccione una ciudad</option>
            <option value="cali">Cali</option>
            <option value="jamundi">Jamundí</option>
        </select><br>

        <label for="dropoff">Dirección de Entrega:</label>
        <input type="text" id="dropoff" name="dropoff" required>
        <div id="dropoffMap"></div><br>

        <label for="pickup_time">Hora de Recogida:</label>
        <input type="datetime-local" id="pickup_time" name="pickup_time" required><br>

        <label for="return_trip">¿Ida y Vuelta?</label>
        <input type="checkbox" id="return_trip" name="return_trip"><br>

        <h2>Datos del Remitente</h2>
        <label for="sender_name">Nombre del Remitente:</label>
        <input type="text" id="sender_name" name="sender_name" required><br>

        <label for="sender_phone">Teléfono del Remitente:</label>
        <input type="tel" id="sender_phone" name="sender_phone" required><br>

        <h2>Datos del Destinatario</h2>
        <label for="recipient_name">Nombre del Destinatario:</label>
        <input type="text" id="recipient_name" name="recipient_name" required><br>

        <label for="recipient_phone">Teléfono del Destinatario:</label>
        <input type="tel" id="recipient_phone" name="recipient_phone" required><br>

        <button type="submit">Reservar</button>
    </form>

    <script>
        function initMap() {
            var pickupAutocomplete = new google.maps.places.Autocomplete(document.getElementById('pickup'));
            var dropoffAutocomplete = new google.maps.places.Autocomplete(document.getElementById('dropoff'));

            var pickupMap = new google.maps.Map(document.getElementById('pickupMap'), {
                center: {lat: 3.40556, lng: -76.45889}, // Coordenadas de ejemplo
                zoom: 13
            });
            var dropoffMap = new google.maps.Map(document.getElementById('dropoffMap'), {
                center: {lat: 3.40556, lng: -76.45889}, // Coordenadas de ejemplo
                zoom: 13
            });

            pickupAutocomplete.addListener('place_changed', function () {
                var place = pickupAutocomplete.getPlace();
                if (!place.geometry) {
                    return;
                }
                if (place.geometry.viewport) {
                    pickupMap.fitBounds(place.geometry.viewport);
                } else {
                    pickupMap.setCenter(place.geometry.location);
                    pickupMap.setZoom(17);
                }
                new google.maps.Marker({
                    position: place.geometry.location,
                    map: pickupMap
                });
            });

            dropoffAutocomplete.addListener('place_changed', function () {
                var place = dropoffAutocomplete.getPlace();
                if (!place.geometry) {
                    return;
                }
                if (place.geometry.viewport) {
                    dropoffMap.fitBounds(place.geometry.viewport);
                } else {
                    dropoffMap.setCenter(place.geometry.location);
                    dropoffMap.setZoom(17);
                }
                new google.maps.Marker({
                    position: place.geometry.location,
                    map: dropoffMap
                });
            });
        }

        google.maps.event.addDomListener(window, 'load', initMap);
    </script>
</body>
</html>
