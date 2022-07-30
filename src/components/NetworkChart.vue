<template>

    <div class="grid gap-8 grid-cols-1 w-1/2 mx-auto">
        <div class="rounded-xl bg-gray-50 border-indigo-800 border-2 shadow p-5">
            <highcharts class="hc" :options="chartOptions" ref="chart"></highcharts>
        </div>
        <div class="rounded-xl bg-gray-50 border-indigo-800 border-2 shadow p-5">
            <div class="flow-root">
                <ul role="list" class="-mb-8">
                    <li v-for="(event, eventIdx) in prufer" :key="event.id">
                        <div class="relative pb-8">
                            <span v-if="eventIdx !== prufer.length - 1"
                                class="absolute top-4 left-4 -ml-px h-full w-0.5 bg-gray-200" aria-hidden="true" />
                            <div class="relative flex space-x-3">
                                <div>
                                    <span
                                        :class="['bg-indigo-100 h-8 w-8 rounded-full flex items-center justify-center ring-8 ring-white']">
                                        {{ event }}
                                    </span>
                                </div>
                                <div class="min-w-0 flex-1 pt-1.5 flex justify-between space-x-4">
                                    <RadioGroup v-model="prufer[eventIdx]">
                                        <RadioGroupLabel class="block text-sm font-medium text-gray-700">Choose a number
                                        </RadioGroupLabel>
                                        <div class="mt-4 flex items-center space-x-3">
                                            <RadioGroupOption as="template" v-for="val in pruferValues" :key="val.id"
                                                :value="val" v-slot="{ active, checked }">
                                                <div
                                                    :class="[active && checked ? 'ring ring-offset-1' : '', !active && checked ? 'ring-2' : '', '-m-0.5 relative p-0.5 rounded-full flex items-center justify-center cursor-pointer focus:outline-none']">
                                                    <span aria-hidden="true"
                                                        :class="['bg-gray-100 h-8 w-8 border border-black border-opacity-10 rounded-full']">
                                                        {{ val }}
                                                    </span>
                                                </div>
                                            </RadioGroupOption>
                                        </div>
                                    </RadioGroup>
                                </div>
                            </div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <p>{{ prufer }}</p>
        <p>{{ pruferSequence }}</p>
    </div>

</template>

<script>
import { Chart } from 'highcharts-vue'
import Highcharts from 'highcharts'
import networkGraphInit from 'highcharts/modules/networkgraph'
import { RadioGroup, RadioGroupLabel, RadioGroupOption } from '@headlessui/vue'

networkGraphInit(Highcharts)

export default {
    components: {
        highcharts: Chart,
        RadioGroup: RadioGroup,
        RadioGroupLabel: RadioGroupLabel,
        RadioGroupOption: RadioGroupOption
    },
    data() {
        return {
            pruferValues: [1, 2, 3, 4, 5, 6, 7, 8, 9],
            prufer: [2, 2, 6, 6, 9, 5, 2],
            chartOptions: {
                chart: {
                    type: 'networkgraph',
                },
                plotOptions: {
                    networkgraph: {
                        keys: ['from', 'to'],
                        layoutAlgorithm: {
                            enableSimulation: true,
                            friction: -0.9
                        }
                    }
                },
                title: {
                    text: 'TRIMMER'
                },
                series: [{
                    dataLabels: {
                        enabled: true,
                        linkFormat: ''
                    },
                    data: [
                        { to: 1, from: 2 },
                        { to: 1, from: 3 },
                        { to: 1, from: 4 },
                        { to: 1, from: 5 },
                        { to: 1, from: 6 },
                        { to: 1, from: 7 },
                        { to: 1, from: 8 },
                        { to: 1, from: 9 },
                    ]
                }]
            }
        }
    },
    computed: {
        pruferSequence() {
            let vertices = this.prufer.length + 2;
            let vertex_set = new Array(vertices);
            var pruferSequence = [];

            for (let i = 0; i < vertices; i++)
                vertex_set[i] = 0;

            for (let i = 0; i < vertices - 2; i++)
                vertex_set[this.prufer[i] - 1] += 1;

            let j = 0;
            for (let i = 0; i < vertices - 2; i++) {
                for (j = 0; j < vertices; j++) {
                    // If j+1 is not present in prufer set
                    if (vertex_set[j] == 0) {
                        // Remove from Prufer set and print
                        // pair.
                        vertex_set[j] = -1;

                        pruferSequence.push({ to: j + 1, from: this.prufer[i] });

                        vertex_set[this.prufer[i] - 1]--;

                        break;
                    }
                }
            }

            j = 0;
            var vertice = {};
            // For the last element
            for (let i = 0; i < vertices; i++) {
                if (vertex_set[i] == 0 && j == 0) {
                    vertice.to = i + 1;
                    j++;
                }
                else if (vertex_set[i] == 0 && j == 1)
                    vertice.from = i + 1;

            }
            pruferSequence.push(vertice);

            this.chartOptions.series[0].data = pruferSequence;

            return pruferSequence;
        }
    }
};
</script>