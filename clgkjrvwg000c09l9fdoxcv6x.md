---
title: "How to pass custom parameters to the auth0 post-login actions using React SDK."
seoTitle: "How to pass feature flags to the auth0 actions from the react SDK"
datePublished: Mon Apr 17 2023 08:01:40 GMT+0000 (Coordinated Universal Time)
cuid: clgkjrvwg000c09l9fdoxcv6x
slug: how-to-pass-custom-parameters-to-the-auth0-post-login-actions-using-react-sdk
tags: auth0, auth0-actions, auth0-react-sdk

---

Hi Folks!!! Hope you all are doing great. I was bumping my head into the desk when I try to figure out a way to pass custom parameters to the auth0 actions.

Here's my use case. I want to execute a post-login action that sets a custom picture for users on their user metadata on their first login. But I want to execute this action only for certain apps on my auth0 tenant with a feature flag.

Since there is no official documentation available on the auth0 page or in the community, I have no other choice but to find a solution by trial and error method. Luckily, I succeeded in that on my third attempt.

In case, If you wonder what auth0 action is, and how it works, Here's the well-explained official [documentation](https://auth0.com/blog/introducing-auth0-actions/).

Now, let's see how to pass and retrieve feature flags in actions.

**Step 1:** Add feature flags/Custom params to the Auth0 provider as mentioned in the below code.

```javascript
     <Auth0Provider
        domain="your domain id"
        clientId="your app client id"
        authorizationParams={{redirect_uri: window.location.origin}}
        setCustomPicture="true"
        CustomParams="x" 
      >
        <App />
      </Auth0Provider>
```

**Step 2:** Login into your auth0 tenant dashboard and Select **Actions -&gt; Flows -&gt; Login -&gt; Add Action -&gt;Build Custom.** Create Action prompt will open, Enter the action name and select Login/Post Login trigger and click on Create button.

**Step 3:** You can retrieve all custom params passed from auth0SDK from `event.request.query` object and user\_metadata from the `event.user` object.

```javascript
function getCustomPicture(email){
   // custom function logic to fetch user picture using mail.
   //returns picture for a user
}

exports.onExecutePostLogin = async (event, api) => {
 const {setCustomPicture,CustomParams1} = event?.request?.query || {};
 console.log(setCustomPicture, CustomParams1); // output: true x y
 const{ email, user_metadata } = event.user;

 if(!user_metadata?.custom_picture && setCustomPicture === "true"){
    let picture = getCustomPicture(email);
    api.user.setUserMetadata("custom_picture", picture);
  }
}
```

**Step 4:** Deploy your action and application again and verify that your action produces desired results.

### **Notes:**

If you want to debug the actions, use the console logs and enable the [Real-time Webtask Logs](https://auth0.com/docs/customize/extensions/real-time-webtask-logs) extension on your tenant.