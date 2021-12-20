<template>
  <!--HTML -->
  <h1>{{ msg }}</h1>
  <div class="train-controls">
    <h2 class="section col-sm-1">Training Data (x,y) pairs</h2>
    <div class="field-label-container">
      <div class="field-label">X</div>
      <div class="field-label">Y</div>
    </div>
    <div v-for="(item, index) in xValues" v-bind:key="index">
      <div>
        <div class="col-sm-1">
          <input type="number" class="field field-x" v-model="xValues[index]" />
          <input type="number" class="field field-y" v-model="yValues[index]" />
        </div>
      </div>
    </div>
    <button class="button-add-example button--green" @click="addItem">+</button>
  </div>

  <div class="predict-controls">
    <h2 class="section col-sm1">Predicting</h2>
    <input
      type="number"
      class="field element"
      v-model="valueToPredict"
      placeholder="Enter an integer number"
    />
    <div class="element" id="output">{{ predictedValue }}</div>

    <button class="element button--green" @click="train">Predict</button>
  </div>
</template>

<script>
import * as tf from "@tensorflow/tfjs";

// JS
export default {
  name: "TensorFlow",
  data() {
    return {
      xValues: [-1, 0, 1, 2, 3, 4],
      yValues: [-3, -1, 1, 3, 5, 7],
      predictedValue:
        "Click on the button to train and predict output for given input!",
      valueToPredict: "",
    };
  },
  methods: {
    addItem() {
      this.xValues.push(0);
      this.yValues.push(0);
    },
    async train() {
      // Define a model for linear regressiom
      const model = tf.sequential();
      model.add(tf.layers.dense({ units: 1, inputShape: [1] }));

      // Prepare the model for training: Specify the loss and optimizer
      model.compile({
        loss: "meanSquaredError",
        optimizer: "sgd",
      });

      const xs = tf.tensor2d(this.xValues, [this.xValues.length, 1]);
      const ys = tf.tensor2d(this.yValues, [this.yValues.length, 1]);

      // Train the model using the data and then predict

      await model.fit(xs, ys, { epochs: 250 }).then(() => {
        const string = model
          .predict(tf.tensor2d([this.valueToPredict], [1, 1]))
          .toString();
        this.predictedValue = string.match(/\d+/g, "").join();
        console.log(this.predictedValue);
      });
    },
  },
  props: {
    msg: String,
  },
};
</script>

<style scoped>
/*CSS*/
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
button {
  color: #42b983;
}
.field.element {
  margin-bottom: 1em;
}
.element {
  margin-bottom: 1em;
}

.field-label-container {
  padding: 0 200px 0 200px;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  margin-bottom: 1rem;
}


</style>
