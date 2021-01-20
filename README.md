# getting-started
Sign In/Create account 
on https://refer-customer-dashboard.vercel.app/
Copy your Client ID and Secret
In your backend application:
Install backend SDK: https://www.npmjs.com/package/avalanche-api
Method within avalanche-api 


var getApiToken = function (_a)..
   
  
is provided to you for fetching your token based on your Client ID and Secret. 
Recommended set-up:
Create a route in your backend that will use above function to fetch your tokens and send it to the client/UI,
Example:
app.get('/refer-api-auth', async (_, res) => {
 try{
   const token = await getApiToken()
   res.json(token)
 }catch{
   console.log('error')
 }
 
});
Easy (unsafe) set-up: you can use this to test the api quickly, but we don’t recommend using this in production
Install avalanche-api above to your client side UI and call that directly within your client side app

Install invite-your-friend dashboard into your front-end:
{showIframe && authToken &&
         <iframe
         sandbox="allow-top-navigation allow-scripts allow-same-origin allow-forms"
         height="500px"
         width="800px"
         src={`${REFERAPP_URL}?email=${email}&name=${name}&base_url=${APP_BASE_URL}&redirect_uri="http://localhost:3000/explore"&token=${authToken}`} />}



And use the following example to fetch your token


const useAuthToken = () => {
 const [authToken, setAuthToken] = useState('');
 async function setToken(){
   const token = await getApiToken();
   setAuthToken(token)
 }
 useEffect(() => {
   setToken()
 }, []);
 return {
   authToken,
   setAuthToken
 }
}
 










And getApiToken() is your func that fetches a token from your backend
 
export const getApiToken = async () => {
 const resp = await fetch(`${YOUR_BACKEND}/refer-api-auth`, {
   headers: {
     'Content-Type': 'application/json',
     'Accept': 'application/json'
       }
   }
 );
 const result = await resp.json();
 
 console.log('resp is ', result, result.token)
 localStorage.setItem('api_token', result.token);
 return result.token;
 }
 

Here email, name need to be provided in the URL parameters
APP_BASE_URL is your application’s url
REFERAPP_URL


const REFERAPP_URL = 'https://refer-ui-two.vercel.app/'
 
Optional: “friends-you-invited’ dashboard
 
{showIframe && authToken &&
 <iframe
 sandbox="allow-top-navigation allow-scripts allow-same-origin allow-forms"
 height="500px"
 width="800px"
 src={`${REFERAPP_URL}/referrals?email=${email}&token=${authToken}`} />}
 
 	
REFERAPP_URL is as before, and method for retrieving authToken is as is before
Tracking

Install front-end SDK
https://www.npmjs.com/package/avalanche-browser

In your index.html on your front-end (landing page)
if(ref_code){
       document.cookie = `refAPI_ref_code=${ref_code}`;
     }

-we need this to grab their referral code, in case user leaves the site and comes back later foro registration

Sign up track
Wherever your sign up, inside of your sign up function call
 
 
     const token = await getApiToken();
    
     signUpMyAppSdk({ email, authReferApiToken: token });
 
    

where signUpMyAppSdk comes from 
https://www.npmjs.com/package/avalanche-browser
And getApiToken() is a method we’ve already defined in the previous section when we were making our iframes

Premium event track
You can designate any event as your referral reward milestone - wherever it happens, drop the following function inside or right after. Token is your JWT token - 

    
     const authReferApiToken = await getApiToken();
     const result = await premiumEventMyAppSdkV2({ authReferApiToken,email });
    




You are done!
Next - test the referral flow.

NOTE 1: emails will come through from my email account, but any email you provide can be substituted in

NOTE 2: you can access all of customers that have been referred to your app, and all of their referrers by creating a get request to the following end point:

salty-reef-38656.herokuapp.com/events/all_referred_users

And provided a query parameter clientId that you got after sign up in the first section.


