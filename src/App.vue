
<template>
  <div id="body">
    <div id="arriba">
      <h1 id="titulo">Nintendo </h1>
      <img id="fotol" src="./img/logo.png" alt="">
      <h1 id="Estas">¿Estas buscando algo en especifico?</h1>
      <input id="barra" type="text" v-model="terminoBusqueda">
      <div id="boto">
        <button id="botona">👥</button>
        <button @click="showModal = true" id="botona">🛒</button>
        <div>
          <transition name="fade">
            <div class="modal-overlay" v-show="showModal"></div>
          </transition>
          <transition name="fade">
            <div class="modal" v-show="showModal">
              <h1 id="R-E">Registro de carrito</h1>
              <h1 id="total" tyle="font-weight: bolder; font-size: 20px;">Total a pagar:</h1>
              <h1 id="totalP">${{ (total).toLocaleString("es-ES", {
                style: "currency",
                currency: "COP",
                minimumFractionDigits: 0,
                maximumFractionDigits: 2,
              }) }}</h1>
              <table id="tabla">
                <tr>
                  <th>Nombre</th>
                  <th>Precio</th>
                  <th>Cantidad</th>
                  <th>Portada del juego</th>
                  <th>Eliminar del carrito</th>
                </tr>
                <tbody>
                  <tr v-for="(cosas, i) in cp" :key="i">
                    <td>{{ cosas.nombre }}</td>
                    <td>${{ (cosas.precio).toLocaleString("es-ES", {
                      style: "currency",
                      currency: "COP",
                      minimumFractionDigits: 0,
                      maximumFractionDigits: 2,
                    }) }}</td>
                    <td>{{ cosas.cantidadEnCarrito }}</td>
                    <td><img :src="cosas.img" id="imgcarro" alt="Imagen"></td>
                    <td> <button @click="eminarP(i)" id="bec">🚮</button></td>
                  </tr>
                </tbody>
              </table>
              <button @click="showModal = false" id="botonx">❌</button>
            </div>
          </transition>
        </div>
      </div>
    </div>

    <div id="abajoc">
      <img id="Bowser" src="./img/Bowser.png" alt="">
      <img id="Mario" src="./img/Picachu.png" alt="">
      <img id="Luigi" src="./img/Luigui.png" alt="">
      <img id="Toad" src="./img/Toad.png" alt="">
      <img id="cazul" src="./img/control_azul.png" alt="">
      <img id="crojo" src="./img/control_rojo.png" alt="">
      <div id="abajo">
        <div id="ctarjetas" v-for="(tarjetas, i) in filtroTarjeta" :key="i">
          <img id="imagentarjeta" :src="tarjetas.img" alt="">
          <h1 id="info">Nombre:{{ tarjetas.nombre }}</h1>
          <h1 id="info2">Precio:${{ (tarjetas.precio).toLocaleString("es-ES", {
            style: "currency",
            currency: "COP",
            minimumFractionDigits: 0,
            maximumFractionDigits: 2,
          }) }}
          </h1>

          <button @click="añadirP(i)" id="carritob">Añadir a la tabla</button>

        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Barra busqueda
const terminoBusqueda = ref('');
const filtroTarjeta = computed(() => {
  return terminoBusqueda.value === ''
    ? tarjeta.value
    : tarjeta.value.filter(item => item.nombre.toLowerCase().includes(terminoBusqueda.value.toLowerCase()));
});

function añadirP(index) {
  const tarjetaSeleccionada = tarjeta.value[index]
  // Verificar si el tarjeta ya está en el carrito
  const tarjetaExistente = cp.value.find(p => p.id === tarjetaSeleccionada.id);

  if (tarjetaExistente) {
    // Si el tarjeta ya está en el carrito, aumentar su cantidad
    tarjetaExistente.cantidadEnCarrito += 1;
  } else {
    tarjetaSeleccionada.cantidadEnCarrito = 1;
    // Si el tarjeta no está en el carrito, añadirlo al carrito
    cp.value.push(tarjetaSeleccionada);
  }
}




const cp = ref([]);
function eminarP(i) {
  cp.value.splice(i, 1)
}

const total = computed(() => {
  return cp.value.reduce((total, tarjeta) => total + tarjeta.precio * tarjeta.cantidadEnCarrito, 0)
})

