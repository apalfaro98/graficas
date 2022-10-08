<script>
import salesData from '../data/data.json';
import { Bar } from 'vue-chartjs';
import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale,
} from 'chart.js';

ChartJS.register(
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale
);

export default {
    name: 'BarChart',
    components: { Bar },

    data() {
        return {
            categories: Object.keys(salesData),
            products: [],
            brands: [],
            category: '',
            product: '',
            brand: '',
            salesData: salesData,
            chartData: {
                labels: [],
                datasets: [
                    {
                        label: 'Ventas',
                        backgroundColor: '#60a5fa',
                        data: [],
                    },
                ],
            },
            chartOptions: {
                responsive: true,
                maintainAspectRatio: false,
            },
        };
    },
    mounted() {
        this.category = this.categories[0];
        this.products = Object.keys(salesData[this.category]);
        this.product = this.products[0];
        this.brands = Object.keys(salesData[this.category][this.product]);
        this.brand = this.brands[0];
        this.chartData.labels = Object.keys(
            salesData[this.category][this.product][this.brand]
        );
        this.chartData.datasets[0].data = Object.values(
            salesData[this.category][this.product][this.brand]
        );
    },
    watch: {
        category() {
            this.products = Object.keys(salesData[this.category]);
            this.product = this.products[0];
            this.brands = Object.keys(salesData[this.category][this.product]);
            this.brand = this.brands[0];
        },
        product() {
            this.brands = Object.keys(salesData[this.category][this.product]);
            this.brand = this.brands[0];
        },
        brand() {
            this.chartData.datasets[0].data = Object.values(
                salesData[this.category][this.product][this.brand]
            );
        },
    },
};
</script>

<template>
    <div class="flex justify-around items-center my-5">
        <div>
            <span class="pr-2">Categorias: </span>
            <select
                v-model="category"
                class="border-black border rounded-lg outline-none"
            >
                <option v-for="(item, i) in categories" :key="i" :value="item">
                    {{ item }}
                </option>
            </select>
        </div>
        <div>
            <span class="pr-2">Productos:</span>
            <select
                v-model="product"
                class="border-black border rounded-lg outline-none"
            >
                <option v-for="(item, i) in products" :key="i" :value="item">
                    {{ item }}
                </option>
            </select>
        </div>
        <div>
            <span class="pr-2">Marcas:</span>
            <select
                v-model="brand"
                class="border-black border rounded-lg outline-none"
            >
                <option v-for="(item, i) in brands" :key="i" :value="item">
                    {{ item }}
                </option>
            </select>
        </div>
    </div>
    <h2 class="text-center">Sales By Month for:</h2>
    <Bar :chart-data="chartData" :chart-options="chartOptions" :height="400" />
</template>
