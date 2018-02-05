I use this repo to statically host something very temporary.

The page on my server just inserts whatever is here into the page, like this, basically:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hosted</title>
</head>
<script>
"use strict"
// Get the latest version of the stuff
fetch("https://raw.githubusercontent.com/DanRuta/hosting/master/index.html")
.then(r => r.text())
.then(s => {
    document.body.innerHTML = s

    // Re-insert the script elements as elements, not as text, so they get evaled
    Array.from(document.body.querySelectorAll("script")).forEach(script => {
        const scriptElem = document.createElement("script")

        if (script.src) {
            scriptElem.src = script.src
        } else {
            scriptElem.innerHTML = script.innerHTML
        }

        script.parentNode.insertBefore(scriptElem, script)
        script.remove()
    })
})
</script>
<body>
</body>
</html>
```

Yes, there are easier alternatives, out there.