# slack-black-gold-theme
A black and gold theme inspired by the New Orleans Saints team colors for the Slack MacOS client. Based on the [slack-dark-mojave-theme](https://github.com/elv1n/slack-dark-mojave-theme).

# Preview    
![Screenshot](https://raw.githubusercontent.com/elv1n/slack-dark-mojave-theme/master/preview.png)

# Installing into Slack    
 **NB:** You'll have to do this every time Slack updates.    
    
### Option 1    
 Find your Slack's application directory.    
    
* Windows: `%homepath%\AppData\Local\slack\`
* Mac: `/Applications/Slack.app/Contents/`
* Linux: `/usr/lib/slack/` (Debian-based)    
    
For versions after and including `3.0.0` code must be added to the following file `resources/app.asar.unpacked/src/static/ssb-interop.js`
    
 At the very bottom, add one    
    
```js    
// First make sure the wrapper app is loaded    
document.addEventListener("DOMContentLoaded", function() {    
    // Then get its webviews    
    const webviews = document.querySelectorAll(".TeamView webview");    
      
    // Fetch our CSS in parallel ahead of time    
    const cssPath = 'https://raw.githubusercontent.com/cPanelRicky/slack-black-gold-theme/master/style.css';    
    const cssPromise = fetch(cssPath).then(response => response.text());    
    
    // Insert a style tag into the wrapper view  
    cssPromise.then(css => {  
      let s = document.createElement('style');  
      s.type = 'text/css';  
      s.innerHTML = css;  
      document.head.appendChild(s);  
    });  
    
    // Wait for each webview to load    
    webviews.forEach(webview => {    
      webview.addEventListener('ipc-message', message => {    
         if (message.channel == 'didFinishLoading')    
            // Finally add the CSS into the webview    
            cssPromise.then(css => {    
               const script = `    
                     let s = document.createElement('style');    
                     s.type = 'text/css';    
                     s.id = 'slack-dark-mojave-css';    
                     s.innerHTML = \`${css}\`;    
                     document.head.appendChild(s);    
                     `    
               webview.executeJavaScript(script);    
            })    
      });    
    });    
});    
```
#license

MIT
