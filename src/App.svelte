<script>
  let tabsandiframes = [];
  let inputurl;
  let nextid = 1;
  let tabOrder = [];
  function isUrl(val = "") {
    if (
      /^http(s?):\/\//.test(val) ||
      (val.includes(".") && val.substr(0, 1) !== " ")
    )
      return true;
    return false;
  }

  function cloak() {
		let inFrame
		try {
			inFrame = window !== top
		} catch (e) {
			inFrame = true
		}
		if (!inFrame && !navigator.userAgent.includes("Firefox")) {
			const popup = open("about:blank", "_blank")
			if (!popup || popup.closed) {
				alert("Popups are disabled!");
				var link = document.querySelector("link[rel*='icon']") || document.createElement('link');
    link.type = 'image/x-icon';
    link.rel = 'shortcut icon';
    link.href = 'https://ssl.gstatic.com/classroom/favicon.png';
	document.title = 'Google Classroom';
    document.getElementsByTagName('head')[0].appendChild(link);
			} else {
				const doc = popup.document
				const iframe = doc.createElement("iframe")
				const style = iframe.style
				const link = doc.createElement("link")
				doc.title = "Loading..."
				link.rel = "icon";
				link.href = "https://ssl.gstatic.com/classroom/favicon.png";
				iframe.src = location.href
				style.position = "fixed"
				style.top = style.bottom = style.left = style.right = 0
				style.border = style.outline = "none"
				style.width = style.height = "100%"
				doc.body.appendChild(iframe)
				location.replace("https://schoology.com")
			}
		}
	  }
  
  function popOut() {
    var win = window.open();
    var url = document.querySelector('iframe.active').src;
    var iframe = win.document.createElement('iframe');
    iframe.style.width = "100%";
    iframe.style.height = "100%";
    iframe.style.border = "none";
    iframe.src = url;
    win.document.body.style.border = "0px";
    win.document.body.style.margin = "0px";
    win.document.body.style.padding = "0px";
    win.document.body.appendChild(iframe);
  }

  function go(value) {
    let url = value.trim();
    if (!isUrl(url)) url = "https://www.google.com/search?q=" + url;
    else if (!(url.startsWith("https://") || url.startsWith("http://")))
      url = "https://" + url;
    let activeIframe = document.querySelector("iframe.active");
    url = __uv$config.encodeUrl(url);
    activeIframe.style.display = "block";
    activeIframe.src = "/go/" + url;
    inputurl = __uv$config.decodeUrl(url);
  }
  function closeTabAndIframe(id) {
    tabsandiframes.splice(tabsandiframes.indexOf(id), 1);
    tabsandiframes = tabsandiframes;
    tabOrder.splice(tabOrder.indexOf(id), 1);
    tabOrder = tabOrder;
    document.getElementById("tab" + tabOrder.slice(-1)[0]).click();
  }
  let tabs = [];
  let iframes = [];


</script>

<div id="topbar">
  <div id="tabsandmore">
    <div id="tabs">
      {#each tabsandiframes as tabandiframe, index (tabandiframe)}
        <button
          class="tab"
          id={"tab" + tabandiframe}
          on:click={() => {
            if (tabOrder.indexOf(tabandiframe) > -1) {
              tabOrder.splice(tabOrder.indexOf(tabandiframe), 1);
            }
            tabOrder[tabOrder.length] = tabandiframe;

            let tab = document.querySelector("#tab" + tabandiframe);
            let iframe = document.getElementById(tabandiframe);
            tabs.forEach((elmnt) => {
              elmnt.classList = "tab";
            });
            iframes.forEach((elmnt) => {
              elmnt.classList = "";
              elmnt.style.display = "none";
            });
            tab.classList.add("active");
            iframe.classList.add("active");

            if (iframe.src !== "") {
              iframe.style.display = "block";
            } else {
              iframe.style.display = "none";
            }
            let url = iframe.src.split(__uv$config.prefix)[1];
            inputurl = __uv$config.decodeUrl(url);
          }}
          bind:this={tabs[index]}
        >
          <img
            class="favicon"
            alt="new tab icon"
            src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAABEElEQVR4nO2ZwWrCQBRFj0KC/oe/2NpV0U9w1ZX+Qb+i3Xdf/ZFK4ElgCsNAMDOaPIP3wNvlJfdyJ1ncgBBCiIFYAlvgBDSAjTxNePYmaMkW/+0g2jqm1bLIMbB9ANGWzHuOgVO0+AJUV65fAZ/JA/fAnHIq4DW632/Ocnzm65478yD6nibq5J3oTSwihxnwkewfbjRhJVpKDXQlUTJn4MfDQFcS95jRDPwnsQP+pmrgVqrkKzQ5A+lXaJIGWmTAG9MRcsaUgDOmBJwxJeCMKQFnTAk4Y0rAGXvKBJqCYmsI6tJiK64W104mauCttFrcDNDpjFruLh+sXv/KrdcJC63ro+MPjmPQkC1eCCEEfbgAuJ68dbQb+kgAAAAASUVORK5CYII="
          />
          <p>New tab</p>
          <button
            class="tabclose"
            on:click={(event) => {
              event.stopPropagation();
              closeTabAndIframe(tabandiframe);
            }}
          >
            <i class="fa-regular fa-circle-xmark" />
          </button>
        </button>
      {/each}
    </div>
    
    <div id="settingsandnewtab">
<button on:click={() => popOut()}>
        <i class="fa-solid fa-copy" />
      </button>
<button on:click={() => go('https://discord.gg/goabyss')}>
        <i class="fa-brands fa-discord" />
      </button>
       <button on:click={() => cloak()}>
<i class="fa-solid fa-masks-theater"></i>
      </button>
      <button
        id="newtabbutton"
        on:click={() => {

          let newnextid = nextid;
          let newtabsandiframes = [...tabsandiframes, newnextid];
          tabsandiframes = newtabsandiframes;
          nextid = nextid + 1;
          function checkIfTabExists() {
            if (document.querySelector("#tab" + newnextid) == null) {
              window.setTimeout(checkIfTabExists, 50);
            } else {
              document.querySelector("#tab" + newnextid).click();
              
              go("https://google.com");

            }
          }
          checkIfTabExists();
        }}
      >
        <i class="fa-solid fa-plus" />
      </button>
    </div>
  </div>
  <form on:submit|preventDefault={() => go(inputurl)} id="urlbar">
    <input
      bind:value={inputurl}
      placeholder="Search or type a URL"
      type="text"
    />
  </form>
</div>

{#each tabsandiframes as tabandiframe, index (tabandiframe)}
  <iframe id={tabandiframe} title="iframe" src="" bind:this={iframes[index]} />
{/each}

<svelte:window
  on:load={() => {
    document.querySelector("#newtabbutton").click();
    window.navigator.serviceWorker.register("./sw.js", {
      scope: __uv$config.prefix,
    });
  }}
/>