// Abrir y mostrar el modal
const showModal = ref(false);
const openModal = () => {
  showModal.value = true;
};
const closeModal = () => {
  showModal.value = false;
};
// Informacion tarjetas
const tarjeta = ref([
  { id: 1, nombre: "The legend of zelda breath of the wild", precio: 284900, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQuEFHOx4C2MbV8foj7GeMk533BPCEstVbT6Q&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 2, nombre: "The legend of zelda tears of the kindom", precio: 408900, img: "https://http2.mlstatic.com/D_NQ_NP_759278-MLB69890739138_062023-O.webp", cantidadEnCarrito: 1 },
  { id: 3, nombre: "Mario kart", precio: 279000, img: "https://exitocol.vtexassets.com/arquivos/ids/7512604/mario-kart-deluxe-8-nintendo-switch.jpg?v=637557703180930000", cantidadEnCarrito: 1 },
  { id: 4, nombre: "Mario party", precio: 240500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ1yAAfWEiYSz_d-iHdRQ3b-f_UoZtK4QTq-Q&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 5, nombre: "Super Smash Bros", precio: 120000, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS43_W2iA5X2A0914TWRyR2d09pTjxM8jhEfA&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 6, nombre: "Super Mario Odyssey", precio: 214900, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRIteQcWjLIAxVp-xJikgUsalUS_yMeucsqYg&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 7, nombre: "Pokemon Y", precio: 249990, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQlr02GYQwwCd7yvs5__roejO955evicNpHOiulSHcgPd3ZfeYLFTzWXWzjWQ6CsoZfbvY&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 8, nombre: "Pokemon X", precio: 249990, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSznYhyeiVAMApK2fgeRpWN7tPuttQoBNaPWNRmdwPnRisMqwlZC3FAXR_NnUCbxaBZZC4&usqp=CAU", cantidadEnCarrito: 1 },
  { id: 9, nombre: "Luigi's mansion", precio: 58400, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRrgLIQIFbU2l9QanJV7W5ApIIlSAYqKIXdJQ&usqp=CAU" },
  { id: 10, nombre: "Luigi's mansion2", precio: 337900, img: "https://i.ytimg.com/vi/xdy0vB7ELPI/sddefault.jpg" },
  { id: 11, nombre: "Donkey kong country tropical freeze", precio: 265900, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTpkGslg0Ggru4ytGSB9YhWCnZAzvAkKyhCXqlxwgsdyERodMBFxaJNZyj4k1DEfgZO3ic&usqp=CAU" },
  { id: 12, nombre: "Animal Crossing", precio: 265900, img: "https://media.cdn.videotesty.pl/media/cache/f5/42/f54260dfcf09ca0fb151337f5f0e5e80.jpg" },
])
</script>



<style scoped>
#body {
  height: 100vh;
}

#total {
  color: rgb(0, 0, 0);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 8mm;
  position: fixed;
  top: 30px;
}

#totalP {
  color: rgb(0, 0, 0);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 8mm;
  position: fixed;
  top: 65px;
}

#arriba {
  border: 3px solid black;
  /* background-color: rgb(85, 172, 253); */
  background-color: rgb(252, 53, 53);
  display: flex;
  justify-content: space-between;
  padding: 10px 20px;
  align-items: center;
}

#titulo {
  position: relative;
  left: 160px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: rgb(255, 255, 255);
  font-size: 10mm;
}

#fotol {
  width: 190px;
  height: 110px;
  position: relative;
  right: 300px;
}

#Estas {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 7mm;
  color: rgb(0, 0, 0);
  position: fixed;
  left: 500px;
  top: 10px;
}

#barra {
  width: 500px;
  height: 25px;
  position: fixed;
  left: 100px;
  top: 20px;
  border-radius: 20px;
}

/* Botones */
#boto {
  display: grid;
  gap: 10px;
  position: relative;
  right: 40px;
}

#botona {
  height: 50px;
  width: 100px;
  border: 3px solid black;
  font-size: 7mm;
  border-radius: 20px;
  background-color: rgb(255, 255, 255);
}

#botonx {
  height: 50px;
  width: 50px;
  border: 0px solid black;
  font-size: 7mm;
  border-radius: 10px;
  background-color: rgb(255, 255, 255);
  position: fixed;
  top: 5px;
}

#bec {
  background-color: red;
  border: 3px solid black;
  height: 50px;
  width: 80px;
  font-size: 2em;
  border-radius: 20px;
}

#carritob {
  background-color: rgba(85, 172, 253, 0.721);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  width: 150px;
  height: 35px;
  font-weight: bold;
  border-radius: 20PX;
  position: relative;
  left: 30PX;
}

#R-E {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  position: fixed;
  top: -10px;
  color: red;
}

#abajo {
  background-color: rgb(255, 255, 255);
  height: 400px;
  /* cambiar esta cosa para el tamaño de la switch */
  width: 900px;
  border: 3px solid black;
  border-radius: 40px;
  overflow-y: scroll;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 40px;
  padding: 30px;
  background-image: url(https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcTqfdO925Y_hMENx9ifLf2nBcLao4r5Lx8aKKSykxWhdIRDQ5fL);
  background-size: cover;
  background-repeat: no-repeat;

}

#abajo::-webkit-scrollbar {
  width: 0px;
  /* Ancho de la barra de desplazamiento */
}

#abajoc {
  display: flex;
  justify-content: center;
  position: relative;
  top: 110px;
  right: 500px;
  /* height: 300px; */
}

