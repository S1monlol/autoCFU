
<script>
let files;

   
    // import pdfjs from static host 
    async function getContent(src) {
        const doc = await pdfjsLib.getDocument(src).promise // note the use of the property promise
        const page = await doc.getPage(1)
        
        return await page.getTextContent()
    }
    


$: if (files) {
        console.log(files[0]);

        let x = window.URL.createObjectURL(files[0]);    

        let w = getContent(x);
        // console log output of w
        w.then((data) => {


            // find check for understanding 
            let check = data.items.filter((item) => {
                return item.str.includes("Blackbody Radiation");
            });
            console.log(check)
            console.log(data)
        })
    };

</script>

<svelte:head>
    <script src="//mozilla.github.io/pdf.js/build/pdf.js"></script>
</svelte:head>

<body>

    

    <!-- input for pdf upload -->
    <input
	bind:files
	id="one"
	type="file"
    />
</body>