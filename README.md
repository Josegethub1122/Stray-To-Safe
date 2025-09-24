
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stray to Safe - Home</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      background: #fefefe;
      color: #333;
    }
    header {
      background: url("https://images.unsplash.com/photo-1507149833265-60c372daea22?auto=format&fit=crop&w=1350&q=80") no-repeat center center/cover;
      color: white;
      text-align: center;
      padding: 100px 20px;
    }
    header h1 {
      font-size: 3em;
      margin: 0;
      text-shadow: 2px 2px 6px rgba(0,0,0,0.5);
    }
    header p {
      font-size: 1.3em;
      margin-top: 10px;
    }
    .container {
      padding: 50px 20px;
      text-align: center;
      max-width: 900px;
      margin: auto;
    }
    .quotes {
      font-style: italic;
      font-size: 1.2em;
      color: #555;
      margin: 30px 0;
    }
    .btn {
      display: inline-block;
      padding: 14px 28px;
      background: #4CAF50;
      color: white;
      text-decoration: none;
      border-radius: 8px;
      font-size: 18px;
      transition: 0.3s;
      cursor: pointer;
    }
    .btn:hover {
      background: #388e3c;
    }
    #mapContainer {
      display: none; /* hidden by default */
      margin: 30px auto;
      width: 90%;
      height: 550px;
      border: 2px solid #4CAF50;
      border-radius: 12px;
      overflow: hidden;
    }
    #map {
      width: 100%;
      height: 100%;
    }
    footer {
      background: #4CAF50;
      color: white;
      text-align: center;
      padding: 15px;
      margin-top: 40px;
    }
  </style>
</head>
<body>

  <header>
    <h1>🐾 Stray to Safe</h1>
    <p>“Saving one animal won’t change the world, but for that one animal, the world will change forever.”</p>
  </header>

  <div class="container">
    <h2>About Us</h2>
    <p>
      Stray to Safe is a community-driven initiative to help stray animals find loving homes.  
      With our platform, people can pin locations of strays in need and connect with kind-hearted individuals  
      who are ready to adopt and give them a second chance at life. 🐶🐱
    </p>

    <div class="quotes">
      “Adopting one pet won’t solve the problem of homelessness,  
      but it will mean the whole world to that one pet.”
    </div>

    <div class="quotes">
      “Animals don’t need our pity, they need our help, love, and homes.”
    </div>

    <!-- Button to show the map -->
    <button class="btn" onclick="showMap()">🌍 View Adoption Map</button>

    <!-- Hidden Map Container -->
    <div id="mapContainer">
      <h2 style="text-align:center; margin:15px 0;">🐾 Adoption Map</h2>
      <div id="map"></div>
    </div>
  </div>

  <footer>
    <p>© 2025 Stray to Safe | Together we can save lives 🐾
         For More Details Contact Admin - 9363258022   </p>
    <p>   Thank You  🐶 </p>
  </footer>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    let mapInitialized = false;
    let markers = [];

    function showMap() {
      document.getElementById("mapContainer").style.display = "block";

      if (!mapInitialized) {
        // Initialize map
        var map = L.map('map').setView([20.5937, 78.9629], 5); // Center: India

        // Load tiles
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          maxZoom: 19,
        }).addTo(map);

        // Add marker on click
        map.on('click', function(e) {
          let form = `
            <b>Add Animal for Adoption</b><br><br>
            <label>Animal Type:</label>
            <select id="animalType">
              <option>Dog</option>
              <option>Cat</option>
              <option>Cow</option>
              <option>Donkey</option>
              <option>Other</option>
            </select><br><br>
            <label>Details:</label><br>
            <textarea id="animalDetails" rows="3" cols="25"></textarea><br><br>
            <label>Photo:</label><br>
            <input type="file" id="animalPhoto" accept="image/*"><br><br>
            <button onclick="saveAnimal(${e.latlng.lat}, ${e.latlng.lng})">Save</button>
          `;
          L.popup()
            .setLatLng(e.latlng)
            .setContent(form)
            .openOn(map);
        });

        // Save animal function
        window.saveAnimal = function(lat, lng) {
          let type = document.getElementById("animalType").value;
          let details = document.getElementById("animalDetails").value;
          let photoInput = document.getElementById("animalPhoto");
          let photoURL = "";

          if (photoInput.files && photoInput.files[0]) {
            photoURL = URL.createObjectURL(photoInput.files[0]);
          }

          let popupContent = `
            <b>${type}</b><br>
            ${details}<br>
            ${photoURL ? `<img src="${photoURL}" width="150"><br>` : ""}
            <button onclick="removeAnimal(${lat}, ${lng})">✅ Mark Adopted</button>
          `;

          let marker = L.marker([lat, lng]).addTo(map).bindPopup(popupContent);
          markers.push({lat, lng, marker});
          map.closePopup();
        }

        // Remove adopted animal
        window.removeAnimal = function(lat, lng) {
          let index = markers.findIndex(m => m.lat === lat && m.lng === lng);
          if (index !== -1) {
            map.removeLayer(markers[index].marker);
            markers.splice(index, 1);
            alert("Animal marked as adopted and removed from map 🐾");
          }
        }

        mapInitialized = true;
      }
    }
  </script>

</body>
</html>
