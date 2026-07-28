<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Luxury Hotel Management System</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css" />
  <style>
    body {
      background: #f4f7fc;
      font-family: system-ui, -apple-system, sans-serif;
    }

    .card {
      border: none;
      border-radius: 1rem;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.04);
      overflow: hidden;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }

    .room-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.08);
    }

    .room-card-img-wrapper {
      position: relative;
      height: 200px;
      overflow: hidden;
      background-color: #e9ecef;
    }

    .room-card-img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.3s ease;
    }

    .room-card:hover .room-card-img {
      transform: scale(1.05);
    }

    .dashboard-stat {
      transition: 0.2s;
      cursor: default;
    }

    .dashboard-stat:hover {
      background: #f0f5ff;
      transform: scale(1.01);
    }

    .status-badge {
      position: absolute;
      top: 12px;
      right: 12px;
      backdrop-filter: blur(4px);
      padding: 0.35em 0.75em;
    }

    .price-tag {
      position: absolute;
      bottom: 12px;
      left: 12px;
      background: rgba(0, 0, 0, 0.65);
      color: white;
      backdrop-filter: blur(4px);
      padding: 0.25rem 0.6rem;
      border-radius: 0.5rem;
      font-size: 0.85rem;
    }

    .login-card {
      max-width: 420px;
      margin: 0 auto;
    }

    .footer {
      font-size: 0.9rem;
      color: #6c757d;
    }

    .hotel-logo {
      font-size: 2.5rem;
    }
  </style>
</head>
<body>

