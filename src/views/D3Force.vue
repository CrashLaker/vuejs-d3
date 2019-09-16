<template>
  <div id="d3force">
    <Dependency :data="data" :grouped="grouped"
                :dir="dir"/>
    <div style="padding-top: 10px; text-align: center">
      <button @click="changeData()">Change Data</button>
      Group? <input type="checkbox" v-model="grouped"/>
      LR <input type="radio" value="lr" v-model="dir"/>
      TD? <input type="radio" value="td" v-model="dir"/>
    </div>
  </div>
</template>



<script>
/* eslint-disable */
import Dependency from '@/components/Dependency'

export default {
  name: "D3Force",
  components: {
    Dependency
  },
  data() {
    return {
      dir: "lr",
      data: null,
      grouped: false,
      dataList: ["data1.json", "data2.json", "data3.json"]
    };
  },
  mounted() {
    this.changeData();
  },
  methods: {
    changeData() {
      const dataIndex = Math.floor(Math.random() * this.dataList.length);
      d3.json(this.dataList[dataIndex])
        .then(data => {
          this.data = data;
        })
        .catch(error => {
          console.error(
            "Cannot proceed with simulation, failed to  retrieve data."
          );
        });
    }
  }
};
</script>


 <style>
  .faded {
    opacity: 0.1;
    transition: 0.3s opacity;
  }
  .highlight {
    opacity: 1;
  }

  path.link {
    fill: none;
    stroke: #666;
    stroke-width: 1.5px;
  }
  path.link.depends {
    stroke: #005900;
    stroke-dasharray: 5, 2;
  }
  path.link.needs {
    stroke: #7f3f00;
  }

  circle {
    fill: #ffff99;
    stroke: #191900;
    stroke-width: 1.5px;
  }
  circle.system {
    fill: #cce5ff;
    stroke: #003366;
  }
  circle.mount {
    fill: #ffe5e5;
    stroke: #660000;
  }
  circle.init {
    fill: #b2e8b2;
    stroke: #001900;
  }

  circle.selected {
    stroke: #ff6666FF !important;
    stroke-width: 3px;
    animation: selected 2s infinite alternate ease-in-out;
  }

  @keyframes selected {
    from {
      stroke-width: 5px;
      r: 26;
    }
    to {
      stroke-width: 1px;
      r: 30;
    }
  }

  text {
    font: 10px sans-serif;
    pointer-events: none;
    text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
  }

  rect.caption {
    fill: #CCCCCCAC;
    stroke: #666;
    stroke-width: 1px;
  }
  text.caption {
    font-size: 14px;
    font-weight: bold;
  }
  </style>