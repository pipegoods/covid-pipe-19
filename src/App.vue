<template>
  <div id="app">

  <nav>
    <div class="nav-wrapper #26a69a teal lighten-1">
      <a href="#" class="brand-logo center">PIPE-19 APP</a>
    </div>
  </nav>

<div class="container">
  <div class="row">
    <div class="col s12 m7 l4">
      <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://fondosmil.com/fondo/9934.jpg">
        </div>
        <div class="card-content">
           <h5>Total casos confirmados</h5>
            <h6><b>{{formatoNumero(totalConfirmados)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
    <div class="col s12 m7 l4">
      <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://i.pinimg.com/originals/4b/a0/39/4ba0396c1100dc774d874974f058bf1d.jpg">
        </div>
        <div class="card-content">
           <h5>Total recuperados</h5>
            <h6><b>{{formatoNumero(totalRecuperados)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
    <div class="col s12 m7 l4">
      

  <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://fondosmil.com/fondo/3043.jpg">
         
        </div>
        <div class="card-content">
            <h5>Total muertos</h5>
            <h6><b>{{formatoNumero(totalMuertos)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
  </div>

  <div class="divider"></div>
 <div>
    <h5 class="center-align">En colombia</h5>



    <div class="row">
    <div class="col s12 m7 l4">
      <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://fondosmil.com/fondo/9934.jpg">
        </div>
        <div class="card-content">
           <h5>Total casos confirmados</h5>
            <h6><b>{{formatoNumero(colConfirmados)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
    <div class="col s12 m7 l4">
      <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://i.pinimg.com/originals/4b/a0/39/4ba0396c1100dc774d874974f058bf1d.jpg">
        </div>
        <div class="card-content">
           <h5>Total recuperados</h5>
            <h6><b>{{formatoNumero(colRecuperados)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
    <div class="col s12 m7 l4">
      

  <div class="row">
    <div class="col s12 m7">
      <div class="card">
        <div class="card-image">
          <img src="https://fondosmil.com/fondo/3043.jpg">
         
        </div>
        <div class="card-content">
            <h5>Total muertos</h5>
            <h6><b>{{formatoNumero(colMuertos)}}</b></h6>
        </div>
      </div>
    </div>
  </div>
    </div>
  </div>


  </div>
  

</div>


  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',
  data : function () {
    return {
    totalConfirmados: 1,
    totalMuertos: 1,
    totalRecuperados: 1,
    colConfirmados: 1,
    colMuertos: 1,
    colRecuperados: 1,
    fechaHoy: '',
    fechaAyer: ''
    }
  },
  created () {
    var dateObj = new Date(); 
    dateObj.setHours(0,0,0,0);
    dateObj.setUTCHours(0);
    // subtract one day from current time                           
    dateObj.setDate(dateObj.getDate() - 1);  
    this.fechaAyer = dateObj.toISOString();
    var today = new Date();
    today.setHours(0,0,0,0);
    today.setUTCHours(0);
    this.fechaHoy = today.toISOString();

    this.fecth();
    this.fecthCol();

  },
  methods: {
    fecth () {
       
      let result = axios.get("https://api.covid19api.com/summary")
        .then((res) => {
          this.totalConfirmados = res.data.Global.TotalConfirmed;
          this.totalMuertos = res.data.Global.TotalDeaths;
          this.totalRecuperados = res.data.Global.TotalRecovered;
          // console.log(res.data.Global);
        })
        .catch((err) => {
          console.log(err);
        });
      return result;
    },
    fecthCol() {
      const params = {
        from: this.fechaAyer,
        to: this.fechaHoy
      }

      let result = axios.get("https://api.covid19api.com/country/colombia", {params})
      .then((res) => {
          this.colConfirmados = res.data[0].Confirmed;
          this.colMuertos = res.data[0].Deaths;
          this.colRecuperados = res.data[0].Recovered;
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
      return result;
    },
    formatoNumero (numero) {
      const spanishNumberFormatter = new Intl.NumberFormat("es-ES");
      return spanishNumberFormatter.format(numero);
    }
  }
}
</script>

