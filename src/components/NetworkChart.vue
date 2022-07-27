<template>

    <div class="grid gap-8 grid-cols-1 w-1/2 mx-auto">
        <div class="rounded-xl bg-gray-50 border-indigo-800 border-2 shadow p-5">
            <highcharts class="hc" :options="chartOptions" ref="chart"></highcharts>
        </div>
        <div class="rounded-xl bg-gray-50 border-indigo-800 border-2 shadow p-5">
            <div class="flow-root">
                <ul role="list" class="-mb-8">
                    <li v-for="(event, eventIdx) in timeline" :key="event.id">
                        <div class="relative pb-8">
                            <span v-if="eventIdx !== timeline.length - 1"
                                class="absolute top-4 left-4 -ml-px h-full w-0.5 bg-gray-200" aria-hidden="true" />
                            <div class="relative flex space-x-3">
                                <div>
                                    <span
                                        :class="[event.iconBackground, 'h-8 w-8 rounded-full flex items-center justify-center ring-8 ring-white']">
                                    </span>
                                </div>
                                <div class="min-w-0 flex-1 pt-1.5 flex justify-between space-x-4">
                                    <RadioGroup v-model="selectedColor">
                                        <RadioGroupLabel class="block text-sm font-medium text-gray-700">Choose a label
                                            color</RadioGroupLabel>
                                        <div class="mt-4 flex items-center space-x-3">
                                            <RadioGroupOption as="template" v-for="color in colors" :key="color.name"
                                                :value="color" v-slot="{ active, checked }">
                                                <div
                                                    :class="[color.selectedColor, active && checked ? 'ring ring-offset-1' : '', !active && checked ? 'ring-2' : '', '-m-0.5 relative p-0.5 rounded-full flex items-center justify-center cursor-pointer focus:outline-none']">
                                                    <RadioGroupLabel as="span" class="sr-only">{{ color.name }}
                                                    </RadioGroupLabel>
                                                    <span aria-hidden="true"
                                                        :class="[color.bgColor, 'h-8 w-8 border border-black border-opacity-10 rounded-full']" />
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
            <!-- <div>
                <div class="grid gap-4 grid-cols-8">
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="prufer[0]" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i2" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i3" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i4" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i5" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i6" />
                    </div>
                    <div class="p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.i7" />
                    </div>
                    <div class="border-b-2 border-l-2 border-gray-900 p-2">
                        <input type="number" name="number"
                            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                            :placeholder="build.i1" v-model="build.root" />
                    </div>
                </div>
            </div> -->
        </div>
        {{ pruferSequence }}
    </div>

</template>

<script>
import { ref } from 'vue'
import { Chart } from 'highcharts-vue'
import Highcharts from 'highcharts'
import networkGraphInit from 'highcharts/modules/networkgraph'
import { RadioGroup, RadioGroupLabel, RadioGroupOption } from '@headlessui/vue'


networkGraphInit(Highcharts)

export default {
    setup() {
        const colors = [
            { name: 'Pink', bgColor: 'bg-pink-500', selectedColor: 'ring-pink-500' },
            { name: 'Purple', bgColor: 'bg-purple-500', selectedColor: 'ring-purple-500' },
            { name: 'Blue', bgColor: 'bg-blue-500', selectedColor: 'ring-blue-500' },
            { name: 'Green', bgColor: 'bg-green-500', selectedColor: 'ring-green-500' },
            { name: 'Yellow', bgColor: 'bg-yellow-500', selectedColor: 'ring-yellow-500' },
        ]

        const selectedColor = ref(colors[1])
    },
    components: {
        highcharts: Chart
    },
    data() {
        return {
            prufer: [
                2,
                2,
                6,
                6,
                9,
                5,
                2
            ],
            timeline: [
                {
                    id: 1,
                    content: 'Applied to',
                    target: 'Front End Developer',
                    href: '#',
                    date: 'Sep 20',
                    datetime: '2020-09-20',
                    iconBackground: 'bg-gray-400',
                },
                {
                    id: 2,
                    content: 'Advanced to phone screening by',
                    target: 'Bethany Blake',
                    href: '#',
                    date: 'Sep 22',
                    datetime: '2020-09-22',
                    iconBackground: 'bg-blue-500',
                },
                {
                    id: 3,
                    content: 'Completed phone screening with',
                    target: 'Martha Gardner',
                    href: '#',
                    date: 'Sep 28',
                    datetime: '2020-09-28',
                    iconBackground: 'bg-green-500',
                },
                {
                    id: 4,
                    content: 'Advanced to interview by',
                    target: 'Bethany Blake',
                    href: '#',
                    date: 'Sep 30',
                    datetime: '2020-09-30',
                    iconBackground: 'bg-blue-500',
                },
                {
                    id: 5,
                    content: 'Completed interview with',
                    target: 'Katherine Snyder',
                    href: '#',
                    date: 'Oct 4',
                    datetime: '2020-10-04',
                    iconBackground: 'bg-green-500',
                },
            ],
            build: {
                i1: 1,
                i2: 1,
                i3: 1,
                i4: 1,
                i5: 1,
                i6: 1,
                i7: 1,
                root: 1,
            },
            chartOptions: {
                chart: {
                    type: 'networkgraph',
                },
                // title: {
                //     text: 'The Indo-European Language Tree'
                // },
                // subtitle: {
                //     text: 'A Force-Directed Network Graph in Highcharts'
                // },
                plotOptions: {
                    networkgraph: {
                        keys: ['from', 'to'],
                        layoutAlgorithm: {
                            enableSimulation: true,
                            friction: -0.9
                        }
                    }
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