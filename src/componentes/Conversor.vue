<template>

  <section class="scr-componentes-conversor">
    <h2>Conversor a dólares</h2>

    <p>Ingrese monto $ : <input type="text" v-model="pesos" ></p>
    <p>Valor del dólar en $ : <input v-if="!actualizar" type="text" v-model="valorDolar" > <input v-else type="text" v-model="valorDolarActualizado" >  - Actualización <input type="checkbox" v-model="actualizar"></p> 
     

    <div >
        <p v-if="!actualizar" >Valor convertido: {{ pesos | toPesosAUSD(valorDolar) | currency('USD')  }}</p>
        <p v-else >Valor convertido: {{ pesos | toPesosAUSD(valorDolarActualizado) | currency('USD')  }}</p>
    </div>

     <p v-if="actualizar" class="fechaA">Fecha de Actualización: {{fechaActualizada}}</p>


      <h2>Resultados</h2>
      <br>

      <div class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Numero de Pregunta</th>
            <th>Resultado</th>
          </tr>
          <tr>
            <td>1</td>
            <td>C</td>
          </tr>
          <tr>
            <td>2</td>
            <td>B</td>
          </tr>
          <tr>
            <td>3</td>
            <td>C</td>
          </tr>
          <tr>
            <td>4</td>
            <td>A, B, D</td>
          </tr>
          <tr>
            <td>5</td>
            <td>A, B</td>
          </tr>
        </table>
      </div>

  </section>

</template>




<script>  
    export default  {
    name: 'scr-componentes-conversor',
    props: [],
    components: {
    },
    mounted () {
      this.cotizacionUSD()
    },
    
    filters : {
      toPesosAUSD : function(value, valorUSD) {
        return (value / valorUSD).toFixed(2)
      },
      currency: function(value,signo) {
        return signo + Number(value==''? 0 : value).toFixed(2)
      }
     
    },
    data () {
      return {
        pesos: 1000,
        valorDolar: 200,
        valor: 0,
        actualizar: false,
        valorDolarActualizado: 0,
        fechaActualizada: null
      }
    },
    methods: {
      async cotizacionUSD() {
          this.valorDolarActualizado = null
          this.fechaActualizada = null
          let { data:respuesta } = await this.axios('https://www.dolarsi.com/api/api.php?type=valoresprincipales')
          let cotizacion = Number(respuesta.find(dato => dato.casa.nombre == 'Dolar Blue').casa.venta.replace(',','.'))
          this.valorDolarActualizado = cotizacion.toFixed(2)
          this.fechaActualizada = new Date().toLocaleString()
          console.log(`Valor: ${this.valorDolarActualizado} - fecha ${this.fechaActualizada}` )
        
          setTimeout(() => {
            this.cotizacionUSD()
          },10000)
        }
    },
    computed: {
    }
}
</script>



<style scoped lang="css">
  .jumbotron {
    background-color: rgb(22, 87, 22);
    color: white;
  }
</style>