#Bowser {
  position: relative;
  left: 1100px;
  top: -100px;
  width: 100px;
  height: 100px;
  margin: 0%;
}

#Mario {
  /* background-position: 500px 150px; */
  position: relative;
  left: 1200px;
  top: -100px;
  width: 100px;
  height: 100px;
  margin: 0%;
}

#Luigi {
  position: relative;
  left: 1300px;
  top: -100px;
  width: 100px;
  height: 100px;
  margin: 0%;
}

#Toad {
  position: relative;
  left: 1400px;
  top: -100px;
  width: 100px;
  height: 100px;
  margin: 0%;
}

#cazul {
  position: relative;
  left: 392px;
  width: 320px;
}

#crojo {
  position: relative;
  left: 1129px;
  width: 290px;
}

#ctarjetas {
  width: 215px;
  height: 390px;
  border: 3px solid black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  top: 15px;
  border-radius: 20px;
  background-color: white;
}

#info {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 5mm;
  position: relative;
  top: -5px;
  left: 3px;
}

#info1 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 5mm;
  position: relative;
  top: -60px;
  left: 140px;
}

#info2 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 17px;
  position: relative;
  top: -5px;
  left: 3px;
}

#imagentarjeta {
  border-radius: 20px;
  position: relative;
  left: 37px;
  top: -5px;
  width: 140px;
  height: 150px;
}

.modal {
  background-color: rgb(255, 255, 255);
  border: 5px solid #000;
  border-radius: 10px;
  padding: 20px;
  position: fixed;
  top: 340px;
  left: 80%;
  transform: translate(-50%, -50%);
  z-index: 101;
  /* Asignar un valor más alto al modal osea que se muestre por encima de otra cosa */
  height: 400px;
  width: 600px;
  overflow-y: scroll;

}

.modal::-webkit-scrollbar {
  width: 0px;
  /* Ancho de la barra de desplazamiento (scroll)*/
}

.modal-overlay {
  /* background-color: rgb(0, 0, 0); */
  position: fixed;
  top: 0px;
  left: 150px;
  width: 50px;
  height: 50px;
}

/* Tabla etc */
#tabla {
  border-collapse: collapse;
  text-align: center;
  background-color: rgba(85, 172, 253, 0.721);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  position: fixed;
  top: 120px;
  right: 2px;
  margin-left: 30px;
  margin-right: 30px;


}

#tabla th,
#tabla td {
  border: 4px solid black;
  padding: 6px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-weight: bold;

}

#imgcarro {
  height: 110px;
  width: 100px;
  border-radius: 20px;
}

@media screen and (max-width:2077px) {
  #barra {
    position: fixed;
    left: 500px;
    top: 85px;
  }

  #Estas {

    position: fixed;
    left: 500px;
    top: 15px;
  }

  #fotol {
    position: fixed;
    left: 20px;
  }

  #abajoc {
    width: 1625px;
  }

  #crojo {
    position: relative;
    left: 850px;
  }

  #Bowser {
    position: relative;
    left: 1000px;
    top: -100px;
    width: 100px;
    height: 100px;
    margin: 0%;
  }

  #Mario {
    position: relative;
    left: 1100px;
    top: -100px;
    width: 100px;
    height: 100px;
    margin: 0%;
  }

  #Luigi {
    position: relative;
    left: 1200px;
    top: -100px;
    width: 100px;
    height: 100px;
    margin: 0%;
  }

  #Toad {
    position: relative;
    left: 1250px;
    top: -100px;
    width: 100px;
    height: 100px;
    margin: 0%;
  }

  .modal {
    position: fixed;
    left: 75%;
  }

  #botonx {
    position: fixed;
    left: 550px;
  }

}

@media screen and (max-width:1373px) {
  #abajoc {
    width: 1600px;
  }


  #crojo {
    position: relative;
    left: 825px;
  }

  #botonx {
    position: fixed;
    left: 550px;
  }

  #fotol {
    position: fixed;
    left: 20px;
  }

  #barra {
    position: fixed;
    left: 430px;
    top: 90px;
  }

  #Estas {
    position: fixed;
    left: 400px;
    top: 20px;
  }
}

@media screen and (max-width:1299px) {
  #fotol {
    position: relative;
    right: 210px;
  }

  #botonx {
    position: fixed;
    left: 550px;
  }

  .modal {
    position: fixed;
    left: 75%;
  }

  #abajoc {
    width: 1600px;
    height: 400px;
    position: fixed;
    right: 300px;
    top: 300px;
  }

  #abajo {
    height: 340px;
  }


  #crojo {
    position: relative;
    left: 824px;
  }

  #cazul {
    position: relative;
    left: 396px;
  }

  #fotol {
    position: fixed;
    left: 20px;
  }

  #barra {
    position: fixed;
    left: 430px;
    top: 90px;
  }

  #Estas {
    position: fixed;
    left: 400px;
    top: 20px;
  }
}</style>
