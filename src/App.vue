<template>
  <div class="table_top">
      <div class="graph">
        <p class="graph_p_first">Показатель</p>
        <p class="graph_p">Выручка, руб</p>
      </div>
      <div class="day_top">
        <p class="day_p_first">Текущий день</p>
        <p class="day_p">500 521</p>
      </div>
      <div class="before_top">
        <p class="before_p_first">Вчера</p>
        <p class="before_p before_p_plus">480 521 <span class="plus_percent">4%</span> </p>
      </div>
      <div class="day_of_the_week_top">
        <p class="day_of_the_week_p_first">Этот день недели</p>
        <p class="day_of_the_week_p p_minus">480 521</p>
      </div>
  </div>
  <div id="graph">
    <canvas ref="canvas"></canvas>
    <ul>
      <li v-for="(item, index) in rows"
       :key="index"
        @click="updateChart(item)">
        <p class="graph_p">{{ item.title }}</p> <p class="day_p">{{ item.value1 }}</p> <p class="before_p" :style="{ 
          backgroundColor: index === 3 || index === 4 ? '#ecf7e7' : 
                 index === 5 || index === 7 || index === 8 ? '#fee6e6' : 
                 '#f5f5f5' 
        }">{{ item.value2 }} <span :style="{ color: index === 5 || index === 7 || index === 8 ? '#ff6968' : ' #6F9628' }">{{ item.percent }}</span></p>
         <p class="day_of_the_week_p" :style="{ backgroundColor: index === 0 || index === 1 || index === 2 || index === 7 ? '#f5f5f5' : ' #ecf7e7'}">{{ item.value3 }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Chart, Legend, PointElement, Tooltip, CategoryScale, LinearScale, LineController, LineElement, } from 'chart.js';

// Регистрация компонентов Chart.js
Chart.register(Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, LineController);

const canvas = ref(null);
let chartInstance = null;

const rows = [
  { title: 'Наличные', value1: 300000, value2: 300000, percent: "0%", value3: 300000 },
  { title: 'Безналичный расчет', value1: 100000, value2: 100000, percent: "0%", value3: 100000 },
  { title: 'Кредитные карты', value1: 100521, value2: 100521, percent: "0%", value3: 100521 },
  { title: 'Средний чек, руб', value1: 1300, value2: 1300, percent: "44%", value3: 900 },
  { title: 'Средний гость, руб', value1: 1200, value2: 1200, percent: "50%", value3: 800 },
  { title: 'Удаления из чека (после оплаты), руб', value1: 1000, value2: 1000, percent: "-9%", value3: 900 },
  { title: 'Удаления из чека (до оплаты), руб', value1: 1300, value2: 1300, percent: "0%", value3: 900 },
  { title: 'Количество чеков', value1: 34, value2: 36, percent: "-6%", value3: 34 },
  { title: 'Количество гостей', value1: 34, value2: 36, percent: "-6%", value3: 32 }
];

const chartOptions = {
  responsive: true,
  plugins: {
    legend: {
      display: false,
      position: 'top'
    },
  }
};

const updateChart = (item) => {
  const chartData = {
    labels: ['', '', ''],
    datasets: [
      {
        label: '',
        backgroundColor: 'rgba(66, 165, 245, 0.2)',
        borderColor: 'green',
        data: [item.value1, item.value2, item.value3],
        fill: true
      }
    ]
  };

  if (chartInstance) {
    chartInstance.data = chartData;
    chartInstance.update();
  }
};

onMounted(() => {
  const initialData = {
    labels: ['', '', ''],
    datasets: [
      {
        label: '',
        backgroundColor: 'rgba(66, 165, 245, 0.2)',
        borderColor: 'green',
        data: [rows[0].value1, rows[0].value2, rows[0].value3],
        fill: true
      }
    ]
  };

  if (canvas.value) {
    chartInstance = new Chart(canvas.value, {
      type: 'line',
      data: initialData,
      options: chartOptions
    });
  }
});
</script>

<style>
*{
  margin: 0;
  padding: 0;
}

.graph_p {
  background-color: #f5f5f5;
  height: 40px;
  text-align: left;
}

.table_top{
  display: grid;
  grid-template-columns: 200px 150px 100px 200px;  
  gap: 10px;      
  text-align: center;            
}

.day_p {
  background-color: #edf8ff;
  display: flex;
  height: 40px;
  padding-right: 30px;
  justify-content: right;
}


.before_p {
  display: flex;
  background-color: #f5f5f5;
  justify-content: space-around;
  height: 40px;
}


.day_of_the_week_p {
  height: 40px;
  background-color: #f5f5f5;
  padding-right: 30px;
  justify-content: right;
}





.graph_p_first {
  justify-content: center;
  background-color: #f5f5f5;
  height: 40px;
}


.day_p_first {
  text-align: center;
  height: 40px;
  justify-content: center;
  background-color: #edf8ff;
}


.before_p_first {
  background-color: #f5f5f5;
  justify-content: center;
  text-align: center;
  height: 40px;
}


.day_of_the_week_p_first {
  justify-content: center;
  height: 40px;
  text-align: center;
  background-color: #f5f5f5;
}

.minus_percent {
  color: #ff6968;
}
.plus_percent {
  color: #6F9628;
}

li {
  gap: 5px;
  display: grid;
  grid-template-columns: 200px 150px 100px 200px;  
}
p {
  margin-top: 5px;
  display: flex;
  align-items: center;
}
.p_plus {
  background-color: #ecf7e7;
}
#graph {
  width: 680px;
}
.p_minus {
  background-color: #fee6e6;
}
</style>
