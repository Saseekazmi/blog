## Asynchronous Nature of React useState()

Recently, I came to know that the **useState() hook** in react component is **asynchronous **in nature.

I was working on a react application that displays random quotes whenever a new quote button is clicked. Initially, I was unaware of the fact that useState() is asynchronous and got an undefined properties error.

 Let's see how I fixed those errors in this article.

**Here's my initial solution which was having an error.**

```
import React, { useEffect, useState } from "react";
import axios from "axios";
import "./App.css";

export default function App() {
  const [quotesArr, setQuotesArr] = useState([]);
  const [currentQuote, setCurrentQuote] = useState({ text: "", author: "" });

  useEffect(() => {
    const getQuotesArr = async () => {
      const { data } = await axios.get(
        "https://gist.githubusercontent.com/camperbot/5a022b72e96c4c9585c32bf6a75f62d9/raw/e3c6895ce42069f0ee7e991229064f167fe8ccdc/quotesArr.json"
      );
      setQuotesArr(data.quotes);
      console.log(quotesArr); // this will log empty array to the console😒
      generateRandomQuote();
    };
    getQuotesArr();
  }, []);

  const generateRandomQuote = () => {
    console.log(quotesArr, quotesArr.length); //got output as [], 0 😑
    const randomQuoteId = Math.floor(Math.random() * quotesArr.length);
    setCurrentQuote(quotesArr[randomQuoteId]); 
  };

  return (
    <div id="quote-box">
      <div id="text">{currentQuote.quote}</div>
      <div id="author">-{currentQuote.author}</div>
      <div id="links">
        <a id="tweet-quote" target="_blank" href="twitter.com/intent/tweet">
          <i className="fa-brands fa-twitter"></i>
        </a>
        <button id="new-quote" onClick={generateRandomQuote}>
          New Quote
        </button>
      </div>
    </div>
  );
}


``` 

I was trying to set a quote from quotesArr state to currentQuote immediately after quotesArr state was initialized. 

I created generateRandomQuote() for this purpose. I was expecting it to generate a random number to fetch a quote from quotesArr state. But unfortunately, quotesArr is coming as an empty array and got below the below error in the console.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1645126401118/U38sLkeDH9.png)

I called generateRandomQuote() only after initializing quotesArr with API response. Then, how come its value became empty inside that function? 

Later, I googled and found that useState() is asynchronous and we should not access state immediately after initializing. 

To fix this issue, I moved generateRandomQuote() to the new useEffect which will execute whenever quotesArr state is modified.

**And, Here's my final solution **

```
import React, { useEffect, useState } from "react";
import axios from "axios";
import "./App.css";

export default function App() {
  const [quotesArr, setQuotesArr] = useState([]);
  const [currentQuote, setCurrentQuote] = useState({ quote: "", author: "" });

  useEffect(() => {
    const getQuotesArr = async () => {
      const { data } = await axios.get(
        "https://gist.githubusercontent.com/camperbot/5a022b72e96c4c9585c32bf6a75f62d9/raw/e3c6895ce42069f0ee7e991229064f167fe8ccdc/quotes.json"
      );
      setQuotesArr(data.quotes);
    };
    getQuotesArr();
  }, []);

  useEffect(() => {
    //Validation to return null if quotesArr is empty. 
    if (!quotesArr.length) {
      return null;
    }
    generateRandomQuote();
  }, [quotesArr]);

  const generateRandomQuote = () => {
    const randomQuoteId = Math.floor(Math.random() * quotesArr.length);
    setCurrentQuote(quotesArr[randomQuoteId]);
  };

  return (
    <div id="quote-box">
      <div id="text">{currentQuote.quote}</div>
      <div id="author">-</div>
      <div id="links">
        <a id="tweet-quote" target="_blank" href="twitter.com/intent/tweet">
          <i className="fa-brands fa-twitter"></i>
        </a>
        <button id="new-quote" onClick={generateRandomQuote}>
          New Quote
        </button>
      </div>
    </div>
  );
}

``` 

If you've reached this point, thank you very much. If you’ve found this article useful, please do consider telling your friends about it. I'll see you all in the next 😊