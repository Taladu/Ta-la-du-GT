<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>Bản đồ chỉ đường OSM</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
  <link rel="stylesheet" href="https://unpkg.com/leaflet-routing-machine/dist/leaflet-routing-machine.css"/>
  <style>
    html, body { height:100%; margin:0; }
    #map { height:100vh; width:100%; }
  </style>
</head>
<body>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script src="https://unpkg.com/leaflet-routing-machine/dist/leaflet-routing-machine.js"></script>

  <script>
    // Lấy tham số từ URL
    function getLatLng(param) {
      const q = new URLSearchParams(window.location.search);
      if (!q.has(param)) return null;
      const [lat, lon] = q.get(param).split(',').map(Number);
      return (lat && lon) ? L.latLng(lat, lon) : null;
    }

    const A = getLatLng('a'); // Điểm đi
    const B = getLatLng('b'); // Điểm đến

    // Khởi tạo bản đồ
    const map = L.map('map').setView([10.776, 106.700], 12);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '© OpenStreetMap contributors'
    }).addTo(map);

    // Tạo tuyến đường nếu có A và B
    if (A && B) {
      L.Routing.control({
        waypoints: [A, B],
        router: L.Routing.osrmv1({ serviceUrl: 'https://router.project-osrm.org/route/v1' }),
        language: 'vi',
        showAlternatives: false,
        routeWhileDragging: true
      }).addTo(map);
      map.fitBounds(L.latLngBounds([A, B]), {padding:[30,30]});
    } else {
      alert("Chưa có tham số ?a=lat,lon&b=lat,lon trong URL");
    }
  </script>
</body>
</html>
