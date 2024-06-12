<template>
  <div class="container mt-5">

    <header class="d-flex justify-content-between align-items-center mb-4 header">
      <div class="logo"></div>
      <div class="d-flex align-items-center hero">
        <span class="text">Benvenuto, Fabian Dumea</span>
        <div class="avatar mr-2"></div>
      </div>
    </header>
    
    <div class="jumbotron bg-light text-dark">
      <h1 class="display-4">Dispositivi Bluetooth</h1>
      <p class="lead">
        Questa pagina ti permette di visualizzare e tenere sotto controllo i dispositivi Bluetooth della tua azienda.
      </p>
    </div>

    <div class="mb-4">
      <label for="status-filter" class="mr-3">Filtra per stato:</label>
      <select id="status-filter" v-model="statusFilter" @change="filterDevices" class="form-control w-auto d-inline-block">
        <option value="">Tutti</option>
        <option value="connected">Connessi</option>
        <option value="disconnected">Non Connessi</option>
      </select>
      <label for="battery-filter" class="ml-3 mr-2">Livello minimo della batteria:</label>
      <input type="number" id="battery-filter" v-model="batteryFilter" @input="filterDevices" class="form-control w-auto d-inline-block" style="width: 100px;">
    </div>

    <div class="table-responsive">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Nome</th>
            <th>Brand</th>
            <th>Stato</th>
            <th>Livello di Batteria</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="device in filteredDevices" :key="device.name">
            <td>{{ device.name }}</td>
            <td>{{ device.brand }}</td>
            <td>{{ device.status }}</td>
            <td>{{ device.battery_level }}%</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      devices: [],
      filteredDevices: [],
      statusFilter: '',
      batteryFilter: 0,
    };
  },
  methods: {
    fetchDevices() {
      axios
        .get('https://run.mocky.io/v3/c2a9174e-b6df-4951-8ee6-c02e31212734')
        .then((response) => {
          this.devices = response.data;
          this.filteredDevices = this.devices;
        });
    },
    filterDevices() {
      this.filteredDevices = this.devices.filter((device) => {
        return (
          (this.statusFilter === '' || device.status === this.statusFilter) &&
          device.battery_level >= this.batteryFilter
        );
      });
    },
  },
  mounted() {
    this.fetchDevices();
  },
};
</script>

<style scoped>
body {
  font-family: 'Verdana', sans-serif;
}

.header {
  background-color: #f8f9fa;
  padding: 10px 0;
  border-radius: 10px;
}

.logo {
  width: 90px;
  height: 90px;
  background-image: url('assets/original_logo.png');
  background-size: contain;
  background-repeat: no-repeat;
  transition: transform 0.2s;
}

.logo:hover {
  transform: scale(1.1);
}


.hero {
  padding-right: 30px;
  transition: transform 0.2s;
}
.hero:hover {
  transform: scale(1.1);
}

.text{
  padding-right: 20px;
}


.avatar {
  width: 50px;
  height: 50px;
  background-image: url('assets/nnpc.png');
  background-size: cover;
  border-radius: 50%;
}


.jumbotron {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

label {
  font-weight: bold;
}

select, input[type="number"] {
  border-radius: 5px;
}

.table-responsive {
  overflow-x: auto;
}

.table th, .table td {
  vertical-align: middle;
}
</style>
