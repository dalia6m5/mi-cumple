<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  
    <div class="contador">

      <section class="d-flex  justify-content-center">

        <div class="dias datos relative">
         <div class="numeros dia">
          {{ c_dias }}
         </div> 
          <div class="label dia text-sm absolute bottom-0">Días</div>
        </div>
        <!-- <spam class="numeros leading-snug ">
          :
        </spam> -->

        <div class="horas datos relative">
          <div class="numeros hora">{{ c_horas }}</div>
          <div class="label hora text-sm absolute bottom-0">Horas</div>
        </div>
        <!-- <spam class="numeros leading-snug">
          :
        </spam> -->

        <div class="minutos datos relative">
          <div class="numeros minuto">{{ c_minutos }}</div>
          <div class="label minuto text-sm absolute bottom-0">Minutos</div>
        </div>
        <!-- <spam class="numeros leading-snug">
          :
        </spam> -->

        <div class="segundos datos relative">
          <div class="numeros segundo">{{ c_segundos }}</div> 
          <div class="label text-sm absolute segundo bottom-0">Segundos</div>
        </div>
       

      </section>

     

    </div>

    <button @click="showAlert" class="boton">Confirma Aquí</button>

  </div>
</template>

<script>
import Swal from 'sweetalert2';
//import axios from 'axios';


export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: ()=>({
    c_dias: 0,
    c_horas: 0,
    c_minutos: 0,
    c_segundos: 0

  }),

  computed: {
    _segundos: () => 1000,
    _minutos() {
      return this._segundos * 60
    },
    _horas() {
      return this._minutos * 60
    },
    _dias() {
      return this._horas * 24
    }

  },

  mounted(){

    this.mostrarTiempo()
  },

  methods:{

    mostrarTiempo() {

      const contador = setInterval(()=> {

        const now = new Date();
        const end = new Date(2023, 5, 3, 20, 0, 0);
        const distance = end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(contador);
          return;
        }

        const dias = Math.floor(distance / this._dias);
        const horas = Math.floor((distance % this._dias) / this._horas);
        const minutos = Math.floor((distance % this._horas) / this._minutos);
        const segundos = Math.floor((distance % this._minutos) / this._segundos);

        this.c_minutos = minutos < 10 ? "0" + minutos : minutos;
        this.c_segundos = segundos < 10 ? "0" + segundos : segundos;
        this.c_horas = horas < 10 ? "0" + horas : horas;
        this.c_dias = dias < 10 ? "0" + dias : dias;



      }, 1000

      );

    },

    showAlert: async function () {
     

  const { value: formValues } = await Swal.fire({
  title: 'Pon tu nombre para confirmar',
  showCancelButton: true,
 
  color: '#f8dd74',
  background: '#342b3a',
  backdrop: `
  rgba(202,0,123,0.4)
   url("https://media.giphy.com/media/cmzmn39EJRyLDF6l2j/giphy.gif")
   center top
   no-repeat
  
   `,
  html:
    '<input id="nombre" class="swal2-input">',
  focusConfirm: false,
  confirmButtonText:'Confirmo',
  confirmButtonColor:'#f8dd74',
  preConfirm: () => {
    return [
     this.nombre = document.getElementById('nombre').value
    ]
  }
})



if (formValues) {
  this.agregarInvitado();

  Swal.fire({
    title:'Yeeh, gracias, ' + this.nombre + '.' +  '\n Agenda aquí',
    showConfirmButton: false,
   
    color: '#88f471',
    background: '#342b3a',
    backdrop: `
  rgba(202,0,123,0.4)
  url("https://media.giphy.com/media/1SuFq8r3ys0n2AQH0V/giphy.gif")
   center top
   no-repeat
   
   `,
    html:
    '<a  target="_blank" href="https://calendar.google.com/calendar/event?action=TEMPLATE&amp;tmeid=NWNpODRlcjNrMXRvY21wbDhhdDY0a2plbTIgZGFsaWEuNjYubTVAbQ&amp;tmsrc=dalia.66.m5%40gmail.com"><img border="0" src="https://www.google.com/calendar/images/ext/gc_button1_es.gif"></a>'
  })
  
 
    
   



}
    },

    async agregarInvitado(){
      const nuevoInvitado ={
        nombre: this.nombre,
      }

      const URLPOST=  "https://63a3ca66471b38b206160d1c.mockapi.io/usuarios"
      const response = await fetch(URLPOST, {

          method:"POST",
          headers:{"Content-Type": "application/json"},
          body: JSON.stringify(nuevoInvitado)
      })

      const respuestaPost = response.json();
      console.log(respuestaPost)
      
    }

  }


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  @import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,800;0,900;1,100;1,200;1,800;1,900&display=swap');


body{
  font-family: 'Montserrat', sans-serif;
  
}

.hello{
  
  background-image:url(../assets/Recurso\ 1FALTAN.png);
    background-position:top;
    background-size: 85%;
    background-repeat: no-repeat;


    
}


.contador {
  display: flex;
  align-items: center;
  justify-content: center;
  
  margin-top: 100px;
  margin-bottom: 20px;
  
}

.datos{
  margin: 10px;
}

.numeros{
  font-weight: bold;
  font-size: 60px;
  text-shadow: 2px 2px 2px rgba(0, 0, 0, 0.25);

}

.dia {
  color: #f8dd74;
 
}

.hora {
  color: #88f471;
  
}

.minuto {
  color: #ffb6dc;
  
}

.segundo {
  color: #9bf0f9;
  
}

.label {
  -webkit-text-stroke: 1px;
  font-size: 13px;
  text-shadow: 1px 1px 1px rgb(43, 43, 43, 0.6);
}

.boton{
  background-color: #ffb6dc;
  border: none;
  color: white;
  -webkit-text-stroke: 1px;
  padding: 5px 10px 5px 10px ;
  font-size: 13px;
}

.boton:hover {
  background-color: #fff3f9;
  color: #ff8ec8;
}





@media (min-width: 768px){
  .numeros{
  font-weight: bold;
  font-size: 85px;

  }

  .datos{
  margin: 20px;
  }

  .boton{
  
  font-size: 15px;
}

  .label {
  -webkit-text-stroke: 1.5px;
  font-size: 20px;
}
}

</style>
