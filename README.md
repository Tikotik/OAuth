# F# OAuth
OAuth library written in F# with KISS principle in mind so it is simple stupid.

## Sample of Use: Goodreads
Define provider urls:
 
```fsharp

let requestTokenUrl = "http://www.goodreads.com/oauth/request_token"
let accessTokenUrl = "http://www.goodreads.com/oauth/access_token"
let authorizeUrl = sprintf "https://www.goodreads.com/oauth/authorize?oauth_token=%s&oauth_callback=%s"

//1. get authorization url
let getAuthorizationData clientKey clientSecret callbackUrl =
    let (token, tokenSecret) = getAuthorizatonToken clientKey clientSecret requestTokenUrl
    (authorizeUrl token callbackUrl, token, tokenSecret) 

//2. after authorization get access token for actual user
let getAccessToken clientKey clientSecret token tokenSecret =
    getAccessToken clientKey clientSecret accessTokenUrl token tokenSecret

//3. build access data
let getAccessData clientKey clientSecret token tokenSecret =
    getAccessData clientKey clientSecret token tokenSecret

//4. use access data for api calls - eg. info about actual user 
let getUser accessData =
    let userResponse = getUrlContentWithAccessData "https://www.goodreads.com/api/auth_user" accessData
    let user = User.Parse userResponse
    user.User

```

