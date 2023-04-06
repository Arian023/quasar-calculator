<template>
  <q-page>
    <div class="row justify-center">
      <div class="col-12 col-sm-8 col-md-6">
        <q-card bordered>
          <q-card-section>
            <div class="text-h5 text-grey-6 text-right">
              {{
                actualValue || expression ? expression + actualValue : '&nbsp;'
              }}
            </div>
          </q-card-section>

          <q-separator inset />
          <q-card-section>
            <div class="text-h3 text-right">{{ result }}</div>
          </q-card-section>
          <q-card-section>
            <div class="row q-col-gutter-sm">
              <div
                v-for="(btnLabel, index) in buttonsCalc"
                :key="index"
                class="col-3"
                @click="btnAction(btnLabel)">
                <q-btn
                  class="full-width text-h6"
                  :color="isNaN(btnLabel) ? 'secondary' : 'grey-2'"
                  :text-color="isNaN(btnLabel) ? 'white' : 'grey-8'"
                  >{{ btnLabel }}</q-btn
                >
              </div>
              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="secondary"
                  @click="btnReset"
                  @keydown.delete="btnReset">
                  Reset
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="primary"
                  @click="btnResult"
                  @keydown.enter="btnResult">
                  =
                </q-btn>
              </div>
            </div>
          </q-card-section>
          <q-separator inset />
          <q-card-section>
            <div class="text-h5">Historial:</div>
            <pre>{{ historyResults }}</pre>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue';
import { evaluate } from 'mathjs';

const buttonsCalc = [
  7,
  8,
  9,
  '%',
  4,
  5,
  6,
  '+',
  1,
  2,
  3,
  '-',
  '.',
  0,
  '/',
  '*',
];
const actualValue = ref(0);
const expression = ref('');
const result = ref(0);
const operatorClick = ref(true);
const historyResults = ref([]);
/** Genera la expresión a calcular
 * @param {string|number} valueBtn - El valor del botón pulsado.
 */
const btnAction = (valueBtn) => {
  if (isNaN(valueBtn)) {
    evaluateOperator(valueBtn);
  } else {
    if (operatorClick.value) {
      actualValue.value = '';
      operatorClick.value = false;
    }
    actualValue.value = `${actualValue.value}${valueBtn}`;
  }
};
/** Evalúa qué operación fue pulsada y la aplica
 * @param {string|number} valueBtn - El valor del botón pulsado.
 */
const evaluateOperator = (valueBtn) => {
  switch (valueBtn) {
    case '.':
      if (actualValue.value.indexOf('.') === -1) {
        actualValue.value = `${actualValue.value}${valueBtn}`;
      }
      break;
    case '%':
      if (actualValue.value !== '')
        actualValue.value = `${parseFloat(actualValue.value) / 100}`;
      break;
    default:
      if (!operatorClick.value) {
        expression.value += `${actualValue.value} ${valueBtn} `;
        actualValue.value = '';
        operatorClick.value = true;
      }
      break;
  }
};
const btnReset = () => {
  actualValue.value = expression.value = '';
  operatorClick.value = true;
};
const btnResult = () => {
  if (!operatorClick.value) {
    result.value = evaluate(expression.value + actualValue.value);
    expression.value = result.value;
    actualValue.value = '';
    historyResults.value.push(
      `${expression.value}${actualValue.value} = ${result.value}`
    );
  }
};
</script>

<script>
export default {
  name: 'IndexPage',
};
</script>
