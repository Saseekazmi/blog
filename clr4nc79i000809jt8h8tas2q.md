---
title: "Global Error Handling in Next.js 13 App Router API"
seoTitle: "Global Error Handling in Next.js 13 App Router API"
seoDescription: "Learn how to implement global error handling in Next.js 13 App Router API routes."
datePublished: Sat Sep 09 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clr4nc79i000809jt8h8tas2q
slug: global-error-handling-in-nextjs-13-app-router-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704701302763/5a7f8a5c-7b52-403c-b45a-28896b7d7210.png
tags: error-handling, nextjs, nextjs-134, app-router

---

Next.js 13 has introduced a new **App Router** that enables developers to leverage React's latest features, including Server Components. I have been exploring this new router and building a proof of concept that serves both UI and API routes.

However, I faced challenges when trying to implement global error handling in API routes. In this article, I would like to share my experience and how I eventually found a solution to this problem.

When I started using Next.js 13 app routes to develop backend APIs, I assumed that we could handle errors globally using middleware as we do in Node.js. But this approach didn't work as expected because Next.js middlewares are used for specific tasks like redirection, not error handling for API routes.

The official Next.js documentation lacks information on handling errors globally in API routes. Additionally, since this app router is relatively new, it was challenging for me to find a solution on Google.

I decided to find a solution on my own. My initial approach was to wrap the code in each API route with a try-catch block. However, this method required me to repeat the same try-catch block for all the API routes, which was not an efficient solution.

So, I wrapped the API logic in a handler function and chained it with the error handler function. It made the code cleaner and reduced the number of lines of code required.

```javascript
import { NextResponse } from "next/server";
import { errorHandler, validator } from "@/util"

const POST = (req) => {
    const handler = async (req) => {
        const user = await req.json();
        await validator(req);        
        const res = await createAccount(user);
        return NextResponse.json(res);
    }
    return errorHandler(req)(handler);
}

export { POST };
```

```javascript
/* @util */
const validator = (req)=>{
    //custom validtion logic goes here...
}

const  errorHandler = (req) => async (handler) => {
  try {
    return await handler(req);
  } catch (err) {
    //custom error Handler logic goes here...
    return NextResponse.json(errorMessage, { status: statusCode });
   }
}

export { errorHandler, validator };
```

Finally, I did it! If you ever find yourself in a situation like mine, don't hesitate to use what I've learned. It could make all the difference!

By developing this POC, I have come to realize that Next.js is primarily geared towards enhancing the front-end development experience. The community surrounding Next.js is also more focused on front-end development and may not offer as much support or tooling for backend-specific tasks.

Therefore, if your project requires more extensive backend functionality, it may be worth considering using Next.js in conjunction with a dedicated backend framework such as Node JS.

Thank you for reading! I appreciate your time and hope you found this post valuable. Stay tuned for more content, and feel free to reach out with any questions or suggestions.