<div id="app" class="container py-4">

  <!-- Login Section -->
  <div id="loginSection" class="row justify-content-center align-items-center" style="min-height: 80vh;">
    <div class="col-11 col-sm-8 col-md-6 col-lg-5">
      <div class="card login-card p-4 p-md-5">
        <div class="card-body">
          <span class="hotel-logo text-center d-block"><i class="bi bi-building text-primary"></i></span>
          <h3 class="mt-2 text-center">Grand Royale</h3>
          <p class="text-center text-muted mb-4">Hotel Management Portal</p>
          <form id="loginForm">
            <div class="mb-3">
              <label class="form-label">Email address</label>
              <input type="email" id="loginEmail" class="form-control" value="admin@hotel.com" />
            </div>
            <div class="mb-3">
              <label class="form-label">Password</label>
              <input type="password" id="loginPassword" class="form-control" value="123456" />
            </div>
            <button type="submit" class="btn btn-primary w-100 py-2">Login <i class="bi bi-box-arrow-in-right"></i></button>
            <div id="loginError" class="text-danger mt-2 text-center small"></div>
          </form>
          <div class="mt-3 text-center small text-muted">Demo: admin@hotel.com / 123456</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Dashboard Section -->
  <div id="dashboardSection" style="display: none;">
    <div class="d-flex flex-wrap justify-content-between align-items-center mb-4 pb-2 border-bottom">
      <h4 class="mb-0"><i class="bi bi-grid-3x3-gap-fill me-2 text-primary"></i>Dashboard</h4>
      <div class="d-flex align-items-center gap-3">
        <span class="badge bg-light text-dark border py-2 px-3"><i class="bi bi-person-circle me-1"></i><span id="userDisplay"></span></span>
        <button id="logoutBtn" class="btn btn-outline-secondary btn-sm"><i class="bi bi-box-arrow-right"></i> Logout</button>
      </div>
    </div>

    <!-- Stats -->
    <div class="row g-3 mb-4">
      <div class="col-6 col-md-3">
        <div class="card dashboard-stat p-3 text-center">
          <h5 class="text-muted m-0 fs-6">Total Rooms</h5>
          <h3 id="statTotalRooms" class="m-0 mt-2 fw-bold">0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="card dashboard-stat p-3 text-center">
          <h5 class="text-muted m-0 fs-6">Available</h5>
          <h3 id="statAvailable" class="m-0 mt-2 fw-bold text-success">0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="card dashboard-stat p-3 text-center">
          <h5 class="text-muted m-0 fs-6">Bookings</h5>
          <h3 id="statBookings" class="m-0 mt-2 fw-bold text-primary">0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="card dashboard-stat p-3 text-center">
          <h5 class="text-muted m-0 fs-6">Guests</h5>
          <h3 id="statCustomers" class="m-0 mt-2 fw-bold">0</h3>
        </div>
      </div>
    </div>

    <!-- Tabs -->
    <ul class="nav nav-tabs mb-3" id="maintabs" role="tablist">
      <li class="nav-item">
        <button class="nav-link active" id="roomsTab" data-bs-toggle="tab" data-bs-target="#roomsPanel"><i class="bi bi-door-open me-1"></i> Rooms</button>
      </li>
      <li class="nav-item">
        <button class="nav-link" id="bookingsTab" data-bs-toggle="tab" data-bs-target="#bookingsPanel"><i class="bi bi-journal-check me-1"></i> Bookings</button>
      </li>
      <li class="nav-item">
        <button class="nav-link" id="profileTab" data-bs-toggle="tab" data-bs-target="#profilePanel"><i class="bi bi-person-badge me-1"></i> Guest Profiles</button>
      </li>
    </ul>

    <div class="tab-content">
      <!-- Rooms Panel -->
      <div class="tab-pane fade show active" id="roomsPanel">
        <div class="row g-3 mb-3 align-items-end">
          <div class="col-md-4">
            <label class="form-label">Search rooms</label>
            <input type="text" id="roomSearch" class="form-control" placeholder="Search by name..." />
          </div>
          <div class="col-md-4">
            <label class="form-label">Filter by type</label>
            <select id="roomTypeFilter" class="form-select">
              <option value="">All types</option>
              <option value="Single">Single</option>
              <option value="Double / Queen">Double / Queen</option>
              <option value="King">King</option>
              <option value="Twin">Twin</option>
              <option value="Double-Double / Quad">Double-Double / Quad</option>
              <option value="Studio">Studio</option>
              <option value="Junior Suite">Junior Suite</option>
              <option value="Full Suite">Full Suite</option>
              <option value="Royal Suite">Royal Suite</option>
              <option value="Villa">Villa</option>
            </select>
          </div>
          <div class="col-md-3 ms-auto d-flex gap-2">
            <button id="addRoomBtn" class="btn btn-primary w-100" data-bs-toggle="modal" data-bs-target="#roomModal"><i class="bi bi-plus-circle me-1"></i> Add Room</button>
          </div>
        </div>
        <div id="roomsContainer" class="row g-3"></div>
      </div>

      <!-- Bookings Panel -->
      <div class="tab-pane fade" id="bookingsPanel">
        <div class="d-flex justify-content-between align-items-center flex-wrap mb-3">
          <h5 class="mb-0"><i class="bi bi-calendar2 me-2"></i>Booking History</h5>
          <button id="newBookingBtn" class="btn btn-success btn-sm" data-bs-toggle="modal" data-bs-target="#bookingModal"><i class="bi bi-plus-lg me-1"></i> New Booking</button>
        </div>
        <div id="bookingsList"></div>
      </div>

      <!-- Profile Panel -->
      <div class="tab-pane fade" id="profilePanel">
        <div class="card p-4">
          <h5 class="mb-3"><i class="bi bi-people me-2"></i>Registered Guests</h5>
          <div id="customerListContainer" class="mt-2"></div>
        </div>
      </div>
    </div>

    <div class="footer mt-5 text-center border-top pt-3">Grand Royale Hotel Management System</div>
  </div>

  <!-- Room Modal -->
  <div class="modal fade" id="roomModal" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header"><h5 class="modal-title" id="roomModalTitle">Room Details</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
        <div class="modal-body">
          <form id="roomForm">
            <input type="hidden" id="roomId" />
            <div class="mb-3"><label class="form-label">Room Name</label><input type="text" id="roomName" class="form-control" required placeholder="e.g. Royal Ocean Villa" /></div>
            <div class="mb-3"><label class="form-label">Type & Layout</label>
              <select id="roomType" class="form-select" required>
                <option value="Single">Single</option>
                <option value="Double / Queen">Double / Queen</option>
                <option value="King">King</option>
                <option value="Twin">Twin</option>
                <option value="Double-Double / Quad">Double-Double / Quad</option>
                <option value="Studio">Studio</option>
                <option value="Junior Suite">Junior Suite</option>
                <option value="Full Suite">Full Suite</option>
                <option value="Royal Suite">Royal Suite</option>
                <option value="Villa">Villa</option>
              </select>
            </div>
            <div class="mb-3"><label class="form-label">Price per night ($)</label><input type="number" id="roomPrice" class="form-control" required /></div>
            <div class="mb-3"><label class="form-label">Custom Image URL (Optional)</label><input type="url" id="roomImage" class="form-control" placeholder="https://..." /></div>
            <div class="mb-3"><label class="form-label">Status</label>
              <select id="roomStatus" class="form-select"><option value="available">Available</option><option value="booked">Booked</option></select>
            </div>
          </form>
        </div>
        <div class="modal-footer"><button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button><button type="button" id="roomSaveBtn" class="btn btn-primary">Save Room</button></div>
      </div>
    </div>
  </div>

  <!-- Booking Modal -->
  <div class="modal fade" id="bookingModal" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header"><h5 class="modal-title">New Booking</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
        <div class="modal-body">
          <form id="bookingForm">
            <div class="mb-3"><label class="form-label">Guest Name</label><input type="text" id="bookingCustomer" class="form-control" required /></div>
            <div class="mb-3"><label class="form-label">Select Room</label><select id="bookingRoomSelect" class="form-select" required></select></div>
            <div class="mb-3"><label class="form-label">Check-in Date</label><input type="date" id="bookingCheckin" class="form-control" required /></div>
            <div class="mb-3"><label class="form-label">Check-out Date</label><input type="date" id="bookingCheckout" class="form-control" required /></div>
          </form>
        </div>
        <div class="modal-footer"><button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button><button type="button" id="bookingSaveBtn" class="btn btn-primary">Save Booking</button></div>
      </div>
    </div>
  </div>

