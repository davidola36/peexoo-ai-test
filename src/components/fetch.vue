<template>
    <div class="u-text-center">
        <button class="button" @click="fetchData"> Fetch and Save</button>
    </div>
</template>

<script>
import axios from 'axios'; 
export default {
    methods: {
        fetchData() {
            axios.get('http://data.fixer.io/api/latest?access_key=6716dd1b87333539300963a0c0c0f11b')
            .then(res=>{
                let headers = {
                    symbols: 'symbols',
                    prices: 'prices'
                }
                let rates = res.data.rates
                let result = []
                for( let keys in rates) {
                    let data = {}
                    data[keys] = keys
                    data[rates[keys]] = rates[keys]
                    result.push(data)
                }
                this.exportCSVFile(headers, result, 'assets')
            })
            .catch(err=>{console.log(err)})
        },
        convertToCSV(objArray) {
            var array = typeof objArray != 'object' ? JSON.parse(objArray) : objArray;
            var str = '';

            for (var i = 0; i < array.length; i++) {
                var line = '';
                for (var index in array[i]) {
                    if (line != '') line += ','

                    line += array[i][index];
                }

                str += line + '\r\n';
            }

            return str;
        },

        exportCSVFile(headers, items, fileTitle) {
            if (headers) {
                items.unshift(headers);
            }

            // Convert Object to JSON
            var jsonObject = JSON.stringify(items);

            var csv = this.convertToCSV(jsonObject);

            var exportedFilenmae = fileTitle + '.csv' || 'export.csv';

            var blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            if (navigator.msSaveBlob) { // IE 10+
                navigator.msSaveBlob(blob, exportedFilenmae);
            } else {
                var link = document.createElement("a");
                if (link.download !== undefined) { // feature detection
                    // Browsers that support HTML5 download attribute
                    var url = URL.createObjectURL(blob);
                    link.setAttribute("href", url);
                    link.setAttribute("download", exportedFilenmae);
                    link.style.visibility = 'hidden';
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                }
            }
        }
    }
}
</script>