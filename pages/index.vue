<template>
  <v-main>
    <v-container class="my-4">
      <h1 class="headline font-weight-bold">
        Horário de Funcionamento
      </h1>
      <v-tabs class="mt-4" color="red">
        <v-tab>Horário</v-tab>
      </v-tabs>
      <v-divider class="mb-4" />
      <p>
        <span class="font-weight-bold my-6">Escolha os horários em que o seu restaurante abrirá no iZiFood.</span>
      </p>
      <v-card class="mx-6 my-2">
        <v-card-text>
          <v-container>
            <v-row align="center" justify="center">
              <v-col xs="12" sm="12" md="4">
                <span class="font-weight-bold headline black--text">
                  <v-icon class="far fa-star red--text" />
                  Recomendações do iZiFood para sua cidade
                </span>
              </v-col>
              <v-col xs="12" sm="12" md="8">
                <span class="font-weight-bold my-6">Fique aberto durante os horários de pico para receber mais pedidos.</span>
                <v-card flat color="grey lighten-2">
                  <v-container>
                    <v-row align="center" justify="center">
                      <v-col class="text-center" cols="4">
                        <p>
                          <span class="font-weight-bold">80%</span> dos pedidos são feitos
                          nestes horário, diariamente:
                        </p>
                      </v-col>
                      <v-col class="text-center" cols="4">
                        <p>
                          <v-icon class="fas fa-sun" left />
                          <span class="font-weight-bold">Pico no almoço</span>
                        </p>
                        <p>Das 11h às 15h</p>
                      </v-col>
                      <v-col class="text-center" cols="4">
                        <p>
                          <v-icon left class="fas fa-moon" />
                          <span class="font-weight-bold">Pico no jantar</span>
                        </p>
                        <p>Das 18h às 23h</p>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
      </v-card>
      <v-btn rounded dark class="my-6" color="red" @click="$root.$emit('Dialog::open', true)">
        <span class="pa-4">Adicionar Horários</span>
      </v-btn>
      <v-alert outlined color="grey darken-1" class="rounded-lg my-2">
        <v-row align="center" justify="space-around">
          <v-col class="text-center" cols="3">
            <span class="display-1 font-weight-bold black--text">{{ totalSemanal }}h</span>
            <br></br>
            <span class="body-2">Totais na semana</span>
          </v-col>
          <v-col class="text-center" cols="3">
            <span class="display-1 font-weight-bold black--text">{{ picoAlmoco }}h</span>
            <br></br>
            <span class="body-2">No pico de almoço iFood</span>
          </v-col>
          <v-col class="text-center" cols="3">
            <span class="display-1 font-weight-bold black--text">{{ picoJanta }}h</span>
            <br></br>
            <span class="body-2">No pico de janta iFood</span>
          </v-col>
        </v-row>
      </v-alert>
      <v-row align="center" justify="center">
        <v-col v-for="dia in dias" :key="dia.dia" class="text-center">
          <span class="headline my-2">{{ dia.dia }}</span>
          <br></br>
          <v-chip v-if="dia.aberto!='Fechado'" outlined class="success success--text my-2">
            <span>
              Aberto por&nbsp;
            </span>
            {{ dia.aberto }}h
          </v-chip>
          <v-chip v-else class="grey lighten-2">
            {{ dia.aberto }}
          </v-chip>
        </v-col>
      </v-row>
      <modal />
    </v-container>
    <v-sheet height="400" width="1250" class="ml-6 mb-6">
      <v-calendar
        ref="calendar"
        :events="events"
        color="success"
        type="week"
        event-color="green"
        value="2020-08-30"
        hide-header
      />
    </v-sheet>
  </v-main>
</template>

<script>

import modal from '@/components/modal'

export default {
  components: {
    modal
  },

  data () {
    return {
      totalSemanal: 0,
      picoAlmoco: 0,
      picoJanta: 0,
      dias: [
        {
          dia: 'Segunda',
          aberto: 'Fechado'
        },
        {
          dia: 'Terça',
          aberto: 'Fechado'
        },
        {
          dia: 'Quarta',
          aberto: 'Fechado'
        },
        {
          dia: 'Quinta',
          aberto: 'Fechado'
        },
        {
          dia: 'Sexta',
          aberto: 'Fechado'
        },
        {
          dia: 'Sábado',
          aberto: 'Fechado'
        },
        {
          dia: 'Domingo',
          aberto: 'Fechado'
        }
      ],

      events: [
        {
          name: '',
          start: '2020-08-30 00:00',
          end: '2020-08-30 00:00'
        },
        {
          name: '',
          start: '2020-08-31 00:00',
          end: '2020-08-31 00:00'
        },
        {
          name: '',
          start: '2020-09-01 00:00',
          end: '2020-09-01 00:00'
        },
        {
          name: '',
          start: '2020-09-02 00:00',
          end: '2020-09-02 00:00'
        },
        {
          name: '',
          start: '2020-09-03 00:00',
          end: '2020-09-03 00:00'
        },
        {
          name: '',
          start: '2020-09-04 00:00',
          end: '2020-09-04 00:00'
        },
        {
          name: '',
          start: '2020-09-05 00:00',
          end: '2020-09-05 00:00'
        }
      ]
    }
  },

  created () {
    this.$root.$on('Schedule::update', ($event) => {
      this.totalSemanal = 0
      this.picoAlmoco = 0
      this.picoJanta = 0
      for (let i = 0; i < $event.length; i++) {
        if ($event[i].checked !== true) {
          this.events[i].start = this.events[i].start.substr(0, 11) + $event[i].abre
          this.events[i].end = this.events[i].end.substr(0, 11) + $event[i].fecha
          let abreHora = $event[i].abre.substr(0, 2)
          abreHora = abreHora * 60
          let fechaHora = $event[i].fecha.substr(0, 2)
          fechaHora = fechaHora * 60
          this.totalSemanal += (fechaHora - abreHora) / 60
          let totalDia = fechaHora - abreHora
          totalDia = totalDia / 60
          this.dias[i].aberto = totalDia
          if (abreHora < 15 * 60) {
            const minAlmoco = abreHora < 11 * 60 ? 11 * 60 : abreHora
            const maxAlmoco = fechaHora > 15 * 60 ? 15 * 60 : fechaHora
            this.picoAlmoco += (maxAlmoco - minAlmoco) / 60
          }
          if (fechaHora > 18 * 60) {
            const minJanta = abreHora < 18 * 60 ? 18 * 60 : abreHora
            const maxJanta = fechaHora > 23 * 60 ? 23 * 60 : fechaHora
            this.picoJanta += (maxJanta - minJanta) / 60
          }
        }
      }
    })
  },

  mounted () {
    this.$refs.calendar.scrollToTime('08:00')
  }
}
</script>

<style scoped>
.my-event {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  border-radius: 2px;
  background-color: #1867c0;
  color: #ffffff;
  border: 1px solid #1867c0;
  font-size: 12px;
  padding: 3px;
  cursor: pointer;
  position: relative;
}

.my-event.with-time {
  position: absolute;
  right: 4px;
  margin-right: 0px;
}
</style>
