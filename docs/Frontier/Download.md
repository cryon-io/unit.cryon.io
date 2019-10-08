
<app-status> </app-status>

<script v-pre>


Vue.component('app-status', {
    data: function () {
        return {
            frontier_windows_url: "",
            frontier_linux_url: "",
            frontier_mac_url: "",
            latest: ""
        }
    },
    async mounted() {
        var css = `
            #download-grid {
                display: grid;
                grid-template-columns: 1fr 1fr 1fr;
                grid-row-gap: 10px;
                grid-template-rows: auto;
                grid-template-areas: "logo logo logo" "none1 title none2" "none1 download none2" "linux win mac"
            }
            @media only screen and (max-width: 767px) {
                #download-grid {
                    grid-template-columns: 1fr;
                    grid-template-areas: "logo" "title" "download" "win" "linux" "mac"
                }
            }

            .title {
                grid-area: title;
                text-align: center;
                margin-top: 0px !important;
                margin-bottom: 0px !important;
            }

            .download {
                grid-area: download;
                text-align: center;
                margin-top: 50px !important;
                color: var(--theme-color);
                margin-bottom: 50px !important;
            }

            .link-logo {
                width: 100px;
                height: 100px;
                display: flex;
                justify-self: center;
            }
            .link-name {
                display: flex;
                justify-self: center;
            }

            .link {
                text-decoration: none;
            }

            .link:hover{ 
                text-decoration: underline;
                cursor: pointer;
            }

            .link-disabled {
                filter: grayscale(100%);
                text-decoration: none !important;
                cursor: not-allowed !important;
            }

            .link-grid {
                display: grid;
                grid-template-columns: 1fr;
                grid-template-rows: auto auto;
            }

            .win {
                grid-area: win;
            }

            .linux {
                grid-area: linux;
            }
            .mac {
                grid-area: mac;
            }

            .frontier-logo {
                grid-area: logo;
                display: flex; 
                align-items: center; 
                justify-content:center
            }
        `,
        head = document.head || document.getElementsByTagName('head')[0],
        style = document.createElement('style');
        head.appendChild(style);

        style.type = 'text/css';
        style.id = 'frontierDownloadStyle'
        style.appendChild(document.createTextNode(css));

        const url = "https://api.github.com/repos/cryon-io/Unit-Frontier/releases/latest";
        const latest = await axios.get(url);
        if (latest && latest.data) {
            const data = latest.data;
            this.latest += ` v${data.tag_name}`
            //console.log(data.assets);
            for (const asset of data.assets) {
                if (asset && asset.name.match(/[Uu]nit.[Ff]rontier.*[Ww]in.*/)) {
                    this.frontier_windows_url = asset.browser_download_url;
                }
                if (asset && asset.name.match(/[Uu]nit.[Ff]rontier.*[Ll]inux.*/)) {
                    this.frontier_linux_url = asset.browser_download_url;
                }
                if (asset && asset.name.match(/[Uu]nit.[Ff]rontier.*[Mm]ac.*/)) {
                    this.frontier_mac_url = asset.browser_download_url;
                }
            }
        }
    },
    beforeDestroy() {
        document.getElementById("frontierDownloadStyle").outerHTML = "";
    },
    methods: {
        download_windows() {
            window.open(window.frontier_windows_url, '_blank')
        },
        download_linux() {
            window.open(window.frontier_linux_url, '_blank')
        },
        download_mac() {
            window.open(window.frontier_mac_url, '_blank')
        }
    },
    computed: {
        sortedApps() {
            return _.orderBy(this.apps, x => x.title)
        }
    },
    template: `
        <div ref="input"> 
            <div id="download-grid">
                <div class="frontier-logo">
                    <img src="assets/img/frontier-logo.png" width="125" />
                </div>
                <h4 class="download"> Download </h4>
                <h2 id="fontier_title" class="title">Unit Frontier {{latest}}</h2>
                <a class="linux link link-grid" @click="download_linux"><img class="link-logo" alt="for linux" src="/assets/img/linux.svg"/> <h4 class="link-name">Linux</h4></a>
                <a class="win link link-grid" @click="download_windows"><img class="link-logo" alt="for windows" src="/assets/img/win.svg"/> <h4 class="link-name">Windows</h4> </a>
                <a class="mac link link-grid link-disabled" disabled><img class="link-logo" alt="for mac" src="/assets/img/mac.svg"/>
                <h4 class="link-name">Mac</h4> </a>
            </div>
        </div>
    `
})
</script>