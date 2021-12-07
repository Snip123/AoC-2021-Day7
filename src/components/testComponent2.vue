<template>
  <div class="counter">
    <textarea v-model="message" placeholder="add multiple lines"></textarea>
    <br />
    <button @click="updateData">Update</button>
    <input type="file" ref="doc" @change="readFile" />
    <br/>
    <span>Output:</span>
    <p style="white-space: pre-line;">{{ output }}</p>
    
  </div>
</template>

<script>

export default {
  name: 'testComponent2',
  data: function() {
    return {
      message: "",
      output: ""
    }
  },
  methods:
  {
    readFile: function () {
      console.log("read")
      this.file = this.$refs.doc.files[0];
      const reader = new FileReader();
      if (this.file.name.includes(".txt")) {
        reader.onload = (res) => {
          this.message = res.target.result;
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      } else {
        reader.onload = (res) => {
          this.message(res.target.result);
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      }
    },

    updateData: function () {
      if(!this.message)
        this.readFile();
      
     let crabPositions = this.message.split(',');
      let medianPos = medianFloored(crabPositions);
      let averagePos = averageFloored(crabPositions);
      let fuelUsed = moveToCrabsToPosition(crabPositions, averagePos);
    console.log(medianPos);
    console.log(averagePos);
console.log(fuelUsed);

    }
  } 
}
function moveToCrabsToPosition(array, target)
{
  return array.reduce((total,current) => {
    return total += fuelCost(Math.abs(current-target));
    },0)
}
function fuelCost(distance)
{
  let fuelCost = 0;
  for (let index = 0; index <= distance; index++) {
    fuelCost += index;
    
  }
  return fuelCost;
}
function medianFloored (array)
{
  array.sort((a,b) => {return a-b;});
  let half = Math.floor(array.length /2);
  return array[half];   
}
function averageFloored (array)
{
  return Math.floor((array.reduce((a,b) => {
    return parseInt(a)+parseInt(b);},0)/array.length));

}
</script>

