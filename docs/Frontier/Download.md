<div id="download-grid">
    <div class="frontier-logo">
        <img src="assets/frontier-logo.png" width="125" />
    </div>
    <h4 class="download"> Download </h4>
    <h2 id="fontier_title" class="title">Unit Frontier</h2>
    <a class="linux link link-grid link-disabled" disabled><img class="link-logo" alt="for linux" src="/assets/linux.svg"/> <h4 class="link-name">Linux</h4></a>
    <a class="win link link-grid" onclick="download_windows()"><img class="link-logo" alt="for windows" src="/assets/win.svg"/> <h4 class="link-name">Windows</h4> </a>
    <a class="mac link link-grid link-disabled" disabled><img class="link-logo" alt="for mac" src="/assets/mac.svg"/>
    <h4 class="link-name">Mac</h4> </a>
</div>

<script>
async function setDownloadUrls() {
    const url = "https://api.github.com/repos/cryon-io/Unit-Frontier/releases/latest";
    const latest = await axios.get(url);
    if (latest && latest.data) {
        const data = latest.data;
        document.getElementById("fontier_title").innerText += ` v${data.tag_name}`
        //console.log(data.assets);
        for (const asset of data.assets) {
            if (asset && asset.name.match(/Unit\.Frontier.*/)) {
                window.frontier_windows_url = asset.browser_download_url;

                break;
            }
        }
    }
}
setDownloadUrls();

window.download_windows = async function() {
    window.open(window.frontier_windows_url, '_blank')
}
</script>

<style>
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
    margin-top: 0px !important;
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
</style>