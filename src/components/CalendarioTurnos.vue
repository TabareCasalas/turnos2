<template>
  <div class="max-w-6xl mx-auto p-4 grid grid-cols-1 md:grid-cols-3 gap-4"
       @touchstart="handleTouchStart"
       @touchmove="handleTouchMove"
       @touchend="handleTouchEnd">
    <div class="md:col-span-2">
      <div class="flex justify-center items-center gap-1 mb-4 flex-wrap text-sm sm:text-base">
        <button @click="anioAnterior" class="w-9 h-9 sm:w-10 sm:h-10 flex items-center justify-center bg-gray-300 rounded hover:bg-gray-400">««</button>
        <button @click="mesAnterior" class="w-9 h-9 sm:w-10 sm:h-10 flex items-center justify-center bg-gray-300 rounded hover:bg-gray-400">«</button>
        <span class="text-base sm:text-lg font-semibold text-center px-2 w-[155px] text-ellipsis overflow-hidden whitespace-nowrap">{{ mesNombre }} {{ anio }}</span>
        <button @click="mesSiguiente" class="w-9 h-9 sm:w--10 sm:h-10 flex items-center justify-center bg-gray-300 rounded hover:bg-gray-400">»</button>
        <button @click="anioSiguiente" class="w-9 h-9 sm:w-10 sm:h-10 flex items-center justify-center bg-gray-300 rounded hover:bg-gray-400">»»</button>
      </div>

      <transition :name="transitionName" mode="out-in">
        <div :key="anio + '-' + mes">
          <div class="grid grid-cols-7 gap-2 text-center text-sm font-semibold">
            <div v-for="(dia, index) in diasSemana" :key="index" class="text-gray-600">
              {{ dia }}
            </div>
          </div>

          <div class="grid grid-cols-7 gap-2 mt-2 text-center">
            <div v-for="i in primerDiaMes" :key="'vacio-' + i"></div>

            <div
              v-for="dia in diasEnMes"
              :key="dia"
              class="dia-calendario py-2 rounded-lg font-bold cursor-pointer relative"
              :class="[
                getClaseTurno(dia),
                esHoy(dia) ? 'es-hoy' : '',
                diaSeleccionado === dia ? 'ring-2 ring-violet-400' : '',
                esFeriado(anio, mes, dia) ? 'border-2 border-black' : ''
              ]"
              :aria-current="esHoy(dia) ? 'date' : null"
              :aria-label="esHoy(dia) ? 'Hoy, ' + dia + ' de ' + mesNombre + ' de ' + anio : null"
              @click="seleccionarDia(dia)"
            >
              <span v-if="esHoy(dia)" class="etiqueta-hoy" aria-hidden="true">HOY</span>
              <span class="numero-dia">{{ dia }}</span>
            </div>
          </div>
        </div>
      </transition>
    </div>

    <div>
