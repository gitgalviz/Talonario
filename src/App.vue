<template>
  <div class="app">
    <div class="cont_titulo" :style="{ backgroundColor: color }">
      <h1 class="text_titulo">Talonario</h1>
    </div>
    <div :class="Alerta()">
      <div class="cont_alerta">
        <p class="text_alerta">{{ alerta }}</p>
      </div>
    </div>
    <div class="container" v-if="RegistroTalonario">
      <div class="cont_form">
        <div class="form" :style="{ backgroundColor: color}">
          <div class="cont_titulo_form">
            <p class="text_titulo_form">Configurar talonario</p>
          </div>
          <div class="cont_text_premio">
            <input type="text" placeholder="Premio" v-model="premio">
          </div>
          <div class="cont_text_precio">
            <input type="text" placeholder="Valor boleta" v-model="Precio">
          </div>
          <div class="cont_tipos_loterias">
            <p class="text_tipos_loterias">Tipo de loteria</p>
            <select v-model="selectedLoteria">
              <option value="Santander">Santander</option>
              <option value="Boyaca">Boyaca</option>
              <option value="Bogota">Bogota</option>
              <option value="Dorado">Dorado</option>
              <option value="Medellin">Medellin</option>
            </select>
          </div>
          <div class="cont_num_boletas">
            <select v-model="selectedCantidad">
              <option value="25">25</option>
              <option value="50">50</option>
              <option value="75">75</option>
              <option value="100">100</option>
            </select>
          </div>
          <div class="cont_fecha">
            <input type="date" v-model="fecha">
          </div>
          <div class="cont_btn">
            <button class="btn" @click="crearTalonario()">Crear talonario</button>
          </div>
        </div>
      </div>
    </div>
    <div class="cont_forma_registro" v-if="RegistrarDueño">
      <div class="cont_form_registro" :style="{ backgroundColor: color}">
        <p class="nam_div_registro">Registro de boletas</p>
        <div class="cont_nombre">
          <input type="text" placeholder="Nombre" v-model="nombre">
        </div>
        <div class="cont_telefono">
          <input type="text" placeholder="Telefono" v-model="telefono">
        </div>
        <div class="cont_direccion">
          <input type="text" placeholder="Direccion" v-model="direccion">
        </div>
        <div class="cont_opciones_boleta">
          <p class="text_option">Pagar boleta?</p>
          <select v-model="selectedBoleta">
            <option value="Si">Si</option>
            <option value="No">No</option>
          </select>
        </div>
        <div class="cont_btn_registro">
          <button class="btn_registro" @click="registrar()">Registrar</button>
        </div>
      </div>
    </div>
    <div class="cuerpo_info" v-if="cuerpo">
      <div class="cont_info_body">
        <div class="cont_titulo_body">
          <p class="text_body">Información</p>
        </div>
        <div class="cont_info_loteria">
          <div class="cont_trofeo">
            <div class="cont_img_t">
              <img src="../img/trofeo.png" class="img_trofeo">
            </div>
            <div class="cont_text_t">
              <p class="text_trofeo">{{ premio_Verificado }}</p>
            </div>
          </div>
          <div class="cont_precio">
            <div class="cont_img_p">
              <img src="../img/dolar.png" class="img_dolar">
            </div>
            <div class="cont_text_p">
              <p class="text_precio">{{ Precio_Verificado }}</p>
            </div>
          </div>
          <div class="cont_loteria">
            <div class="cont_img_l">
              <img src="../img/museo.png" class="img_museo">
            </div>
            <div class="cont_text_l">
              <p class="text_loteria">{{ selectedLoteria_Verificado }}</p>
            </div>
          </div>
          <div class="cont_calendario">
            <div class="cont_img_c">
              <img src="../img/calendario.png" class="img_calendario">
            </div>
            <div class="cont_text_c">
              <p class="text_calendario">{{ fecha_Verificado }}</p>
            </div>
          </div>
          <div class="cont_btn_editar">
            <button class="btn_editar" @click="editarTalonario()">Editar</button>
          </div>
        </div>
      </div>
      <div class="conte_balotas">
        <div v-for="(boleta, i) in boleta" :key="i" :class="getBoletaColor(boleta.estado)">
          <div class="cont_balota" @click="comprarBoleta(i)">
            <div class="cont_num_balota">
              <p class="text_num_balota">{{ boleta.item }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="cont_acciones">
        <div class="cont_titulo_acciones">
          <p class="text_acciones">Acciones</p>
        </div>
        <div class="cont_info_acciones">
          <div class="cont_btn1">
            <button class="btn1">Estado</button>
          </div>
          <div class="cont_btn2">
            <button class="btn2" @click="listarBoletas()">Listar tus boletas</button>
          </div>
          <div class="cont_btn3">
            <button class="btn3" @click="personalizarColores()">Personalizar talonario</button>
          </div>
          <div class="cont_btn4">
            <button class="btn4" @click="imprimir()">Generar PDF</button>
          </div>
        </div>
      </div>
    </div>
    <div class="cont_balotas_compradas" v-if="balotasCompradas">
      <div>
        <table class="tabla_boletas">
          <tr>
            <th>Numero de boleta</th>
            <th>Nombre</th>
            <th>Telefono</th>
            <th>Direccion</th>
            <th>Estado de pago</th>
          </tr>
          <tr class="balotas_compradas" v-for="(boletasCompradas, i) in boletasCompradas" :key="i">
            <td>{{ boletasCompradas.numero }}</td>
            <td>{{ boletasCompradas.nombre }}</td>
            <td>{{ boletasCompradas.telefono }}</td>
            <td>{{ boletasCompradas.direccion }}</td>
            <td>{{ boletasCompradas.pago }}</td>
          </tr>
        </table>
        <button class="button_atras" @click="atras()">Regresar</button>
      </div>
    </div>
    <div class="cont_options_boletas" v-if="cont_options_boletas">
      <div class="options_boletas" :style="{ backgroundColor: color }">
        <div class="cont_btn_datos">
          <button @click="DatosDueño()">Información dueño</button>
        </div>
        <div class="cont_btn_pagar" v-if="pagarBoleta">
          <button @click="PagarBoleta()">Pagar boleta</button>
        </div>
        <div class="cont_btn_liberar">
          <button @click="liberarBalota()">Liberar balota</button>
        </div>
        <div class="cont_btn_atras">
          <button @click="atras2()">Regresar</button>
        </div>
      </div>
    </div>
    <div class="cont_datos_dueño" v-if="cont_datos_dueño">
      <div class="datos_dueño" :style="{ backgroundColor: color }">
        <div class="info_nombre">
          <p class="text_info_nombre">Nombre:</p>
          <p class="text_nombre">{{ dueño.nombre }}</p>
        </div>
        <div class="info_telefono">
          <p class="text_info_telefono">Telefono:</p>
          <p class="text_telefono">{{ dueño.telefono }}</p>
        </div>
        <div class="info_direccion">
          <p class="text_info_direccion">Direccion:</p>
          <p class="text_direccion">{{ dueño.direccion }}</p>
        </div>
        <div class="info_pago">
          <p class="text_info_pago">Estado</p>
          <p class="text_pago">{{ dueño.pago }}</p>
        </div>
        <div class="cont_btn_atras">
          <button @click="atras3()">Regresar</button>
        </div>
      </div>
    </div>
    <div class="cont_personalizar" v-if="contPersonalizar">
      <div class="sub_cont_personalizar"  :style="{ backgroundColor: color }">
        <div class="cont_input_color">
          <input type="color" v-model="color">
        </div>
        <div class="cont_btn_atras">
          <button @click="atras4()">Regresar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import jsPDF from 'jspdf';
import autoTable from 'jspdf-autotable';

jsPDF.autoTable = autoTable;

const premio = ref('');
const premio_Verificado = ref('');
const Precio = ref('');
const Precio_Verificado = ref('');
const selectedLoteria = ref('');
const selectedLoteria_Verificado = ref('');
const selectedCantidad = ref('');
const selectedBoleta = ref('');
const fecha = ref('');
const fecha_Verificado = ref('');
const nombre = ref('');
const telefono = ref('');
const direccion = ref('');
const pago = ref('');
const RegistroTalonario = ref(true);
const RegistrarDueño = ref(false);
const cuerpo = ref(false);
const boletaSeleccionada = ref(null);
const balotasCompradas = ref(false);
const estado = ref(0);
const cont_options_boletas = ref(false);
const pagarBoleta = ref(false);
const estadoBoleta = ref('');
const alerta = ref('');
const cont_datos_dueño = ref(false);
const id = ref(0);
let i = ref(0);
let color = ref("#ccc")
const contPersonalizar = ref(false)

const boleta = ref([]);
const boletasCompradas = ref([]);
const dueño = ref([]);

const ocultarAlerta = () => {
  setTimeout(() => {
    alerta.value = '';
    Alerta();
  }, 2500);
};

const Alerta = () => {
  if (alerta.value !== '') {
    return 'alerta2';
  }
  else {
    return 'alerta';
  }
};

const crearTalonario = () => {
  if(premio.value === '' || Precio.value === '' || selectedLoteria.value === '' || selectedCantidad.value === '' || fecha.value === '') {
    alerta.value = 'Todos los campos son obligatorios';
    ocultarAlerta();
    return;
  }
  else {
    premio_Verificado.value = premio.value;
    Precio_Verificado.value = Precio.value;
    selectedLoteria_Verificado.value = selectedLoteria.value;
    fecha_Verificado.value = fecha.value;
    const cantidad = selectedCantidad.value;
  
    for (let i = 0; i < cantidad; i++) {
      boleta.value.push({
        item: i + 1,
        estado: estado.value,
      });
    }
    RegistroTalonario.value = false;
    cuerpo.value = true;
  }
};

const comprarBoleta = (index) => {
  boletaSeleccionada.value = index;
  if (boleta.value[index].estado === 0) {
    RegistrarDueño.value = true;
    cuerpo.value = false;
  }
  else if (boleta.value[index].estado === 1) {
    cont_options_boletas.value = true;
    pagarBoleta.value = false;
    cuerpo.value = false;
    i = boletasCompradas.value.findIndex((boletaComprada) => {
      return boletaComprada.numero === boleta.value[index].item;
    });
  }
  else if (boleta.value[index].estado === 2) {
    cont_options_boletas.value = true;
    pagarBoleta.value = true;
    cuerpo.value = false;
    i = boletasCompradas.value.findIndex((boletaComprada) => {
      return boletaComprada.numero === boleta.value[index].item;
    });
  }
};

const registrar = () => {
  if (nombre.value === '' || telefono.value === '' || direccion.value === '' || selectedBoleta.value === '') {
    alerta.value = 'Todos los campos son obligatorios';
    ocultarAlerta();
    return;
  }
  else{
    if (selectedBoleta.value === 'Si') {
      estado.value = 1;
      estadoBoleta.value = "Pagada";
    }
    else {
      estado.value = 2;
      estadoBoleta.value = "No pagada";
    }
    boletasCompradas.value.push({
      id: id.value++,
      numero: boleta.value[boletaSeleccionada.value].item,
      nombre: nombre.value,
      telefono: telefono.value,
      direccion: direccion.value,
      pago: estadoBoleta.value,
    });
    boleta.value[boletaSeleccionada.value].estado = estado.value;
    nombre.value = '';
    telefono.value = '';
    direccion.value = '';
    pago.value = '';
    selectedBoleta.value = '';
    estado.value = 0;
    RegistrarDueño.value = false;
    cuerpo.value = true;
  }
};

const DatosDueño = () => {
  cont_datos_dueño.value = true;
  cont_options_boletas.value = false;
  cuerpo.value = false;

  dueño.value = {
    nombre: boletasCompradas.value[i].nombre,
    telefono: boletasCompradas.value[i].telefono,
    direccion: boletasCompradas.value[i].direccion,
    pago: boletasCompradas.value[i].pago,
  };
};


const PagarBoleta = () => {
  boleta.value[boletaSeleccionada.value].estado = 1;
  cont_options_boletas.value = false;
  cuerpo.value = true;
};

const liberarBalota = () => {
  const selectedBoletaIndex = boletaSeleccionada.value;
  boleta.value[selectedBoletaIndex].estado = 0;
  cont_options_boletas.value = false;
  cuerpo.value = true;

  boletasCompradas.value = boletasCompradas.value.filter((boletaComprada) => {
    return boletaComprada.numero !== boleta.value[selectedBoletaIndex].item;
  });
};

const atras = () => {
  balotasCompradas.value = false;
  cuerpo.value = true;
};  

const atras2 = () => {
  cont_options_boletas.value = false;
  cuerpo.value = true;
};

const atras3 = () => {
  cont_datos_dueño.value = false;
  cuerpo.value = true;
};

const atras4 = () => {
  contPersonalizar.value = false;
  cuerpo.value = true;
};

const getBoletaColor = (estado) => {
  switch (estado) {
    case 0:
      return 'ClaseInicial';
    case 1:
      return 'ClasePagada';
    case 2:
      return 'ClaseNoPagada';
  }
};

const editarTalonario = () => {
  premio.value = '';
  Precio.value = '';
  selectedLoteria.value = '';
  selectedCantidad.value = '';
  fecha.value = '';
  RegistroTalonario.value = true;
  cuerpo.value = false;
  boleta.value = [];
};

const listarBoletas = () => {
  balotasCompradas.value = true;
  cuerpo.value = false;
};

const personalizarColores = () => {
  cuerpo.value = false;
  contPersonalizar.value = true;
};

const imprimir = () => {
  const doc = new jsPDF();

  doc.setFontSize(12);
  doc.text("Resumen de boletas vendidas", 10, 10);

  const tableData = boletasCompradas.value.map((boleta, index) => [
    boleta.numero,
    boleta.nombre,
    boleta.telefono,
    boleta.direccion,
    boleta.pago,
  ]);

  doc.autoTable({
    head: [["Boleta", "Nombre", "Teléfono", "Dirección", "Pago"]],
    body: tableData,
    startY: 20,
  });

  const totalBoletas = boletasCompradas.value.length;
  doc.text(`Total de balotas compradas: ${totalBoletas}`, 10, doc.autoTable.previous.finalY + 10);

  doc.save("vendidas.pdf");
};
</script>

<style scoped>
.app {
  display: grid;
  grid-template-rows: 10vh, 80vh, 10vh;
}

.text_titulo {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 50px;
  font-family: Arial;
  color: rgb(0, 0, 0);
}

.alerta {
  display: none;
}

.alerta2{
  position: absolute;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #00000095;
}

.cont_alerta {
  width: 30%;
  height: 25vh;
  background-color: #ccc;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
  font-weight: bold;
  padding: 8px 1%;
  text-align: center;
}

.cont_form_registro {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  padding: 20px;
}

.cont_forma_registro {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
}

.nam_div_registro {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
}

.cont_form_registro input[type="text"],
.cont_form_registro button {
  background-color: white;
  padding: 10px;
  margin: 5px 0;
  border: none;
  border-radius: 5px;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
}

.cont_form {
  width: 300px;
}

.form {
  padding: 20px;
  border-radius: 5px;
}

.text_titulo_form {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

input[type="text"],
select,
input[type="date"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.btn {
  background-color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn:hover {
  background-color: #2aff34;
}


.cont_form_registro {
  width: 300px;
  margin: 0 auto;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}


.btn_registro {
  background-color: #4caf50;
  color: rgb(0, 0, 0);
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}


.btn_registro:hover {
  background-color: #1aff35;
}


.cont_nombre,
.cont_telefono,
.cont_direccion,
.cont_identificacion,
.cont_btn_registro {
  margin-bottom: 15px;
}

/* acciones y informacion */
/* Estilos para la sección de Información */

.cuerpo_info {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  width: 100%;
}

.cont_info_body {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-right: 20px;
  width: 20%;
}

.cont_info_loteria,
.cont_btn_editar {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
  width: 100%;
  height: auto;
}

.cont_titulo_body {
  border-bottom: 1px solid #ccc;
  margin-bottom: 15px;
}

.text_body {
  font-size: 20px;
  font-weight: bold;
  color: #333;
}

.cont_acciones {
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 20%;
}

.cont_titulo_acciones {
  border-bottom: 1px solid #ccc;
  margin-bottom: 15px;
}

.text_acciones {
  font-size: 20px;
  font-weight: bold;
  color: #333;
}

.btn_editar {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #ccc;
  color: #000000;
  width: 200px;
  margin-right: 30px;

}

.btn1,
.btn2,
.btn3,
.btn4 {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #ccc;
  color: #000000;
  width: 200px;
  margin-right: 30px;
  margin-bottom: 10px;
}

.btn_editar:hover,
.btn1:hover,
.btn2:hover,
.btn3:hover,
.btn4:hover {
  background-color: #1aff35;
}

.cont_trofeo,
.cont_precio,
.cont_loteria,
.cont_calendario {
  display: flex;
  align-items: center;
  margin-bottom: 20px;

}

.cont_img_t,
.cont_img_p,
.cont_img_l,
.cont_img_c {
  width: 50px;
  margin-right: 10px;
}

.img_trofeo,
.img_dolar,
.img_museo,
.img_calendario {
  max-width: 100%;
}

.cont_text_t,
.cont_text_p,
.cont_text_l,
.cont_text_c {
  flex: 1;
}

.conte_balotas {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
}

.text_num_balota {
  font-size: 16px;
  color: #fff;
  margin: 0;
}

/* diseño de tablas de balotas */
.tabla_boletas {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
  font-family: Arial, sans-serif;
}

.tabla_boletas th,
.tabla_boletas td {
  border: 1px solid #dddddd;
  padding: 10px;
  text-align: left;
}

.tabla_boletas th {
  background-color: #4CAF50;
  color: white;
}

.tabla_boletas tr:nth-child(even) {
  background-color: #f2f2f2;
}

.tabla_boletas tr:hover {
  background-color: #ddd;
}

.button_atras {
  background-color: #4CAF50;
  color: white;
  margin-top: 20px;
  margin-bottom: 20px;
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.button_atras:hover {
  background-color: #45a049;
}

.ClaseInicial {
  background-color: #007bff;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  cursor: pointer;
}

.ClasePagada {
  background-color: #28a745;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  cursor: pointer;
}

.ClaseNoPagada {
  background-color: #dc3545;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  cursor: pointer;
}

.ClaseInicial:hover,
.ClasePagada:hover,
.ClaseNoPagada:hover {
  transform: scale(1.1);
}

.cont_options_boletas {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
  width: 100%;
}

.options_boletas {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  padding: 20px;
}

.cont_btn_datos,
.cont_btn_pagar,
.cont_btn_liberar {
  margin-bottom: 15px;
}

.cont_btn_datos button,
.cont_btn_pagar button,
.cont_btn_liberar button,
.cont_btn_atras button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #ffffff;
  color: #000000;
  width: 200px;
}

.cont_btn_datos button:hover,
.cont_btn_pagar button:hover,
.cont_btn_liberar button:hover,
.cont_btn_atras button:hover {
  background-color: #1aff35;
} 

.cont_datos_dueño {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
  width: 100%;
}

.datos_dueño {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  padding: 20px;
}

.info_nombre,
.info_telefono,
.info_direccion,
.info_pago {
  margin-bottom: 15px;
  display: flex;
}

.text_info_nombre,
.text_info_telefono,
.text_info_direccion,
.text_info_pago {
  font-size: 20px;
  font-weight: bold;
  margin-right: 10px;
}

.cont_personalizar{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90vh;
  width: 100%;
}

.sub_cont_personalizar{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 20%;
  height: 20vh;
}

.cont_input_color{
  width: 100%;
  padding: 0 2%;
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}

.cont_input_color input{
  width: 90%;
  border: none;
  border-radius: 8px;
  height: 10vh;
}
</style>