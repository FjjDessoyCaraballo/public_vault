# What is N8N

Simple no-code/low-code platform to automate tasks. There are many ways of automating whatever possible task you may have, but for this document I will focus on a simple scraper.

## Simple scraper

The scraper will:

1. Make an HTTP GET request to `https://www.example.com`;
2. Format the file from XML to JSON format;
3. Parse the `body` out of the JSON file;
4. Save the result into a text file;

![N8N Scraper Workflow](Pasted%20image%2020251002151501.png)

### HTTP request

The HTTP GET request is quite simple and does not need any strategies for circumvention, such as headers and made up cookie sessions.

![HTTP Request Configuration](Pasted%20image%2020251002151819.png)

The result of that request will be the HTML body in XML format.

### Format the file

This part of the process is quite straightforward too. The only necessary thing it to select the mode `XML to JSON`.

![XML to JSON Conversion](Pasted%20image%2020251002151954.png)

### Parse the body

This steps only requires you to declare an object that contains the `p[0]`, which is the body. After that you will have a JavaScript object that can be extracted to text form.

![Parse Body Step](Pasted%20image%2020251002152231.png)


### Convert to text file

Another rather simple step that only requires you to reference the paragraph using dot notation to access the JavaScript object. After that you just need to select in mode `Convert to Text File` and your file is ready for download.

![Convert to Text File](Pasted%20image%2020251002152952.png)