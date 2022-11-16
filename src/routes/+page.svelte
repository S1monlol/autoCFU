
<script>
let files;

   
    // import pdfjs from static host 
    async function getContent(src) {
        const doc = await pdfjsLib.getDocument(src).promise // note the use of the property promise

        

        const numPages = doc.numPages

        let content = []
        let page

        for(let i = 1; i < numPages + 1; i++) {

            page = await doc.getPage(i)

            // add page content to content array
            content = content.concat((await page.getTextContent())['items'])

            


        }



        
        return await content
    }
    


$: if (files) {
        console.log(files[0]);

        let x = window.URL.createObjectURL(files[0]);    

        let w = getContent(x);

        let questions = []

        // console log output of w
        w.then((data) => {


            console.log(data)
            let question 
            let check = data.filter((item) => {
                // if (item.str.match(/\d\./) != null), get next item
                if(item.str.match(/\d\./)) {
                    
                    question = {}

                    question.q = item.str
                    question.q += data[data.indexOf(item) + 2]['str']
                    for(let i = 1; i < 5; i++){
                        // if data[data.indexOf(item) + i doesnt start with the letter a
                        
                        
                        if(!data[data.indexOf(item) + i].str.match(/a\./)){
                            question.q += data[data.indexOf(item) + i]['str']    
                        } else {
                            question.a = data[data.indexOf(item) + i + 2]['str']
                            question.b = data[data.indexOf(item) + i + 6]['str']
                            question.c = data[data.indexOf(item) + i]['str']
                            question.d = data[data.indexOf(item) + i]['str']

                        }
                    }

                    
                }

                // add if question doesnt already exist
                if(!questions.includes(question) && question != undefined){
                    questions.push(question)
                }
            });
            // get question
            console.log(questions)
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