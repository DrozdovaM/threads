<template>
  <div id="inspire">
    <v-card
        flat
        color="transparent">
      <v-subheader>Количество узлов</v-subheader>
      <v-card-text>
        <v-slider
            :min="unit.min"
            :max="unit.max"
            v-model="unit.val"
            :thumb-color="unit.color"
            thumb-label="always"
            tick-size="4"
            ticks="always"
            v-on:change="onUnitsChange($event)"
        >
          <template v-slot:append>
            <v-text-field
                v-model="unit.val"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 40px"
            ></v-text-field>
          </template>
        </v-slider>
      </v-card-text>
    </v-card>
      <v-stage
          ref="stage"
        :config="configKonva">
      <v-layer ref="layer">
        <v-rect id="rect"
            v-for="rect in configRect"
            :key="rect.id"
            :config="{
              x: rect.x,
              y: 10,
              height: 200,
              width: 100,
              stroke: 'black',
              dash: [10, 10]
            }"
        >
        </v-rect>
        <v-circle
            v-for="circle in configCircle"
            :key="circle.id"
            :config="{
              x: circle.x,
              y: circle.y,
              radius: 25,
              stroke: circle.stroke

            }">
        </v-circle>
        <v-arrow
            v-for="arrow1 in configArrow1"
            :key="arrow1.id"
            :config="{
              x: arrow1.x,
              y: 85,
              points: [0, 0, 0, 50],
              pointerLength: 5,
              pointerWidth: 5,
              fill: 'black',
              stroke: 'black',
              strokeWidth: 4
            }">
        </v-arrow>
        <v-arrow
            v-for="arrow2 in configArrow2"
            :key="arrow2.id"
            :config="{
              x: arrow2.x,
              y: 85,
              points: [0, 50, 0, 0],
              pointerLength: 5,
              pointerWidth: 5,
              fill: 'black',
              stroke: 'black',
              strokeWidth: 4}">

        </v-arrow>
        <div v-if="configRect.length !== 1 && configRect.length !== 13 ">
          <v-arrow
                   v-for="arrow3 in configArrow3"
                   :key="arrow3.id"
                   :config="{
              x: arrow3.x,
              y: 60,
              points: [0, 0, 100, 0],
              pointerLength: 5,
              pointerWidth: 5,
              fill: 'black',
              stroke: 'black',
              strokeWidth: 2}">

          </v-arrow>
        </div>
        <v-rect
            :config="configDB">
        </v-rect>
        <v-ellipse id="db"
            :config="configEllipse">

        </v-ellipse>
      </v-layer>
    </v-stage>

  </div>

</template>

<script src="/src/libs/leader-line/leader-line.min.js"></script>
<script>
import Vue from 'vue';
import Vuetify from 'vuetify'
import VueKonva from 'vue-konva'

Vue.use(VueKonva)
Vue.use(Vuetify)
const width = window.innerWidth;
const height = window.innerHeight

export default {
  vuetify: new Vuetify(),

  data() {
    return {
      value: 0,
      unit: { val: 1, color: 'orange', min: 1, max: 13},
      configKonva: {
        width: width,
        height: height
      },
      configRect: [
        {
          x: 10,
        }
      ],
      configCircle: [
        {
          x: 60,
          y: 60,
          stroke: "black"
        },
        {
          x: 60,
          y: 160,
          stroke: "black"
        }
      ],
      configEllipse: {
        x: width/2 + 50,
        y: 430,
        radiusX: 45,
        radiusY: 30,
        stroke: 'black'
      },
      configDB: {
        x: width/2,
        y: 400,
        width: 100,
        height: 100,
        stroke: 'black',
        cornerRadius: [40, 40, 30, 30],
        shadowBlur: 2
      },
      configArrow1: [
        {
          x: 60,
        }
      ],
      configArrow2: [
        {
          x: 60,
        }
      ],
      configArrow3: [
        {
          x: 85,
        }
      ]
    }
  },
  methods: {
    onUnitsChange(number) {
      if (this.configRect.length < number) {
        console.log('alo')
        while (this.configRect.length < number) {
          this.configRect.push({
            x: this.configRect[this.configRect.length - 1].x + 150,
          });
          let newCircles;
          if (this.configRect.length - 1 % 2 === 0) {
            newCircles = [
              {
                x: this.configCircle[this.configCircle.length - 1].x + 150,
                y: this.configCircle[this.configCircle.length - 1].y + 100,
                stroke: "black"
              },
              {
                x: this.configCircle[this.configCircle.length - 1].x + 150,
                y: this.configCircle[this.configCircle.length - 1].y,
                stroke: "black"
              }
            ];
            this.configCircle = this.configCircle.concat(newCircles);

          } else {
              newCircles = [
              {
                x: this.configCircle[this.configCircle.length - 1].x + 150,
                y: this.configCircle[this.configCircle.length - 1].y - 100,
                stroke: "black"
              },
              {
                x: this.configCircle[this.configCircle.length - 1].x + 150,
                y: this.configCircle[this.configCircle.length - 1].y,
                stroke: "black"
              }
            ];
            this.configCircle = this.configCircle.concat(newCircles);
          }
          this.configArrow1.push({
            x: this.configArrow1[this.configArrow1.length - 1].x + 150
          });
          this.configArrow2.push({
                x: this.configArrow2[this.configArrow2.length - 1].x + 150
              }
          );
          this.configArrow3.push({
            x: this.configArrow2[this.configArrow2.length - 1].x + 25
          });
        }
      } else {
          while (this.configRect.length > number) {
            const difference = this.configRect.length - (this.configRect.length - number);
            this.configCircle.splice(difference * 2, number * 2);
            this.configRect.splice(difference, number);
            this.configArrow1.splice(difference, number);
            this.configArrow2.splice(difference, number);
            this.configArrow3.splice(difference, number);

          }
        }
      // new LeaderLine(document.getElementById("rect"), document.getElementById("db"), {
      //   color: "red",
      //   size: 3
      // });
    }
  }
}
</script>