<div class="mt-6 text-sm">
        <ul class="space-y-1">
          <li><span class="inline-block w-4 h-4 bg-red-300 rounded mr-2"></span>Primer turno (22:00 a 06:00)</li>
          <li><span class="inline-block w-4 h-4 bg-green-300 rounded mr-2"></span>Segundo turno (06:00 a 14:00)</li>
          <li><span class="inline-block w-4 h-4 bg-blue-300 rounded mr-2"></span>Tercer turno (14:00 a 22:00)</li>
          <li><span class="inline-block w-4 h-4 border border-gray-300 rounded mr-2"></span>Día libre</li>
          <li><span class="inline-block w-4 h-4 border-2 border-black rounded mr-2"></span>Feriado</li>
        </ul>
      </div>

      <div v-if="diaSeleccionado !== null" class="mt-6 text-sm border-t pt-4">

        <p><strong>Fecha:</strong> {{ diaSeleccionado }}/{{ mes + 1 }}/{{ anio }}</p>
        <p><strong>Turno:</strong> {{ getDescripcionTurno(diaSeleccionado) }}</p>
        <p v-if="esFeriado(anio, mes, diaSeleccionado)"><strong>Feriado:</strong> {{ esFeriado(anio, mes, diaSeleccionado) }}</p>
        <p><strong>Releva a:</strong> Producción {{ getProduccionRelevada(diaSeleccionado) }}</p>
        <p><strong>Es relevado por:</strong> Producción {{ getProduccionQueReleva(diaSeleccionado) }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.slide-left-enter-active, .slide-left-leave-active,
.slide-right-enter-active, .slide-right-leave-active {
  transition: all 0.1s ease;
  position: relative;
}
.slide-left-enter-from {
  transform: translateX(100%);
  opacity: 0;
}
.slide-left-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}
.slide-right-enter-from {
  transform: translateX(-100%);
  opacity: 0;
}
.slide-right-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
.bg-red-300 { background-color: #f87171; }
.bg-green-300 { background-color: #4ade80; }
.bg-blue-300 { background-color: #60a5fa; }

/* Todos los días tienen la misma altura, incluso el que lleva la etiqueta. */
.dia-calendario {
  min-height: 3.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.5rem 0.125rem;
}
.etiqueta-hoy {
  position: absolute;
  top: 3px;
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none;
  border-radius: 0.3rem;
  padding: 1px 0.3rem;
  background: #172554;
  color: #fff;
  font-size: 0.6rem;
  font-weight: 800;
  letter-spacing: 0.035em;
  line-height: 1rem;
}
.es-hoy .numero-dia {
  transform: translateY(0.45rem);
}
</style>


<script>
function calcularPascua(anio) {
  const f = Math.floor,
        G = anio % 19,
        C = f(anio / 100),
        H = (C - f(C / 4) - f((8 * C + 13) / 25) + 19 * G + 15) % 30,
        I = H - f(H / 28) * (1 - f(29 / (H + 1)) * f((21 - G) / 11)),
        J = (anio + f(anio / 4) + I + 2 - C + f(C / 4)) % 7,
        L = I - J,
        mes = 3 + f((L + 40) / 44),
        dia = L + 28 - 31 * f(mes / 4);
  return new Date(anio, mes - 1, dia);
}

function obtenerFeriados(anio) {
  const pascua = calcularPascua(anio);

  const semanaSanta = [];
  const nombresSemanaSanta = [
    'Domingo de Ramos',
    'Lunes Santo',
    'Martes Santo',
    'Miércoles Santo',
    'Jueves Santo',
    'Viernes Santo',
    'Sábado Santo'
  ];

  for (let i = 0; i < 7; i++) {
    const dia = new Date(pascua);
    dia.setDate(pascua.getDate() - (7 - i));
    semanaSanta.push({ fecha: dia, nombre: nombresSemanaSanta[i] });
  }

  const carnavalLunes = new Date(pascua);
  carnavalLunes.setDate(pascua.getDate() - 48);
  const carnavalMartes = new Date(pascua);
  carnavalMartes.setDate(pascua.getDate() - 47);

  const feriadosFijos = [
    ['01-01', 'Año Nuevo'],
    ['01-06', 'Día de Reyes'],
    ['05-01', 'Día de los Trabajadores'],
    ['05-18', 'Batalla de las Piedras'],
    ['06-19', 'Natalicio de Artigas'],
    ['07-18', 'Jura de la Constitución'],
    ['08-25', 'Declaratoria de la Independencia'],
    ['10-12', 'Día de la Raza'],
    ['11-02', 'Día de los Difuntos'],
    ['12-25', 'Navidad']
  ];

  const feriados = {};
  for (const [md, nombre] of feriadosFijos) {
    feriados[`${anio}-${md}`] = nombre;
  }
  feriados[carnavalLunes.toISOString().split('T')[0]] = 'Carnaval';
  feriados[carnavalMartes.toISOString().split('T')[0]] = 'Carnaval';

  semanaSanta.forEach(({ fecha, nombre }) => {
    feriados[fecha.toISOString().split('T')[0]] = nombre;
  });

  return feriados;
}


export default {
  name: 'CalendarioTurnos',
  props: {
    produccion: Number
  },
  data() {
    const hoy = new Date()
    return {
      mes: hoy.getMonth(),
      anio: hoy.getFullYear(),
      diaSeleccionado: null,
      diasSemana: ['Dom', 'Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb'],
      inicioCicloPorProduccion: {
        1: new Date(2001, 9, 7),
        2: new Date(2001, 10, 3),
        3: new Date(2001, 10, 30),
        4: new Date(2001, 11, 27)
      },
      ciclo: [
        1,1,1,1,1,0,0,3,3,3,3,3,0,0,
        2,2,2,2,2,0,0,1,1,1,1,1,1,0,0,
        3,3,3,3,3,0,0,2,2,2,2,2,0,0,
        1,1,1,1,1,0,0,3,3,3,3,3,3,0,0,
        2,2,2,2,2,0,0,1,1,1,1,1,0,0,
        3,3,3,3,3,0,0,2,2,2,2,2,2,0,
        1,1,1,1,1,1,0,3,3,3,3,3,3,0,
        2,2,2,2,2,2,0,0
      ],
      touchStartX: 0,
      touchDeltaX: 0,
      transitionName: 'slide-left',
      feriadosPorAnio: {}
    }
  },
  computed: {
    diasEnMes() {
      return Array.from(
        { length: new Date(this.anio, this.mes + 1, 0).getDate() },
        (_, i) => i + 1
      )
    },
    primerDiaMes() {
      return new Date(this.anio, this.mes, 1).getDay()
    },
    mesNombre() {
      return new Date(this.anio, this.mes).toLocaleString('es-ES', { month: 'long' })
    }
  },
  methods: { 
    esHoy(dia) {
      const hoy = new Date()
      return hoy.getDate() === dia && hoy.getMonth() === this.mes && hoy.getFullYear() === this.anio
    },
    esFeriado(anio, mes, dia) {
      if (!this.feriadosPorAnio[anio]) {
        this.feriadosPorAnio[anio] = obtenerFeriados(anio);
      }
      const fecha = `${anio}-${String(mes + 1).padStart(2, '0')}-${String(dia).padStart(2, '0')}`;
      return this.feriadosPorAnio[anio][fecha];
    },
    handleTouchStart(e) {
      this.touchStartX = e.changedTouches[0].clientX;
    },
    handleTouchMove(e) {
      this.touchDeltaX = e.changedTouches[0].clientX - this.touchStartX;
    },
    handleTouchEnd() {
      const diff = this.touchDeltaX;
      this.touchDeltaX = 0;
      if (Math.abs(diff) > 50) {
        if (diff > 0) {
          this.transitionName = 'slide-right';
          this.mesAnterior();
        } else {
          this.transitionName = 'slide-left';
          this.mesSiguiente();
        }
      }
    },
    getClaseTurno(dia) {
      const fecha = new Date(this.anio, this.mes, dia)
      const turno = this.getTurno(fecha, this.produccion)

      switch (turno) {
        case 1: return 'bg-red-500 text-white'
        case 2: return 'bg-green-500 text-white'
        case 3: return 'bg-blue-500 text-white'
        default: return 'text-black border bg-white'
      }
    },
    getTurno(fecha, prod) {
      const inicio = this.inicioCicloPorProduccion[prod]
      const diasTranscurridos = Math.floor((fecha - inicio) / (1000 * 60 * 60 * 24))
      const posicion = ((diasTranscurridos % 108) + 108) % 108
      return this.ciclo[posicion] ?? 0
    },
    getDescripcionTurno(dia) {
      const fecha = new Date(this.anio, this.mes, dia)
      const turno = this.getTurno(fecha, this.produccion)
      switch (turno) {
        case 1: return 'Primer turno (22:00 a 06:00)'
        case 2: return 'Segundo turno (06:00 a 14:00)'
        case 3: return 'Tercer turno (14:00 a 22:00)'
        default: return 'Día libre'
      }
    },
    getProduccionRelevada(dia) {
      const fecha = new Date(this.anio, this.mes, dia)
      const turnoActual = this.getTurno(fecha, this.produccion)

      if (turnoActual === 1) {
        const fechaAnterior = new Date(this.anio, this.mes, dia - 1)
        return this.buscarProduccionPorTurno(fechaAnterior, 3)
      } else if (turnoActual === 2) {
        return this.buscarProduccionPorTurno(fecha, 1)
      } else if (turnoActual === 3) {
        return this.buscarProduccionPorTurno(fecha, 2)
      }
      return 'ninguna'
    },
    getProduccionQueReleva(dia) {
      const fecha = new Date(this.anio, this.mes, dia)
      const turnoActual = this.getTurno(fecha, this.produccion)

      if (turnoActual === 1) {
        return this.buscarProduccionPorTurno(fecha, 2)
      } else if (turnoActual === 2) {
        return this.buscarProduccionPorTurno(fecha, 3)
      } else if (turnoActual === 3) {
        const fechaSiguiente = new Date(this.anio, this.mes, dia + 1)
        return this.buscarProduccionPorTurno(fechaSiguiente, 1)
      }
      return 'ninguna'
    },
    buscarProduccionPorTurno(fecha, turnoBuscado) {
      for (let prod = 1; prod <= 4; prod++) {
        if (prod !== this.produccion && this.getTurno(fecha, prod) === turnoBuscado) {
          return prod
        }
      }
      return 'ninguna'
    },
    seleccionarDia(dia) {
      this.diaSeleccionado = dia
    },
    mesAnterior() {
  this.transitionName = 'slide-right';
  if (this.mes === 0) {
    this.mes = 11
    this.anio--
  } else {
    this.mes--
  }
  this.diaSeleccionado = null
},
mesSiguiente() {
  this.transitionName = 'slide-left';
  if (this.mes === 11) {
    this.mes = 0
    this.anio++
  } else {
    this.mes++
  }
  this.diaSeleccionado = null
},
    anioAnterior() {
      this.anio--
      this.diaSeleccionado = null
    },
    anioSiguiente() {
      this.anio++
      this.diaSeleccionado = null
    }
  }
}
</script>
