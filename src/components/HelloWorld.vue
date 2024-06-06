<template>
  <v-container>
    <div>
      <v-dialog v-model="dialog" max-width="600">
        <template v-slot:activator="{ props: activatorProps }">
          <v-btn class="text-none font-weight-regular" prepend-icon="mdi-view-grid-plus" text="Agregar tarea" variant="tonal" v-bind="activatorProps"></v-btn>
        </template>

        <v-card prepend-icon="mdi-checkbox-marked-circle-plus-outline" title="AGREGAR UNA NUEVA TAREA A LA LISTA">
          <v-card-text>
            <v-row dense>
              <v-col cols="12">
                <v-text-field label="Tarea" type="text" v-model="task" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Fecha" type="date" v-model="date" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Hora" type="time" v-model="time" required></v-text-field>
              </v-col>
            </v-row>
            <small class="text-caption text-medium-emphasis">Todos los campos son obligatorios</small>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="error" text="Cerrar" variant="plain" @click="dialog = false; reset()"></v-btn>
            <v-btn color="success" text="Guardar" variant="tonal" @click="saveTask()"></v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <v-table fixed-header v-if="tasks.length > 0">
      <thead>
        <tr>
          <th class="text-center">Actividad</th>
          <th class="text-center">Fecha</th>
          <th class="text-center">Hora</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in tasks" :key="index">
          <td class="text-center">{{ row.task }}</td>
          <td class="text-center">{{ formatDateToMX(row.date) }}</td>
          <td class="text-center">{{ formatTimeToMXWithAmPm(row.time)}}</td>
        </tr>
      </tbody>
    </v-table>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      task: null,
      date: null,
      time: null,
      tasks: []
    }),
    methods: {
      reset() {
        this.task = null,
        this.date = null,
        this.time = null
      },
      saveTask(){
        let tmp = JSON.parse(localStorage.getItem('tasks')) || []
        tmp.push({
          task: this.task,
          date: this.date,
          time: this.time
        })
        localStorage.setItem('tasks', JSON.stringify(tmp))
        this.tasks = JSON.parse(localStorage.getItem('tasks'))
        this.dialog = false
        this.reset()
      },
      formatTimeToMXWithAmPm(timeString) {
        // Dividir la cadena de entrada en horas y minutos
        let [hours, minutes] = timeString.split(':').map(Number);

        // Crear una fecha con la hora proporcionada
        let date = new Date();
        date.setHours(hours, minutes, 0, 0); // Establecer horas y minutos

        // Opciones para formatear la hora en formato de 12 horas con AM/PM
        let options = {
          hour: '2-digit',
          minute: '2-digit',
          second: '2-digit',
          hour12: true // Esto asegura que se use el formato de 12 horas con AM/PM
        };

        // Obtener la hora formateada
        let timeStringFormatted = date.toLocaleTimeString('es-MX', options);

        return timeStringFormatted;
      },
      formatDateToMX(dateString) {
        // Crear una fecha a partir de la cadena de entrada
        let date = new Date(dateString);

        // Opciones para formatear la fecha en formato de México
        let options = {
          day: '2-digit',
          month: '2-digit',
          year: 'numeric'
        };

        // Obtener la fecha formateada
        let dateFormatted = date.toLocaleDateString('es-MX', options);

        return dateFormatted;
      }
    },
    created(){
      if (!localStorage.getItem('tasks')) {
        localStorage.setItem('tasks', JSON.stringify([]))
      }
      this.tasks = JSON.parse(localStorage.getItem('tasks'))
    }
  }
</script>
