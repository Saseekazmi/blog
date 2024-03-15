---
title: "Auth0-NextJs SDK: Learn to redirect users to the same page post-logout using cookies"
seoTitle: "Auth0-NextJS SDK: Learn to redirect users to the same page post-logout"
datePublished: Fri Feb 09 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cltsnm0db000h0akz5f1k2anz
slug: auth0-nextjs-sdk-learn-to-redirect-users-to-the-same-page-post-logout-using-cookies
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1710506342604/e5a87a96-172d-418d-a5a3-56b3dc64495f.png
tags: cookies, authentication, auth0, nextjs, logout, nextauthjs

---

Hello! I hope you are doing well. I have been working on a Next.js project that uses Auth0 to securely manage user flows. It has multiple sections and routes where users can log in or log out. In the existing implementation, when users log out, they are redirected to the homepage. I want to redirect users to the same page or route from which they initiated the logout.

### **Do you think there is a simple and straightforward solution in the Auth0-Next.js SDK?**

The answer is both **YES AND NO!**

**Yes** - It is simple if you have a default logout page, and want to redirect to the homepage.

**No** - If you have dynamic logout URLs.

With the Auth0-Next.js SDK, logout URLs must be specified in the Auth0 dashboard settings. Users can only be redirected to the URLs specified on the dashboard, and you will encounter errors if you attempt to redirect to any other page after logging out.

### **How did I manage to fix it?**

Using a browser *Cookies* 🍪

Here are the steps I took:

1. When users click on the sign-out button, a new cookie(`returnTo:"<Current page URL>`") will be set on their browser.
    
2. I created a new redirect route ([www.saseek.com/redirect](http://www.saseek.com/redirect)) and added it to the allowed logout URL for the application in the Auth0 dashboard.
    
3. Auth0 will redirect users to the new redirect page upon successful logout and check whether the cookie is present on their browser.
    
4. If the cookie is present, users will be redirected to the URL stored in that cookie and that cookie will be destroyed.
    
5. If it's not present, they will be redirected to the homepage.
    
6. I also added a loading indicator to display until the redirection is completed.
    

### Actual Code implementation for reference

```javascript
import React from 'react';
import Cookies from 'js-cookie'

import Loader from './Loader';

const LogoutRedirect = (props) => {
  const updateReturnTo = (returnTo) => {
      let updatedUrl = new URL(returnTo);
	  let stringUrl = updatedUrl.toString();
      return stringUrl;
  }

  React.useEffect(() => {
    const hasReturnToCookie = Cookies.get('returnTo');
    Cookies.remove('returnTo', { domain: window.location.hostname });
    window.location.href = hasReturnToCookie ? updateReturnTo(hasReturnToCookie) : '/';
    
  }, []);

  return <Loader loaderText='Redirecting...' />
};

export default LogoutRedirect;
```

And... that's it! Now I can redirect users to any page you want without any issues 🙌🏼

If you've reached this point, thank you very much. I hope that this article has been helpful to you and I'll see you all in the next.