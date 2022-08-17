# How does a browser render HTML, CSS, JS to DOM? What is the mechanism behind it?

A browser requests for the files(HTML, CSS, JS) files from the server or have them present locally on the computer. Browser executes these files in certain order and presents them to our screens. Every browser has a very important component called browser engine which is relevant in this process. The name of Firefox's browser engine is **Gecko** and the Chrome's browser engine is **Blink**.

## Processing of the HTML file

Server responds to the browser's request for an HTML document by sending a binary stream file, which is typically a text file with a response header content-type set to text/html and charset set to UTF-8 indicating that it is an HTML document and is encoded in UTF-8. This information is used by the browser to convert the binary file into the readable text format.Without it, browser is unable to comprehend how to process the received files. Data is transferred in package of bytes, as computer works with the binary data. However, browser doesn't work with binary.

## Tokenisation

A browser still will not be able to do much with just having a set of characters. These texts would be meaningless to browser unless tokenised.
Browser interprets an HTML file by parsing it. During the process of tokenisation, browser takes note of all the start and end HTML tags. The parser is able to understand the strings within the angle brackets(<>) and associated rules with them. Parser can also differentiate among different tags.

## Conversion to nodes 

The tokens formed above are converted to the nodes which are objects with certain properties. Each node can be considered as a single entity which is used to form a tree data structure by linking those nodes. This combination of nodes is known as DOM(Document object model). Each node in the DOM has some sort of relationship with other nodes which could be:

* Parent-Child relationship
* Sibling relationship

The construction of DOM tree is the first process a browser needs to go through before anything else. 

## Fetching CSS file

In the HTML file, we most often have CSS file either linked externally, or using **style** tag in the **head** section of an HTML file, or by using **inline css**. When the browser is constructing the DOM, it simultaneously sends the request for the CSS file encountered in the **link** tag, which links to an external stylesheet or through other ways of fetching CSS. Throught this, browser fetches the bytes of CSS data. 

## Construction of CSSOM

This CSS data goes through the similar process of tokenisation and node formation as an HTML file and constructs its own tree which is known as CSSOM(Cascading style sheets object model), which contains the information regarding the styles for the elements in the HTML document. However, CSSOM doesn't target DOM elements which can't be printed like title, meta, script. Properties in CSS are inherited by the children in the CSSOM tree, which is the cascading behaviour of CSS. Some can be overridden as well.


## Render tree

Render tree is another tree which calculates the layout of each element and showing the element on to the screen. Render tree is a combination of both DOM and CSSOM. It contains the nodes which will eventually get printed on the screen. Anything that doesn't occupy the space on the page won't be present on the render tree. Consider a div with display property set to none. This element won't be present in the render tree as it doesn't occupy any space. However, elements with visibility: hidden or opacity: 0 will still be present in the tree as they do occupy space on the page.

## Sequence of rendering

1. Construction of DOM tree from the HTML file.

2. Construction of CSSOM tree from the CSS(external, inline or embedded).

3. Formation of render tree(elements to be displayed).

## Optimisation 

The first rule for the optimisation of website includes getting HTML and CSS to the client as soon as possible. So the HTML and CSS are render blocking the resources while they are being delivered.

## Introduction of Javascript

Javascript is fetched by the web browser, when it encounters **script** tag in the HTML file, which can be external or embedded. The problem with javascript is that it can add or remove the DOM tree elements as well as modify the CSS properties. When we introduce javascript earlier in the **head** section, it stops the DOM construction process until the script finishes, which can slow down the execution of a page and keep the users waiting. The best way to use javascript is after the DOM and CSSOM trees are formed. This can also be solved by using async keyword which will let the DOM construction to continue and execute scripts only when they are done downloading and are ready.

## References

https://blog.logrocket.com/how-browser-rendering-works-behind-scenes/


https://medium.com/jspoint/how-the-browser-renders-a-web-page-dom-cssom-and-rendering-df10531c9969