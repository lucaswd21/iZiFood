<template>
  <v-dialog v-model="dialog" width="475">
    <v-card class="rounded-lg my-2" width="475">
      <v-card-title class="headline">
        Horários da Loja
      </v-card-title>
      <v-row class="mx-6">
        <v-col cols="4" class="mt-n4">
          <v-checkbox
            v-for="item in schedule"
            :key="item.dia"
            v-model="item.checked"
            :label="item.dia"
            :class="item.label"
          />
        </v-col>
        <v-col cols="8">
          <v-row v-for="item in schedule" :key="item.dia" align="center" justify="center">
            <v-col cols="5" :class="item.field">
              <v-text-field
                v-model="item.abre"
                v-mask="'##:##'"
                pattern="[0-9]"
                rounded
                outlined
                :disabled="item.checked"
              />
            </v-col>
            <v-col :class="item.icon" cols="2">
              <v-icon class="fas fa-minus" />
            </v-col>
            <v-col cols="5" :class="item.field">
              <v-text-field
                v-model="item.fecha"
                v-mask="'##:##'"
                pattern="[0-9]"
                rounded
                outlined
                :disabled="item.checked"
              />
            </v-col>
          </v-row>
        </v-col>
      </v-row>
      <v-btn rounded dark class="mt-n16 mx-4" color="red" @click="$root.$emit('Schedule::update', schedule),dialog=false">
        <span class="pa-4">Salvar</span>
      </v-btn>
    </v-card>
  </v-dialog>
</template>
<script>

export default {
  data () {
    return {
      dialog: false,
      schedule: [
        {
          dia: 'Segunda',
          checked: false,
          label: 'mb-n2',
          field: 'mt-n4',
          icon: 'mt-n10',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Terça',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Quarta',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Quinta',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Sexta',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Sábado',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        },
        {
          dia: 'Domingo',
          checked: false,
          label: 'mb-n1',
          field: 'mt-n13',
          icon: 'mt-n16',
          abre: '00:00',
          fecha: '00:00'
        }
      ]
    }
  },

  created () {
    this.$root.$on('Dialog::open', ($event) => {
      this.dialog = $event
    })
  }
}
</script>
