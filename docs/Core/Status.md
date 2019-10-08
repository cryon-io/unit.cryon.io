# Latest Available Versions

## App Versions

<app-list> </app-list>
* Availability is availability of daemon files

<script v-pre>

async function getLatestRelease(repository, fileRegex) {
    const url = `https://api.github.com/repos/${repository}/releases/latest`;
    const latest = await axios.get(url);
    if (latest && latest.data) {
        const data = latest.data;
        let available = `Not file matching ${fileRegex}`;
        for (const asset of data.assets) {
            if (Array.isArray(fileRegex)) {
                let _matched = true;
                for (const reg in fileRegex) {
                    if (!asset || !asset.name.match(reg)) {
                        _matched_ = false;
                        break;
                    } 
                }
                if (_matched) {
                    available = true
                }
            } else {
                if (asset && asset.name.match(fileRegex)) {
                    available = true;
                    break;
                } 
            }
        }
        return { available: available, version: data.tag_name };
    }
}
Vue.component('app-version', {
    props: [ 'title', 'repository', 'available' ],
    data: function () {
        return {
            version: '',
            available: true
        }
    },
    async created() {
        this.version = await getLatestRelease(this.repository);
    },
    template: `
        <div style="grid-column: 1/2">{{title}}</div>
        <div style="grid-column: 2/3">{{ version }}</div>
        <div style="grid-column: 3/4">{{ available == true ? "available" : "failure"  }}</div>
    `
})

Vue.component('app-list', {
    data: function () {
        return {
            apps: [
                { title: "SnowGem", repository: "Snowgem/Snowgem", fileRegex: /ubuntu18\.04/ },
                { title: "Ether1 SN/MN", repository: "Ether1Project/Ether-1-SN-MN-Binaries" },
                { title: "Ether1 GN", repository:"Ether1Project/Ether-1-GN-Binaries" },
                { title: "Gincoin", repository: "GIN-coin/gincoin-core", fileRegex: /binaries.*linux.*64bit/ },
                { title: "Crown", repository: "Crowndev/crown-core", fileRegex: /Crown.*Linux.*64\.zip/ },
                { title: "Ulead", repository: "uleadapp/ulead", fileRegex: /x86_64.*linux/ },
                { title: "Horizen SN", special: true, note: "apt based" },
                { title: "Airwire", repository: "AirWireOfficial/wire-core", fileRegex: [ /x86_64-linux/, /(?!.*qt.*).*/ ] },
                { title: "Xerom", repository: "xero-official/go-xerom", fileRegex: /geth.*linux/ }        
            ],
            style: {
                display: 'grid',
                "grid-template-columns": "1fr 1fr 1fr"    
            }
        }
    },
    async mounted() {
        const reflect = p => p.then(v => ({v, status: "fulfilled" }),
                            e => ({e, status: "rejected" }));
        const appPromises = [];
        for (app of this.apps) {
            if (app.special) 
                appPromises.push(Promise.resolve({available: app.note, version: app.note}));
            else
                appPromises.push(getLatestRelease(app.repository, app.fileRegex));
        }
        const results = await Promise.all(appPromises.map(reflect))
        
        for (let i = 0; i < results.length; i++) {
            const result = results[i]
            const app = this.apps[i]
            if (status === 'rejected') {
                app.version = 'unknown'
                app.available = 'unknown'
                this.$set(this.apps, i, app)
                
            } else {
                
                app.version = result.v.version
                app.available = result.v.available
                this.$set(this.apps, i, app)
            }
        }
    },
    computed: {
        sortedApps() {
            return _.orderBy(this.apps, x => x.title)
        }
    },
    template: `
        <div class="app-grid" :style="style">
            <div style="grid-column: 1/2; text-decoration: underline;">App</div>
            <div style="grid-column: 2/3; text-decoration: underline;">Version</div>
            <div style="grid-column: 3/4; text-decoration: underline;">Availability</div>
            <template v-for="app of sortedApps">
                <div style="grid-column: 1/2">{{ app.title}}</div>
                <div style="grid-column: 2/3">{{ app.version }}</div>
                <div style="grid-column: 3/4">{{ app.available == true ? "available" : app.available }}</div>
            </template>

        </div>`
})
</script>