</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/js/bootstrap.bundle.min.js"></script>
<script>
  (function() {
    const STORAGE = {
      rooms: 'hotel_rooms_v5',
      bookings: 'hotel_bookings_v5',
      currentUser: 'hotel_current_user'
    };

    const TYPE_FALLBACKS = {
      'Single': 'https://images.unsplash.com/photo-1631049307264-da0ec9d70304?auto=format&fit=crop&w=800&q=80',
      'Double / Queen': 'https://cdn.thealtairhotel.com/s3-uploads/20210908171806/Altair-QQ-1-LG-1920x1281.jpg',
      'King': 'https://ik.imagekit.io/tvlk/blog/2024/12/shutterstock_2521917539.jpg?tr=q-70,c-at_max,w-1000,h-600',
      'Twin': 'https://arlohotels.com/soho/wp-content/uploads/sites/2/2022/05/two-twin-room-1.jpeg',
      'Double-Double / Quad': 'https://www.frasersproperty.com/content/dam/frasers-hospitality/english/properties/united-kingdom/south-kensington/park-international-hotel-south-kensington/images/gallery-images/rooms/room-type-main-images/quad-room/PIHL_Quad%20Room.jpg',
      'Studio': 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?auto=format&fit=crop&w=800&q=80',
      'Junior Suite': 'https://images.unsplash.com/photo-1591088398332-8a7791972843?auto=format&fit=crop&w=800&q=80',
      'Full Suite': 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80',
      'Royal Suite': 'https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?auto=format&fit=crop&w=800&q=80',
      'Villa': 'https://images.unsplash.com/photo-1613977257363-707ba9348227?auto=format&fit=crop&w=800&q=80'
    };

    function getData(key) {
      try { return JSON.parse(localStorage.getItem(key)) || []; } catch (e) { return []; }
    }

    function setData(key, data) {
      localStorage.setItem(key, JSON.stringify(data));
    }

    function initRooms() {
      if (!localStorage.getItem(STORAGE.rooms)) {
        const defaults = [
          // Scenic Views
          { id: 1, name: 'Ocean Vista Suite', type: 'King', price: 350, status: 'available', image: 'https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?auto=format&fit=crop&w=800&q=80' },
          { id: 2, name: 'Sunset Bay Deluxe', type: 'Double / Queen', price: 260, status: 'available', image: 'https://images.unsplash.com/photo-1566665797739-1674de7a421a?auto=format&fit=crop&w=800&q=80' },
          { id: 3, name: 'Garden Sanctuary Room', type: 'Twin', price: 190, status: 'booked', image: 'https://images.unsplash.com/photo-1590490360182-c33d57733427?auto=format&fit=crop&w=800&q=80' },
          { id: 4, name: 'Skyline Terrace King', type: 'King', price: 380, status: 'available', image: 'https://ik.imagekit.io/tvlk/blog/2024/12/shutterstock_2521917539.jpg?tr=q-70,c-at_max,w-1000,h-600' },
          { id: 5, name: 'Mountain Crest Studio', type: 'Studio', price: 210, status: 'available', image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?auto=format&fit=crop&w=800&q=80' },
          
          // Color & Gemstone Themes
          { id: 6, name: 'Emerald Haven Room', type: 'Double-Double / Quad', price: 310, status: 'available', image: 'https://images.unsplash.com/photo-1602002418082-a4443e081dd1?auto=format&fit=crop&w=800&q=80' },
          { id: 7, name: 'Sapphire Coastal Suite', type: 'Junior Suite', price: 420, status: 'booked', image: 'https://images.unsplash.com/photo-1591088398332-8a7791972843?auto=format&fit=crop&w=800&q=80' },
          { id: 8, name: 'Amber Sunset Villa', type: 'Villa', price: 850, status: 'available', image: 'https://images.unsplash.com/photo-1613977257363-707ba9348227?auto=format&fit=crop&w=800&q=80' },
          { id: 9, name: 'Ivory Crown Penthouse', type: 'Royal Suite', price: 1200, status: 'available', image: 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80' },

          // Architectural / Suite Additions
          { id: 10, name: 'Grand Executive Full Suite', type: 'Full Suite', price: 600, status: 'available', image: 'https://images.unsplash.com/photo-1578683010236-d716f9a3f461?auto=format&fit=crop&w=800&q=80' },
          { id: 11, name: 'Private Lagoon Sanctuary', type: 'Villa', price: 950, status: 'available', image: 'https://images.unsplash.com/photo-1540555700478-4be289fbecef?auto=format&fit=crop&w=800&q=80' }
        ];
        setData(STORAGE.rooms, defaults);
      }
    }

    function initBookings() {
      if (!localStorage.getItem(STORAGE.bookings)) {
        setData(STORAGE.bookings, [
          { id: 101, customer: 'Alice Johnson', roomId: 3, checkin: '2026-07-20', checkout: '2026-07-25' },
          { id: 102, customer: 'David Smith', roomId: 7, checkin: '2026-07-22', checkout: '2026-07-29' }
        ]);
      }
    }

    initRooms(); 
    initBookings();

    const $ = id => document.getElementById(id);

    function showDashboard(user) {
      localStorage.setItem(STORAGE.currentUser, JSON.stringify(user));
      $('loginSection').style.display = 'none';
      $('dashboardSection').style.display = 'block';
      $('userDisplay').textContent = user.name || 'Admin';
      refreshAll();
    }

    $('loginForm').addEventListener('submit', (e) => {
      e.preventDefault();
      if ($('loginEmail').value === 'admin@hotel.com' && $('loginPassword').value === '123456') {
        showDashboard({ email: $('loginEmail').value, name: 'Admin Manager' });
      } else {
        $('loginError').textContent = 'Invalid credentials. Use admin@hotel.com / 123456';
      }
    });

    $('logoutBtn').onclick = () => {
      localStorage.removeItem(STORAGE.currentUser);
      location.reload();
    };

    if (localStorage.getItem(STORAGE.currentUser)) {
      showDashboard(JSON.parse(localStorage.getItem(STORAGE.currentUser)));
    }

    function renderRooms() {
      const container = $('roomsContainer');
      const search = $('roomSearch').value.toLowerCase();
      const typeFilter = $('roomTypeFilter').value;
      let rooms = getData(STORAGE.rooms);

      if (search) rooms = rooms.filter(r => r.name.toLowerCase().includes(search));
      if (typeFilter) rooms = rooms.filter(r => r.type === typeFilter);

      if (rooms.length === 0) {
        container.innerHTML = '<div class="col-12 text-center text-muted py-5">No rooms found.</div>';
        return;
      }

      container.innerHTML = rooms.map(room => {
        const imageUrl = room.image || TYPE_FALLBACKS[room.type] || TYPE_FALLBACKS['Single'];
        return `
          <div class="col-md-6 col-lg-4">
            <div class="card room-card h-100 d-flex flex-column justify-content-between">
              <div>
                <div class="room-card-img-wrapper">
                  <img src="${imageUrl}" class="room-card-img" alt="${room.name}" onerror="this.src='${TYPE_FALLBACKS['Single']}'" />
                  <span class="badge ${room.status === 'available' ? 'bg-success' : 'bg-warning text-dark'} status-badge">${room.status}</span>
                  <div class="price-tag"><strong>$${room.price}</strong> / night</div>
                </div>
                <div class="p-3">
                  <div class="d-flex justify-content-between align-items-center mb-1">
                    <h5 class="card-title mb-0 fs-6 fw-bold">${room.name}</h5>
                    <span class="badge bg-secondary">${room.type}</span>
                  </div>
                </div>
              </div>
              <div class="p-3 pt-0 d-flex gap-2 border-0">
                <button class="btn btn-sm btn-outline-primary editRoomBtn" data-id="${room.id}"><i class="bi bi-pencil"></i> Edit</button>
                <button class="btn btn-sm btn-outline-danger deleteRoomBtn" data-id="${room.id}"><i class="bi bi-trash"></i></button>
                ${room.status === 'available' 
                  ? `<button class="btn btn-sm btn-success checkInBtn ms-auto" data-id="${room.id}"><i class="bi bi-box-arrow-in-right"></i> Check In</button>`
                  : `<button class="btn btn-sm btn-warning checkOutBtn ms-auto" data-id="${room.id}"><i class="bi bi-box-arrow-right"></i> Check Out</button>`
                }
              </div>
            </div>
          </div>
        `;
      }).join('');

      container.querySelectorAll('.editRoomBtn').forEach(btn => btn.onclick = () => editRoom(Number(btn.dataset.id)));
      container.querySelectorAll('.deleteRoomBtn').forEach(btn => btn.onclick = () => deleteRoom(Number(btn.dataset.id)));
      container.querySelectorAll('.checkInBtn').forEach(btn => btn.onclick = () => checkInRoom(Number(btn.dataset.id)));
      container.querySelectorAll('.checkOutBtn').forEach(btn => btn.onclick = () => checkOutRoom(Number(btn.dataset.id)));
    }

    function checkInRoom(roomId) {
      const name = prompt('Guest full name for check-in:');
      if (!name) return;
      let rooms = getData(STORAGE.rooms).map(r => r.id === roomId ? { ...r, status: 'booked' } : r);
      let bookings = getData(STORAGE.bookings);
      bookings.push({ id: Date.now(), customer: name, roomId: roomId, checkin: '2026-07-28', checkout: '2026-07-31' });
      setData(STORAGE.rooms, rooms);
      setData(STORAGE.bookings, bookings);
      refreshAll();
    }

    function checkOutRoom(roomId) {
      let rooms = getData(STORAGE.rooms).map(r => r.id === roomId ? { ...r, status: 'available' } : r);
      let bookings = getData(STORAGE.bookings).filter(b => b.roomId !== roomId);
      setData(STORAGE.rooms, rooms);
      setData(STORAGE.bookings, bookings);
      refreshAll();
    }

    function editRoom(id) {
      const room = getData(STORAGE.rooms).find(r => r.id === id);
      if (!room) return;
      $('roomId').value = room.id;
      $('roomName').value = room.name;
      $('roomType').value = room.type;
      $('roomPrice').value = room.price;
      $('roomImage').value = room.image || '';
      $('roomStatus').value = room.status;
      new bootstrap.Modal($('roomModal')).show();
    }

    function deleteRoom(id) {
      if (!confirm('Remove this room record permanently?')) return;
      setData(STORAGE.rooms, getData(STORAGE.rooms).filter(r => r.id !== id));
      refreshAll();
    }

    function renderBookings() {
      const bookings = getData(STORAGE.bookings);
      const rooms = getData(STORAGE.rooms);
      const container = $('bookingsList');

      if (!bookings.length) {
        container.innerHTML = '<div class="text-center text-muted py-4">No active room bookings found.</div>';
        return;
      }

      container.innerHTML = `<ul class="list-group">` + bookings.map(b => {
        const room = rooms.find(r => r.id === b.roomId);
        const roomImg = room ? room.image : TYPE_FALLBACKS['Single'];
        return `
          <li class="list-group-item d-flex justify-content-between align-items-center py-3">
            <div class="d-flex align-items-center gap-3">
              <img src="${roomImg}" class="rounded" style="width: 50px; height: 50px; object-fit: cover;" />
              <div>
                <h6 class="mb-0 fw-bold">${b.customer}</h6>
                <small class="text-muted">${room ? room.name : 'Unknown Room'} &bull; ${b.checkin} to ${b.checkout}</small>
              </div>
            </div>
            <button class="btn btn-sm btn-outline-danger" onclick="cancelBooking(${b.id})">Cancel</button>
          </li>
        `;
      }).join('') + `</ul>`;
    }

    function populateBookingModal() {
      const select = $('bookingRoomSelect');
      const availableRooms = getData(STORAGE.rooms).filter(r => r.status === 'available');
      select.innerHTML = availableRooms.length 
        ? availableRooms.map(r => `<option value="${r.id}">${r.name} (${r.type} - $${r.price}/night)</option>`).join('')
        : '<option value="">No available rooms</option>';
    }

    window.cancelBooking = function(id) {
      const booking = getData(STORAGE.bookings).find(b => b.id === id);
      if (booking) {
        let rooms = getData(STORAGE.rooms).map(r => r.id === booking.roomId ? { ...r, status: 'available' } : r);
        setData(STORAGE.rooms, rooms);
        setData(STORAGE.bookings, getData(STORAGE.bookings).filter(b => b.id !== id));
        refreshAll();
      }
    };

    function renderCustomers() {
      const customers = Array.from(new Set(getData(STORAGE.bookings).map(b => b.customer)));
      $('customerListContainer').innerHTML = customers.length 
        ? `<ul class="list-group">` + customers.map(c => `<li class="list-group-item"><i class="bi bi-person me-2"></i>${c}</li>`).join('') + `</ul>`
        : '<p class="text-muted">No guest records yet.</p>';
    }

    function refreshAll() {
      const rooms = getData(STORAGE.rooms);
      const bookings = getData(STORAGE.bookings);
      $('statTotalRooms').textContent = rooms.length;
      $('statAvailable').textContent = rooms.filter(r => r.status === 'available').length;
      $('statBookings').textContent = bookings.length;
      $('statCustomers').textContent = new Set(bookings.map(b => b.customer)).size;

      renderRooms();
      renderBookings();
      renderCustomers();
      populateBookingModal();
    }

    $('addRoomBtn').onclick = () => {
      $('roomForm').reset();
      $('roomId').value = '';
    };

    $('roomSaveBtn').onclick = () => {
      const id = $('roomId').value;
      const name = $('roomName').value.trim();
      const type = $('roomType').value;
      const price = Number($('roomPrice').value);
      const image = $('roomImage').value.trim() || TYPE_FALLBACKS[type];
      const status = $('roomStatus').value;

      let rooms = getData(STORAGE.rooms);
      if (id) {
        rooms = rooms.map(r => r.id === Number(id) ? { id: Number(id), name, type, price, image, status } : r);
      } else {
        rooms.push({ id: Date.now(), name, type, price, image, status });
      }

      setData(STORAGE.rooms, rooms);
      bootstrap.Modal.getInstance($('roomModal')).hide();
      refreshAll();
    };

    $('bookingSaveBtn').onclick = () => {
      const customer = $('bookingCustomer').value.trim();
      const roomId = Number($('bookingRoomSelect').value);
      const checkin = $('bookingCheckin').value;
      const checkout = $('bookingCheckout').value;

      if (!customer || !roomId || !checkin || !checkout) {
        alert('Please fill out all booking fields.');
        return;
      }

      let bookings = getData(STORAGE.bookings);
      bookings.push({ id: Date.now(), customer, roomId, checkin, checkout });
      setData(STORAGE.bookings, bookings);

      let rooms = getData(STORAGE.rooms).map(r => r.id === roomId ? { ...r, status: 'booked' } : r);
      setData(STORAGE.rooms, rooms);

      bootstrap.Modal.getInstance($('bookingModal')).hide();
      $('bookingForm').reset();
      refreshAll();
    };

    $('roomSearch').oninput = renderRooms;
    $('roomTypeFilter').onchange = renderRooms;

  })();
</script>
</body>
</html> 
